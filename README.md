# 🧠 Classification Automatique des Interventions Pharmaceutiques

## 🎯 1. Objectif du Projet

Ce projet vise à développer et valider des modèles de traitement automatique du langage (NLP) pour catégoriser automatiquement les interventions pharmaceutiques (IP) réalisées lors de l’analyse pharmaceutique des prescriptions hospitalières.

Les modèles utilisent les commentaires des pharmaciens et les libellés de molécules prescrites pour accomplir deux tâches principales :

1.  Tâche 1: Prédire si une erreur de prescription potentiellement **grave** a été identifiée.
2.  Tâche 2: Classer chaque commentaire dans l’une des **11 classes principales** de la Société Française de Pharmacie Clinique (SFPC).

L'automatisation de ce processus permet d'assister les pharmaciens, d'améliorer l'efficience du suivi et de renforcer la sécurité des patients.

## 🧾 2. Données et Outils

### Données
Les données utilisées proviennent des Hôpitaux Universitaires de Strasbourg.
- **`data_defi3.csv.gz`**: Contient les libellés de molécules, les commentaires des pharmaciens et les classes d'IP.
- **`SFPC_encodage.xlsx`**: Fournit la correspondance entre les classes SFPC et leurs codes numériques.

### Environnement Technique
- **Langage** : Python 3.x
- **Bibliothèques principales** : Pandas, NumPy, NLTK, Scikit-learn, XGBoost, Imbalanced-learn, Matplotlib, Seaborn.

## 🧩 3. Méthodologie et Approche Réalisée

Notre solution s'articule autour d'un pipeline NLP complet, du nettoyage des données brutes à la modélisation prédictive.

### 3.1. Prétraitement des Données et Ingénierie de Caractéristiques

Une fonction de nettoyage robuste a été appliquée pour standardiser le texte :
- Conversion en minuscules et suppression des accents.
- **Tokenisation sémantique** : Remplacement des dosages (ex: `10mg`) et dates par des jetons génériques (`<dosage>`, `<date>`) pour une meilleure généralisation.
- **Racinisation (Stemming)** : Réduction des mots à leur racine (ex: `recommandé` -> `recommand`).

Pour injecter une connaissance métier, nous avons également créé **3 caractéristiques binaires (features)** en détectant la présence de mots-clés critiques :
- `contient_ci` (contre-indication)
- `contient_surdosage`
- `contient_interaction`

### 3.2. Représentation Textuelle

Nous avons utilisé la méthode **TF-IDF (Term Frequency-Inverse Document Frequency)** pour convertir les commentaires nettoyés en vecteurs numériques. Cette approche donne plus de poids aux mots qui sont à la fois fréquents dans un commentaire mais rares dans l'ensemble des documents, ce qui permet de faire ressortir les termes les plus significatifs.

### 3.3. Modélisation

#### Tâche 1 : Prédiction de la Gravité (Binaire)
- **Définition des classes graves** : 4 (Surdosage), 5 (Non indiqué), 6.3 (Association déconseillée), 6.4 (Contre-indication).
- **Gestion du Déséquilibre** : Les cas graves étant minoritaires (17%), nous avons utilisé la technique **SMOTE** (Synthetic Minority Over-sampling Technique) pour rééquilibrer le jeu de données d'entraînement.
- **Modèle** : Un **`VotingClassifier`** a été choisi pour sa robustesse. Il agrège les prédictions de trois modèles :
  1.  Régression Logistique
  2.  Random Forest
  3.  XGBoost (avec pondération des classes)
- **Optimisation du Seuil** : Le seuil de décision a été optimisé pour **maximiser le Rappel** (capacité à détecter les cas graves) tout en maintenant une Précision acceptable, une stratégie cruciale pour la sécurité des patients.

#### Tâche 2 : Classification en 11 Classes (Multiclasse)
- **Modèle** : Un classifieur **`XGBoost`** a été entraîné, cet algorithme étant très performant pour les tâches de classification multiclasse.

## 📊 4. Résultats et Évaluation

### Tâche 1 : Performance de la Détection des Cas Graves

Grâce à l'optimisation du seuil, notre modèle binaire a atteint un **Rappel de 87%** pour une **Précision de 70%**. Cette approche a permis de **réduire de 45%** le nombre de cas graves non détectés par rapport à une optimisation standard du F1-score.
La stratégie 2 (Rappel Optimal) a été retenue car elle minimise les Faux Négatifs (94 contre 170).

### Tâche 2 : Performance de la Classification Multiclasse

Le modèle a atteint une **exactitude (accuracy) globale de 78%**.
- **Points forts** : Très bonne performance sur les classes fréquentes (1, 4, 8).
- **Points faibles** : Difficultés à classifier correctement les classes très rares (ex: classe 7), souvent confondues avec la classe majoritaire (classe 1).


## 🧮 Scripts et Utilisation

### Fichiers du Projet
- `defi_2_VERSION_FINALE.ipynb` : Notebook Jupyter contenant l'ensemble du code pour l'analyse, l'entraînement et la génération des prédictions.
- `predictions_test_set.csv` : Prédictions sur notre jeu de test interne.
- `predictions_valid_set.csv` : Fichier de soumission final pour le jeu de validation.
- `img/` : Dossier contenant les visualisations.

### Comment Exécuter
1.  Assurez-vous que toutes les bibliothèques listées dans le notebook sont installées.
2.  Exécutez les cellules du notebook `defi_2_VERSION_FINALE.ipynb` de manière séquentielle.
3.  Les fichiers de prédictions seront générés à la fin de l'exécution.

## 🧠 Mots-clés

Santé · Analyse de données · NLP · TF-IDF · Classification automatique · Machine Learning · XGBoost
