

_Le moteur de recherche pour les appareils connectés à Internet_

# Installation
pip install shodan

# Commandes principales
shodan search "org:\"Nom de l'Organisation\""  # Recherche par organisation
shodan host 8.8.8.8                           # Information sur une IP
shodan count apache country:FR                # Statistiques sur Apache en France

**Cas d'utilisation:**

- Recherche d'appareils exposés sur Internet
- Détection de services vulnérables
- Cartographie d'infrastructure

**Site web:** [https://www.shodan.io/](https://www.shodan.io/)
tags: #reconnaissance
[[OSINT]]