# 🌐 3D Financial Graph Visualizer - Unity & API Anti-Fraude

Ce projet est une application de visualisation en temps réel de graphes financiers 3D développée avec **Unity**. Elle se connecte à une API REST locale pour cartographier les comptes bancaires (nœuds) et les transactions (liens) afin de mettre en évidence des clusters comportementaux et des anomalies de fraude (Phase 2).

Pour surmonter la contrainte technique liée à la volumétrie de la base de données (plus de 244 000 nœuds), l'application implémente un algorithme de filtrage inversé exhaustif : elle extrait d'abord un sous-ensemble de transactions, puis scanne l'intégralité de l'API pour instancier uniquement les nœuds connectés.

---

## 🚀 Fonctionnalités

* **Scan Exhaustif de l'API :** Parcours complet et asynchrone des 244 560 nœuds par paquets (chunks) sans gel de l'application (optimisation par `Coroutines`).
* **Graphe Induit & Fluide :** Instanciation physique 3D (`Spheres` et `LineRenderers`) limitée uniquement aux nœuds connectés pour préserver le framerate (60+ FPS).
* **Coloration et Typage des Clusters :** Identification visuelle instantanée des groupes d'utilisateurs par codes couleur dynamiques.
* **Détection Visuelle de la Fraude :** * Les comptes frauduleux ou critiques apparaissent sous forme de **grosses sphères rouges**.
    * Les transactions frauduleuses (`is_fraud == 1`) sont tracées par des **lignes rouges épaisses**.

---

## 🛠️ Architecture du Code (`GraphClient.cs`)

Le script fonctionne en 3 étapes clés séquentielles :
1.  **Extraction des Edges :** Requête vers `/api/edges` et mémorisation de toutes les extrémités (IDs sources et cibles) dans un `HashSet<string>` (recherche en $O(1)$).
2.  **Scan Global des Nodes :** Boucle de pagination dynamique (`limit` et `offset`) qui lit l'ensemble de l'API. Si l'ID d'un nœud est présent dans le `HashSet`, sa sphère est construite et stylisée.
3.  **Tracé des Liens :** Liaison géométrique des éléments à l'écran via des composants `LineRenderer` configurés avec des shaders optimisés.

---

## 📦 Installation et Lancement

### Preréquis
* **Unity** (Version 2021.3 LTS ou supérieure recommandée).
* L'**API REST** locale active et accessible sur le port `8010`.

### Procédure de configuration
1.  Téléchargez et extrayez le fichier `.zip` du projet.
2.  Ouvrez le dossier racine dans **Unity Hub**.
3.  Assurez-vous que votre serveur/API local est lancé (`http://127.0.0.1:8010`).
4.  Dans Unity, ouvrez la scène principale dans le dossier `Assets`.
5.  Sélectionnez le GameObject portant le script `GraphClient` et ajustez les paramètres dans l'Inspecteur si nécessaire :
    * `Graph Scale` : Échelle globale du graphe (Défaut : `2700`).
    * `Spread Amount` : Force de la dispersion 3D (Défaut : `1500`).
6.  Appuyez sur **Play** ▶️.

---

## 📊 Endpoints API Utilisés

Le client interagit de manière autonome avec les routes suivantes :
* `GET /api/edges?limit=1500` - Récupération des transactions financières.
* `GET /api/nodes?limit=10000&offset={X}` - Lecture paginée de la base des utilisateurs.

---

## 👥 Auteur
* **Développement Unity & Intégration Algorithmique** : [Votre Nom / Pseudo]
* **Contexte du Projet** : Visualisation de données massives (Graphes) appliquée à la détection de fraudes bancaires.
