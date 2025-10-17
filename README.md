# Classification-des-Interventions-Pharmaceutiques
🧠 Défi 2 – Classification de textes (Analyse pharmaceutique)
🎯 Objectif du projet

Ce projet vise à développer et valider des modèles de traitement automatique du langage (NLP) pour catégoriser automatiquement les interventions pharmaceutiques (IP) réalisées lors de l’analyse pharmaceutique des prescriptions hospitalières.

Les modèles utilisent les commentaires des pharmaciens et les libellés de molécules prescrites pour :

(Question 1) prédire si une erreur de prescription potentiellement grave a été identifiée.

(Question 2) classer chaque commentaire dans l’une des 11 classes principales de la Société Française de Pharmacie Clinique (SFPC).

🧾 Données

Les données utilisées proviennent des Hôpitaux Universitaires de Strasbourg et sont issues du logiciel d’aide à la prescription.

Fichier source : data_defi2.csv.gz
Ce fichier contient trois colonnes :

molecule : libellé du médicament prescrit

commentaire : commentaire du pharmacien

classe_IP : catégorie d’intervention pharmaceutique (selon la SFPC)

Fichier d’encodage : SFPC_encodage.xlsx
→ correspondance entre les classes SFPC et leurs codes numériques (1 à 11).

🧩 Étapes du projet
1. Prétraitement des données

Nettoyage des commentaires (ponctuation, casse, stopwords, lemmatisation)

Fusion éventuelle avec le libellé de molécule

Gestion des valeurs manquantes et des doublons

2. Représentation textuelle

TF-IDF

Word embeddings (Word2Vec, FastText)

LLM embeddings (optionnel – BERT, CamemBERT)

3. Modélisation
Question 1 : Gravité (binaire)

Classes graves : 4 (Surdosage), 5 (Non indiqué), 6.3 (Association déconseillée), 6.4 (Contre-indication)

Objectif → prédire grave (1) ou non grave (0)

Question 2 : Classification multiclasses (11 classes)

Prédire la classe principale de l’intervention pharmaceutique.

🧮 Scripts attendus

modele_gravite.py → modèle binaire (grave / non grave)

modele_classes.py → modèle multiclasses (1 à 11)

Chaque script doit :

accepter un fichier au même format que data_defi2.csv.gz

produire un fichier CSV de sortie avec les prédictions correspondantes.

📊 Évaluation

Question 1 (40%) : F1-score (puis PPV/rappel en cas d’égalité)

Question 2 (60%) : précision globale

Bonus : meilleure performance sur la prédiction binaire

🧠 Mots-clés

Santé · Analyse de données · NLP · TF-IDF · Classification automatique · Deep Learning · LLM

🧠 Mots-clés

Santé · Analyse de données · NLP · TF-IDF · Classification automatique · Deep Learning · LLM
