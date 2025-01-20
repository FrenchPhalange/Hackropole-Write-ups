# Scully 1

## 🎯 Objectif

Notre développeur web n’est pas très doué en matière de sécurité… Saurez-vous afficher le flag afin que nous puissions lui montrer qu’il faut faire des efforts ?

Note : Aucun bruteforce n’est nécessaire.

Une variante de cette épreuve est disponible ici : Scully 2.

## 🔍 Découverte

Le site web présente un formulaire de connexion simple avec :
- Un champ username
- Un champ password
- Un bouton de connexion

L'inspection du code source révèle un traitement classique des identifiants via une requête POST vers une API.

## 💉 Exploitation

### Première analyse
L'application utilise une validation côté serveur qui semble vulnérable aux injections SQL classiques.

### Solution
1. Dans le champ "Nom d'utilisateur" :
```sql
' OR 1=1 --
```

2. Dans le champ "Mot de passe" :
```
fsdfdsdf
```

Cette injection fonctionne car :
- Le `'` ferme la chaîne de caractères dans la requête SQL
- `OR 1=1` ajoute une condition toujours vraie
- `--` commente le reste de la requête SQL

## 🎉 Flag

Flag obtenu après connexion réussie 

## 📝 Notes
- L'injection SQL de base `' OR 1=1 --` fonctionne directement, indiquant une absence totale de protection
- Aucune validation des entrées n'est effectuée côté serveur
- Pas besoin de techniques avancées ou de bruteforce

## 🔒 Correction
Pour sécuriser ce type de vulnérabilité :
- Utiliser des requêtes préparées
- Échapper les caractères spéciaux
- Mettre en place une validation côté serveur
- Utiliser un ORM pour la gestion des requêtes SQL
- Implémenter une limitation du nombre de tentatives de connexion
