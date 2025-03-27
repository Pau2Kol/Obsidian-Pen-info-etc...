![[Pasted image 20250122094109.png]]


https://3os.org/penetration-testing/cheatsheets/gobuster-cheatsheet/#global-flags

Outil pour brute-forcer les répertoires et noms de fichiers sur les serveurs web

# Mode directory
gobuster dir -u http://example.com -w /usr/share/wordlists/dirb/common.txt

# Mode DNS
gobuster dns -d example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

# Mode vhost
gobuster vhost -u http://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

**Paramètres importants:**

- `-t` : Nombre de threads (défaut: 10)
- `-x` : Extensions à rechercher (ex: php,txt,html)
- `-b` : Statuts HTTP à ignorer
- `-c` : Cookie à utiliser

**Cas d'utilisation:**

- Découverte de répertoires cachés
- Énumération de sous-domaines
- Recherche de fichiers sensibles

**Ressource:** [Gobuster Cheat Sheet](https://3os.org/penetration-testing/cheatsheets/gobuster-cheatsheet/#global-flags)

## Bonnes pratiques de reconnaissance

1. **Commencer par la reconnaissance passive** avant de passer à des méthodes actives
2. **Documentez systématiquement** vos découvertes
3. **Limitez la bande passante** lors de scans pour éviter les détections
4. **Respectez le cadre légal** - obtenez toujours les autorisations nécessaires
5. **Corrélation des informations** entre différentes sources pour une meilleure précision

## Ressources recommandées

- [OSINT Framework](https://osintframework.com/)
- [SecLists](https://github.com/danielmiessler/SecLists) - Collections de wordlists
- [SANS OSINT Cheat Sheet](https://www.sans.org/blog/list-of-resource-links-from-open-source-intelligence-summit-2019/)

#reconnaissance #osint #outil