
## Outils de Reconnaissance Active

### Nmap

_Outil de cartographie réseau et de découverte de services_![[Pasted image 20250122093433.png]]https://www.stationx.net/nmap-cheat-sheet/

# Scans de base
nmap 192.168.1.1                      # Scan de base
nmap -p 1-1000 192.168.1.1            # Scan des ports 1-1000
nmap -p- 192.168.1.1                  # Scan de tous les ports
nmap -sV 192.168.1.1                  # Détection de version
nmap -sC 192.168.1.1                  # Scripts par défaut
nmap -A 192.168.1.1                   # Scan agressif (OS + versions + scripts)

# Scan furtif
nmap -sS 192.168.1.1                  # Scan SYN

# Scan de découverte
nmap -sP 192.168.1.0/24               # Ping scan

**Cas d'utilisation:**

- Découverte d'hôtes actifs
- Énumération de services et versions
- Détection de vulnérabilités avec NSE scripts

**Ressource:** [Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)