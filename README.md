# Classification automatique des interventions pharmaceutiques

> **NLP · Machine Learning · Pharmacie clinique · Sécurité médicamenteuse**

Projet de traitement automatique du langage naturel (**NLP**) consacré à la **classification automatique des interventions pharmaceutiques (IP)** issues de l’analyse pharmaceutique des prescriptions hospitalières.

L’objectif est d’évaluer dans quelle mesure des modèles de machine learning peuvent exploiter les **commentaires des pharmaciens** et les **libellés des molécules prescrites** afin d’automatiser deux tâches complémentaires :

1. **Détection des interventions potentiellement graves** : classification binaire visant à identifier les situations nécessitant une attention particulière.
2. **Classification des interventions pharmaceutiques** : attribution d’une intervention à l’une des **11 classes principales de la classification SFPC**.

Le projet s’inscrit dans une démarche de valorisation des données de pharmacie clinique et d’aide à la détection des situations à risque médicamenteux.

---

## Résultats principaux

| Tâche       | Problème                                 | Modèle principal | Performance rapportée                |
| ----------- | ---------------------------------------- | ---------------- | ------------------------------------ |
| **Tâche 1** | Détection des cas potentiellement graves | VotingClassifier | **Rappel : 87 % · Précision : 70 %** |
| **Tâche 2** | Classification en 11 classes SFPC        | XGBoost          | **Accuracy : 78 %**                  |

Pour la tâche 1, le seuil de décision a été ajusté afin de privilégier le **rappel**, dans une logique de réduction des faux négatifs.

La stratégie retenue rapporte **94 faux négatifs contre 170** avec une optimisation standard fondée sur le F1-score.

> **Important :** les performances présentées correspondent aux évaluations réalisées dans le cadre de ce projet. Elles ne constituent pas une validation clinique externe et ne doivent pas être interprétées comme une performance directement transposable à d’autres établissements ou populations.

---

## Objectifs

### Objectif principal

Développer un pipeline reproductible de NLP et de machine learning permettant d’automatiser la classification d’interventions pharmaceutiques à partir de données textuelles issues de l’activité de pharmacie clinique.

### Objectifs secondaires

* Prétraiter et normaliser les commentaires pharmaceutiques.
* Transformer les textes en représentations numériques exploitables par des algorithmes de machine learning.
* Intégrer des connaissances métier sous forme de variables indicatrices.
* Gérer le déséquilibre entre les classes.
* Comparer et combiner plusieurs modèles de classification.
* Optimiser le seuil de décision en fonction de l’objectif de détection.
* Évaluer les performances sur des jeux de données distincts.

---

## Données

Les données utilisées dans ce projet proviennent des **Hôpitaux Universitaires de Strasbourg (HUS)**.

### Fichiers principaux

| Fichier                        | Description                                                                                                                                                          |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data_defi3.csv`               | Données utilisées pour l’analyse et l’entraînement, comprenant notamment les libellés de molécules, les commentaires pharmaceutiques et les classes d’interventions. |
| `SFPC_encodage.csv`            | Table de correspondance utilisée pour l’encodage des classes d’interventions selon la classification SFPC.                                                           |
| `valid_set.csv`                | Jeu de données de validation utilisé pour l’évaluation et/ou la production des prédictions.                                                                          |
| `predictions_test_set.csv`     | Prédictions produites sur le jeu de test.                                                                                                                            |
| `predictions_valid_set.csv`    | Prédictions produites sur le jeu de validation.                                                                                                                      |
| `defi_2_VERSION_FINALE .ipynb` | Notebook principal contenant le pipeline d’analyse et de modélisation.                                                                                               |

### Confidentialité et gouvernance

Les données de santé nécessitent une attention particulière en matière de confidentialité, de gouvernance et de réutilisation.

Toute utilisation ou redistribution des données doit respecter les conditions applicables aux données sources ainsi que le cadre réglementaire en vigueur.

---

## Méthodologie

Le workflow repose sur plusieurs étapes :

```text
Données
   ↓
