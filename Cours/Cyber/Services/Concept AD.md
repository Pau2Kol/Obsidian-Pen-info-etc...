## 1. Définition et Concept Général

L'Active Directory (AD) est un service d'annuaire développé par Microsoft, qui joue un rôle central dans les infrastructures réseau Windows. Il agit comme une base de données centralisée qui permet de gérer et de sécuriser les ressources d'un réseau d'entreprise.

## 2. Architecture Fondamentale

### Composants Principaux

- **Contrôleurs de domaine** : Serveurs qui hébergent et gèrent l'Active Directory
- **Domaines** : Groupe logique d'utilisateurs, d'ordinateurs et de stratégies
- **Forêts** : Collection de domaines partageant une configuration commune
- **Unités Organisationnelles (OU)** : Conteneurs pour organiser et gérer les objets

## 3. Mécanismes d'Authentification

### Protocoles de Sécurité

- **Kerberos** : Protocole d'authentification principal dans AD
- **NTLM** (moins sécurisé) : Ancien mécanisme d'authentification
- **LDAP** : Protocole pour accéder et modifier les informations de l'annuaire

EXPloit [[AD Exploit]]

## AD DS
base de donnée  sous forme de ntds.dit et stockée dan %systemroot%\NTDS

### AD DS Schéma
Défini tout les type d'objet pouvant être stocké dans l'AD comme un reglement

Renfore les règles concernant la création d'objet et la configuration

## forêt 

Une forêt c'est une collection d'un ou plusieurs domaine trees 
Propriétés:
- Partage un schéma commun
- Partage une configuration de partition commun
- Partage un catalogue global permettant la recherche
- Confiance entre chaque domaine de la forêt 
- Partage les chéma d'admin et de groupe d'entreprise 

## Organizational Units (OUs)

Ce sont les conteneurs de l'ad qui contiennent users,groups,computer,et les autres OUs
Permet de:
- Représenter l'oganisation de manière logique et hiérarchique
- gérer une collection d'objets de manière consitante
- Délégée les permissions aux administrateur de groups d'objets
- D'appliquer les police de l'entreprise 