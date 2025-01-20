# 🌐 Challenge Web - Memes Cachés

## 📁 Catégorie
Web
https://hackropole.fr/fr/challenges/web/fcsc2022-web-chatpristi-1/

## ⭐ Difficulté
Facile

## 🎯 Objectif
Le challenge consiste à trouver des flags cachés dans un site web de memes. Le site présente une galerie d'images avec des tags et une barre de recherche. D'après l'énoncé, il y a deux flags à découvrir, et pour cette première partie, il faut trouver un meme caché.

## 🔍 Découverte

### 📊 Analyse initiale du site
Le site web présente les caractéristiques suivantes :
- Une galerie de memes avec des images stockées dans le dossier `/static/`
- Une barre de recherche en haut de la page
- Des tags associés à chaque image (fcsc, crypto, web, etc.)
- Utilisation du framework CSS Bulma
- Les images sont nommées avec ce qui semble être des hashes SHA-256

### 🎯 Points d'intérêt
- Une image est taguée avec "flag"
- Le paramètre de recherche dans l'URL : `/?search=`
- Plusieurs images sont liées au FCSC (France CyberSecurity Challenge)

## 💻 Exploitation

### 🔐 Test d'injection SQL

1. Observation : La recherche utilise probablement une base de données SQL pour filtrer les images

2. Tentative d'injection : `') OR 1=1 --`
   - `'` : Ferme la chaîne de caractères
   - `)` : Ferme une potentielle parenthèse de la requête
   - `OR 1=1` : Force la condition à être toujours vraie
   - `--` : Commente le reste de la requête

3. Résultat : L'injection fonctionne et révèle des memes cachés non visibles dans la galerie normale

## 🚩 Flag
Le flag a été trouvé parmi les memes cachés révélés par l'injection SQL.

## 📝 Notes
Cette vulnérabilité est un exemple classique d'injection SQL permettant de contourner les filtres de recherche en manipulant la requête SQL sous-jacente.

## 🛡️ Correction
Pour corriger cette vulnérabilité, plusieurs approches sont possibles :
- Utiliser des requêtes préparées (prepared statements)
- Échapper correctement les caractères spéciaux
- Implémenter une validation stricte des entrées utilisateur
- Utiliser un ORM (Object-Relational Mapping) qui gère automatiquement la sécurité des requêtes

## 📚 Ressources
- [OWASP - SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP - SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