Prétraitement / normalisation du texte
   ↓
Extraction des caractéristiques
   ↓
TF-IDF + variables métier
   ↓
Gestion du déséquilibre
   ↓
Modélisation
   ↓
Évaluation
   ↓
Optimisation du seuil
   ↓
Prédictions
```

### 1. Prétraitement du texte

Les commentaires pharmaceutiques sont normalisés afin d’améliorer la robustesse des représentations textuelles :

* conversion en minuscules ;
* suppression des accents ;
* normalisation de certaines informations variables ;
* remplacement de certains **dosages** et **dates** par des tokens génériques (`<dosage>`, `<date>`) ;
* stemming afin de réduire certaines variations morphologiques.

Cette étape vise à limiter la variabilité lexicale tout en conservant l’information utile à la classification.

### 2. Variables issues de la connaissance métier

Trois variables binaires ont été construites à partir de mots-clés associés à des situations médicamenteuses critiques :

* `contient_ci` : présence d’un terme associé à une **contre-indication** ;
* `contient_surdosage` : présence d’un terme associé à un **surdosage** ;
* `contient_interaction` : présence d’un terme associé à une **interaction médicamenteuse**.

Ces variables complètent la représentation purement textuelle du commentaire.

### 3. Représentation TF-IDF

Les commentaires sont transformés en vecteurs numériques à l’aide de **TF-IDF (Term Frequency–Inverse Document Frequency)**.

Cette représentation permet de pondérer les termes selon leur fréquence dans un document et leur capacité à distinguer les documents au sein du corpus.

---

## Modélisation

### Tâche 1 — Détection des interventions potentiellement graves

La tâche binaire définit comme situations graves les classes :

* **4** — Surdosage ;
* **5** — Non indiqué ;
* **6.3** — Association déconseillée ;
* **6.4** — Contre-indication.

### Gestion du déséquilibre

Les observations correspondant aux situations graves représentent environ **17 %** des données d’apprentissage.

La méthode **SMOTE (Synthetic Minority Over-sampling Technique)** a été utilisée sur le jeu d’entraînement afin de limiter l’effet du déséquilibre des classes.

### Modèle

Un **VotingClassifier** combine trois familles de modèles :

1. Régression logistique ;
2. Random Forest ;
3. XGBoost avec pondération des classes.

### Optimisation du seuil

Compte tenu de l’objectif de détection des situations potentiellement graves, l’optimisation privilégie le **rappel** afin de limiter les faux négatifs, tout en conservant une précision acceptable.

---

### Tâche 2 — Classification multiclasse

La seconde tâche consiste à prédire la classe SFPC parmi **11 catégories**.

Le modèle principal retenu est un **XGBoost** adapté à la classification multiclasse.

---

## Évaluation

### Tâche 1 — Classification binaire

La stratégie d’optimisation du seuil rapportée dans le projet obtient :

* **Rappel : 87 %**
* **Précision : 70 %**

L’optimisation orientée rappel a permis de réduire le nombre de cas graves non détectés par rapport à une stratégie standard basée sur le F1-score :

* **94 faux négatifs** avec la stratégie orientée rappel ;
* **170 faux négatifs** avec la stratégie de comparaison.

### Tâche 2 — Classification multiclasse

Le modèle multiclasse obtient une **accuracy globale de 78 %**.

Les performances sont meilleures pour les classes les plus représentées, notamment les classes **1, 4 et 8**.

Les classes très rares restent plus difficiles à prédire, avec notamment des confusions avec la classe majoritaire.

> **Interprétation :** compte tenu du déséquilibre entre les catégories, l’accuracy globale doit être interprétée conjointement avec les métriques par classe, notamment la précision, le rappel et le F1-score.

---

## Structure du dépôt

```text
Classification-des-Interventions-Pharmaceutiques/
│
├── README.md
├── data_defi3.csv
├── valid_set.csv
├── SFPC_encodage.csv
├── predictions_test_set.csv
├── predictions_valid_set.csv
└── defi_2_VERSION_FINALE .ipynb
```

### Notebook principal

`defi_2_VERSION_FINALE .ipynb` contient le workflow d’analyse, de prétraitement, d’entraînement des modèles, d’évaluation et de génération des prédictions.

---

## Technologies utilisées

| Technologie          | Utilisation                          |
| -------------------- | ------------------------------------ |
| **Python**           | Développement du pipeline            |
| **Jupyter Notebook** | Analyse et expérimentation           |
| **Pandas**           | Manipulation des données             |
| **NumPy**            | Calcul numérique                     |
| **NLTK**             | Traitement du langage naturel        |
| **Scikit-learn**     | Prétraitement, modèles et évaluation |
| **XGBoost**          | Apprentissage supervisé              |
| **imbalanced-learn** | Gestion du déséquilibre / SMOTE      |
| **Matplotlib**       | Visualisation                        |
| **Seaborn**          | Visualisation statistique            |

---

## Reproduire l’analyse

### Prérequis

Python 3 et un environnement Jupyter sont nécessaires.

Installation des principales dépendances :

```bash
pip install pandas numpy nltk scikit-learn xgboost imbalanced-learn matplotlib seaborn jupyter
```

### Cloner le dépôt

```bash
git clone https://github.com/FLOWER-2024/Classification-des-Interventions-Pharmaceutiques.git
cd Classification-des-Interventions-Pharmaceutiques
```

### Lancer Jupyter

```bash
jupyter notebook
```

Puis ouvrir :

```text
defi_2_VERSION_FINALE .ipynb
```

et exécuter les cellules dans l’ordre.

> Pour une reproduction rigoureuse, il est recommandé de documenter les versions de Python et des principales bibliothèques ainsi que les paramètres des modèles.

---

## Limites

Plusieurs limites doivent être prises en compte avant toute utilisation opérationnelle :

* déséquilibre entre les classes, notamment pour les catégories rares ;
* dépendance aux caractéristiques linguistiques du corpus disponible ;
* risque de dégradation des performances lors d’une application à un autre établissement ou à une autre population ;
* absence de validation clinique externe dans le cadre présenté ici ;
* nécessité d’analyser les performances par classe et pas uniquement l’accuracy globale ;
* nécessité de documenter précisément les versions logicielles, les paramètres et la stratégie de séparation des jeux de données.

---

## Perspectives

Les développements futurs pourraient notamment porter sur :

* la comparaison avec des représentations par embeddings et des modèles de langage ;
* l’évaluation de modèles NLP plus avancés ;
* l’analyse de la calibration des probabilités ;
* la validation externe sur un corpus indépendant ;
* l’analyse des erreurs par type d’intervention pharmaceutique ;
* l’étude de l’explicabilité des prédictions ;
* l’évaluation de la robustesse du modèle sur des données provenant d’autres contextes hospitaliers.

---

## Compétences mobilisées

**Data Science · Machine Learning · NLP · Classification supervisée · Feature engineering · TF-IDF · SMOTE · XGBoost · Scikit-learn · Python · Analyse de données de santé · Pharmacie clinique**

---

## Statut du projet

Projet d’analyse de données et de **machine learning appliqué à la pharmacie clinique et aux données de santé**.

Les résultats présentés correspondent à l’expérimentation décrite dans le notebook et doivent être interprétés en tenant compte du contexte des données, de la méthodologie utilisée et des limites présentées ci-dessus.

---

## Mots-clés

**NLP · Natural Language Processing · Machine Learning · Pharmacie clinique · Interventions pharmaceutiques · SFPC · Sécurité médicamenteuse · Classification · XGBoost · TF-IDF · SMOTE · Python · Données de santé**
