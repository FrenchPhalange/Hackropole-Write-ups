# 🔍 Challenge Web/Reverse - Flag Checker

## 📁 Catégorie
Web/Reverse Engineering

## ⭐ Difficulté
Moyenne

## 🎯 Objectif
Un service web permet de vérifier si un flag est correct. Le site utilise WebAssembly pour la vérification, et nous devons retrouver le flag valide.

## 🔍 Analyse

### Structure du site
1. Une interface web simple avec :
   - Un champ de saisie pour le flag
   - Un bouton "Check" pour vérifier
   - Un bouton "Clear" pour effacer
   
2. Les fichiers clés :
   - `index.html` : Interface utilisateur
   - `index.js` : Code JavaScript pour charger le WASM
   - `index.wasm` : Module WebAssembly contenant la logique de vérification

### Analyse du WebAssembly
Dans le fichier WASM, nous avons trouvé :

1. Une chaîne de données encodée à l'adresse 1024 :
```
E@P@x4f1g7f6ab:42`1g:f:7763133;e0e;03`6661`bee0:33fg732;b6fea44be34g0~
```

2. La fonction de vérification (`$b`) qui :
   - Prend notre input en paramètre
   - Effectue un XOR avec 3 sur chaque caractère
   - Compare le résultat avec la chaîne stockée

## 💡 Exploitation

Pour retrouver le flag, nous devons inverser l'algorithme :
1. Prendre la chaîne encodée
2. Faire un XOR avec 3 sur chaque caractère

Code JavaScript pour le décodage :
```javascript
let encoded = "E@P@x4f1g7f6ab:42`1g:f:7763133;e0e;03`6661`bee0:33fg732;b6fea44be34g0~";
let flag = '';
for(let c of encoded) {
    flag += String.fromCharCode(c.charCodeAt(0) ^ 3);
}
console.log(flag);
```

## 🚩 Flag
```
FCSC{7e2d4e5ba971c2d9e944502008f3f830c5552caff3900ed4018a5efb77af07d3}
```

## 📝 Notes et Apprentissages
- Importance de l'analyse du code WebAssembly
- Simplicité de certains schémas de chiffrement (ici, un simple XOR)
- Utilité des outils de développement du navigateur pour le debug et l'exécution de code

## 🛡️ Recommandations
Pour une meilleure sécurité :
- Ne pas utiliser d'algorithmes de chiffrement simples et réversibles
- Ne pas stocker les données sensibles directement dans le code client
- Implémenter la vérification côté serveur plutôt que côté client
