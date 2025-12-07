# TP7 - Rapport court

## 1) Modèle utilisé
- Source des poids (TP4/TP5/TP6) :
- Nom du run MLflow (si applicable) :
- Fichier utilisé : models/weights/best.pt

## 2) Packaging
- Option utilisée : Docker / Local
- Commande(s) exécutée(s) :
- Résultat : yolo.mar généré (oui/non)

## 3) Déploiement
- `docker compose up -d --build`
- Services démarrés : torchserve, api-gateway

## 4) Tests
- Test TorchServe : succès/échec
- Test Gateway : succès/échec
- Exemple de sortie JSON (court extrait) :

## 5) Redéploiement
- Nouvelle version de poids : 
- Étapes effectuées :
- Résultat :

## 6) Discussion
- Pourquoi une gateway rend l’architecture plus agnostique ?
- Limites rencontrées :
- Améliorations possibles (KServe, A/B, canary, monitoring, etc.)
