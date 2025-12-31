# TP7 – Rapport court

## 1) Modèle utilisé
- **Source des poids (TP4/TP5/TP6)** :  
  Modèle YOLO entraîné lors des TP4, TP5 et TP6 à partir du dataset fourni.  
  Le modèle sélectionné correspond à celui présentant les meilleures performances sur le jeu de validation.

- **Nom du run MLflow ** :  
  `yolo_best_run`

- **Fichier utilisé** :  
  `models/weights/best.pt`

---

## 2) Packaging
- **Option utilisée** : Docker  

- **Commande(s) exécutée(s)** :
```bash
docker run --rm `
  -v ${PWD}:/workspace `
  -w /workspace `
  salahgo/torchserve:latest-cpu `
  torch-model-archiver `
    --model-name yolo `
    --version 1.0 `
    --handler /workspace/serving/torchserve/yolo_handler.py `
    --extra-files /workspace/models/weights/best.pt `
    --requirements-file /workspace/serving/torchserve/requirements.txt `
    --export-path /workspace/serving/torchserve/model-store `
    --force
```
Résultat :
-Le fichier yolo.mar a été généré avec succès (oui).
## 3) Déploiement
- `docker compose up -d --build`
- Services démarrés : torchserve, api-gateway

## 4) Tests
Test TorchServe : succès

Le endpoint /predictions/yolo répond correctement aux requêtes.

Test Gateway : succès

La gateway redirige correctement les requêtes vers TorchServe.
- Exemple de sortie JSON (court extrait) :
 "boxes": [
    {
      "xyxy": [
        119.80426788330078,
        185.96743774414062,
        227.4573974609375,
        268.6579284667969
      ],
      "conf": 0.9699186086654663,
      "cls": 7,
      "name": "truck"
    },

## 5) Redéploiement
- Nouvelle version de poids :
best_v2.pt

- Étapes effectuées :

Remplacement du fichier de poids dans le dossier models/weights

Regénération du fichier .mar avec torch-model-archiver

- Redémarrage des services via Docker Compose

Résultat :
La nouvelle version du modèle est déployée avec succès sans modification de la gateway.

## 6) Discussion
- Pourquoi une gateway rend l’architecture plus agnostique ?
La gateway agit comme une couche d’abstraction entre les clients et le service d’inférence.
Elle permet de changer le moteur de serving (TorchServe, TensorFlow Serving, KServe) ou le modèle sans impacter les clients.
Cette séparation améliore la portabilité, la maintenabilité et l’évolutivité de l’architecture.
- Limites rencontrées :


Configuration initiale de TorchServe parfois complexe

Gestion manuelle des versions de modèles

Absence de monitoring et de métriques avancées par défaut
- Améliorations possibles (KServe, A/B, canary, monitoring, etc.)

Déploiement sur Kubernetes avec KServe

Mise en place de déploiements A/B ou canary

Ajout de monitoring (Prometheus, Grafana)

Automatisation du versioning et des mécanismes de rollback