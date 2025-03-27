## Introduction

Sublist3r est un outil Python populaire en reconnaissance passive utilisé pour énumérer les sous-domaines d'un domaine cible en utilisant diverses sources publiques. Cet outil est particulièrement utile dans les phases initiales de tests d'intrusion ou d'évaluations de sécurité.

## Fonctionnalités principales

- Énumération rapide des sous-domaines en utilisant plusieurs moteurs de recherche simultanément
- Prise en charge de multiples sources comme Google, Yahoo, Bing, Baidu, et Ask
- Utilisation d'API comme Virustotal, Threatcrowd, DNSdumpster, et Netcraft
- Option de filtrage des résultats avec recherche de ports ouverts
- Multithreading pour des recherches plus rapides

## Installation
```
# Cloner le répertoire depuis GitHub
git clone https://github.com/aboul3la/Sublist3r.git

# Accéder au répertoire
cd Sublist3r

# Installer les dépendances
pip install -r requirements.txt
```

## Utilisation basique

	`python sublist3r.py -d example.com`

## Options principales

| Option | Description                                                 |
| ------ | ----------------------------------------------------------- |
| -d     | Spécifier le domaine cible                                  |
| -b     | Activer le bruteforce des sous-domaines                     |
| -p     | Effectuer un scan de ports sur les sous-domaines découverts |
| -v     | Activer le mode verbeux                                     |
| -t     | Nombre de threads à utiliser (par défaut : 40)              |
| -e     | Spécifier des moteurs de recherche                          |
| -o     | Enregistrer les résultats dans un fichier                   |
## Exemple d'utilisation avancée

`python sublist3r.py -d example.com -b -p 80,443,8080 -v -t 50 -o results.txt`