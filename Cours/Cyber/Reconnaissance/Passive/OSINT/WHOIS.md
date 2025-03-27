_Outil de requête d'informations sur les domaines et adresses IP_

# Installation
apt install whois

# Commandes principales
whois example.com                             # Information sur un domaine
whois 8.8.8.8                                # Information sur une IP
whois -h whois.arin.net -- '-i origin AS15169' # Information sur un AS

**Cas d'utilisation:**

- Obtenir les coordonnées de contact
- Vérifier les dates d'expiration de domaines
- Identifier les plages d'IP appartenant à une organisation

tags: #reconnaissance 
[[OSINT]]
