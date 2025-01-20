# 🌐 Challenge Web - BaguetteVPN (1/2)
https://hackropole.fr/fr/challenges/web/fcsc2021-web-baguette-vpn-1/

## 📁 Catégorie
Web

## ⭐ Difficulté
Facile

## 🎯 Objectif
Découvrir le fonctionnement d'un site web présentant un service de VPN et trouver un flag caché.

## 🔍 Découverte et Analyse

### Analyse initiale
- Le site web présente un service de VPN nommé "BaguetteVPN"
- Dans le code source HTML, on trouve des commentaires révélant deux endpoints :
  ```html
  <!-- 
    Changelog :
      - /api/image : CDN interne d'images
      - /api/debug
  -->
  ```

### Investigation des endpoints
1. Test de `/api/debug` :
   - Retourne un dump des variables globales de l'application
   - Révèle que l'application tourne sous Flask
   - Montre le chemin du fichier source : `/app/baguettevpn_server_app_v0.py`

2. Test de `/api/image` :
   - Endpoint qui accepte un paramètre `fn`
   - Renvoie une erreur 500 lors des tentatives d'accès au fichier source

## 💡 Exploitation

### Accès direct au code source
La clé de la solution a été de tenter d'accéder directement au fichier source via l'URL :
```
http://localhost:8000/baguettevpn_server_app_v0.py
```

Cette approche a fonctionné grâce à la route catch-all de Flask :
```python
@app.route("/<path:path>")
def load_page(path):
    if ".." in path:
        return Response("Interdit", status=403)
    try:
        with open(path, "r") as myfile:
            mime = "text/" + path.split(".")[-1]
            return Response(myfile.read(), mimetype=mime)
    except Exception as e:
        return Response(str(e), status=404)
```

Cette route permet de lire n'importe quel fichier dans le répertoire courant tant qu'il ne contient pas `..` dans son chemin.

## 🚩 Flag
Le flag était directement visible dans le code source de l'application :
```
FCSC{e5e3234f8dae908461c6ee777ee329a2c5ab3b1a8b277ff2ae288743bbc6d880}
```

## 📝 Notes et Apprentissages
- Importance de vérifier les routes catch-all dans les applications web
- Les commentaires dans le code source peuvent révéler des endpoints intéressants
- La fonctionnalité de lecture de fichier peut être exploitée de différentes manières
- L'accès direct aux fichiers sources peut parfois être plus simple qu'une exploitation complexe

## 🛡️ Correction
Pour sécuriser l'application, il faudrait :
- Limiter l'accès aux fichiers sources
- Implémenter une whitelist des fichiers accessibles
- Ne pas exposer les fichiers sensibles dans le répertoire web
- Supprimer les routes de debug en production
