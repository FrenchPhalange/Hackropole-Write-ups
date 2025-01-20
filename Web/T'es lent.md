# T'es lent - Challenge FCSC 2023
https://hackropole.fr/fr/challenges/web/fcsc2023-web-tes-lent/

## 🎯 Objectif
"Vous avez très envie de rejoindre l’organisation du FCSC 2024 et vos skills d’OSINT vous ont permis de trouver ce site en construction. Prouvez à l’équipe organisatrice que vous êtes un crack en trouvant un flag !"
Trouver le flag caché sur le site web "T'es lent" en exploitant les informations sensibles laissées dans le code source.

## 🔍 Découverte

En inspectant le code source de la page d'accueil, on trouve :
1. Une première offre de stage visible
2. Une deuxième offre commentée :
```html
<!--
<div class="col-md-6">
  <div class="h-100 p-5 text-white bg-dark rounded-3">
    <h2>Générateur de noms de challenges</h2>
    <p>Vous serez en charge de trouver le noms de toutes les épreuves du FCSC 2024.</p>
    <a class="btn btn-outline-light" href="/stage-generateur-de-nom-de-challenges.html">Plus d'infos</a>
  </div>
</div>
-->
```

## 💉 Exploitation

### Première étape
En accédant à la page commentée `/stage-generateur-de-nom-de-challenges.html`, on découvre une nouvelle page avec un autre commentaire HTML révélant une interface d'administration :

```html
<!--
Ne pas oublier d'ajouter cette annonce sur l'interface d'administration secrète : /admin-zithothiu8Yeng8iumeil5oFaeReezae.html
-->
```

### Solution finale
En accédant directement à l'URL de l'interface d'administration :
```
/admin-zithothiu8Yeng8iumeil5oFaeReezae.html
```
Le flag est affiché sur la page.

## 🎉 Flag
```
FCSC{237dc3b2389f224f5f94ee2d6e44abbeff0cb88852562b97291d8e171c69b9e5}
```

## 📝 Notes
- Les commentaires HTML sont souvent utilisés pour cacher des informations sensibles
- Le challenge joue sur l'exploration du code source
- Plusieurs niveaux de découverte (page commentée → page admin)
- Le nom du challenge "T'es lent" fait probablement référence au fait qu'il faut prendre son temps pour lire le code source

## 🔒 Correction
Pour sécuriser ce type de vulnérabilité :
- Ne jamais laisser de commentaires contenant des informations sensibles dans le code source
- Ne pas utiliser des URLs "secrètes" comme seule méthode de sécurité (security through obscurity)
- Implémenter une véritable authentification pour les interfaces d'administration
- Nettoyer le code en production des commentaires de développement
