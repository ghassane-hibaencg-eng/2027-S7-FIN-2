# 20 projets de science des données orientés finance, banque et FinTech

**Public :** licence avancée, master, reconversion professionnelle  
**Durée indicative :** 2 à 4 semaines par projet  
**Outils :** Python, pandas, NumPy, scikit-learn, matplotlib, Google Colab

## Tableau récapitulatif

| N° | Projet | Type d’apprentissage | Modèles / méthodes proposés | Difficulté | Source de données |
|---:|---|---|---|---|---|
| 1 | Prédiction du défaut de crédit | Classification | Régression logistique, Random Forest, Gradient Boosting | Intermédiaire | [UCI — Default of Credit Card Clients](https://archive.ics.uci.edu/dataset/350) |
| 2 | Scoring et décision d’octroi de crédit | Classification | Régression logistique, Random Forest, analyse des coûts | Intermédiaire | [UCI — Statlog German Credit](https://archive.ics.uci.edu/dataset/144) |
| 3 | Détection de fraude bancaire | Classification déséquilibrée / anomalies | Régression logistique, Random Forest, Isolation Forest | Avancé | [OpenML — Credit Card Fraud](https://www.openml.org/search?id=42175&sort=runs&type=data) |
| 4 | Prédiction des faillites d’entreprises | Classification | Régression logistique, SVM, Random Forest | Avancé | [UCI — Polish Companies Bankruptcy](https://archive.ics.uci.edu/dataset/365) |
| 5 | Prédiction de souscription bancaire | Classification | Régression logistique, Random Forest, Gradient Boosting | Débutant | [UCI — Bank Marketing](https://archive.ics.uci.edu/dataset/222) |
| 6 | Analyse des demandes de crédit | Classification | KNN, arbre de décision, Random Forest | Débutant | [UCI — Credit Approval](https://archive.ics.uci.edu/dataset/27) |
| 7 | Détection de faux billets | Classification | Régression logistique, SVM, arbres | Débutant | [UCI — Banknote Authentication](https://archive.ics.uci.edu/dataset/267) |
| 8 | Évaluation du risque de solvabilité | Classification | Régression logistique, Random Forest, Gradient Boosting | Avancé | [UCI — Taiwanese Bankruptcy Prediction](https://archive.ics.uci.edu/dataset/572) |
| 9 | Prévision des taux d’intérêt | Régression temporelle | Persistance, Ridge, Gradient Boosting | Intermédiaire | [FRED — DGS10](https://fred.stlouisfed.org/series/DGS10) |
| 10 | Prévision du taux de change EUR/USD | Régression temporelle | Marche aléatoire, Ridge, Random Forest | Avancé | [FRED — DEXUSEU](https://fred.stlouisfed.org/series/DEXUSEU) |
| 11 | Prévision de volatilité boursière | Régression temporelle | Volatilité historique, Ridge, Gradient Boosting | Avancé | [FRED — SP500](https://fred.stlouisfed.org/series/SP500) |
| 12 | Prévision du chiffre d’affaires par client | Régression | Moyenne historique, Ridge, Random Forest | Intermédiaire | [UCI — Online Retail II](https://archive.ics.uci.edu/dataset/502) |
| 13 | Segmentation des clients selon leur valeur financière | Clustering | K-means, CHA, mélanges gaussiens | Débutant | [UCI — Online Retail](https://archive.ics.uci.edu/dataset/352) |
| 14 | Détection des transactions financières atypiques | Détection d’anomalies | Isolation Forest, LOF, One-Class SVM | Avancé | [OpenML — Credit Card Fraud](https://www.openml.org/search?id=42175&sort=runs&type=data) |
| 15 | Segmentation du risque financier des entreprises | ACP / clustering | ACP, K-means, CHA, GMM | Intermédiaire | [UCI — Taiwanese Bankruptcy Prediction](https://archive.ics.uci.edu/dataset/572) |
| 16 | Détection de régimes boursiers | Clustering temporel | K-means, mélanges gaussiens, ACP | Avancé | [FRED — SP500](https://fred.stlouisfed.org/series/SP500), [DGS10](https://fred.stlouisfed.org/series/DGS10) |
| 17 | Détection de comportements d’achat inhabituels | Détection d’anomalies | Isolation Forest, DBSCAN, seuils robustes | Intermédiaire | [UCI — Online Retail II](https://archive.ics.uci.edu/dataset/502) |
| 18 | Optimisation d’une campagne bancaire | Classification et décision | Régression logistique, Gradient Boosting, analyse du coût | Intermédiaire | [UCI — Bank Marketing](https://archive.ics.uci.edu/dataset/222) |
| 19 | Tableau de bord de risque de crédit | Transversal : analyse + classification | Régression logistique, Random Forest, calibration | Intermédiaire | [UCI — Statlog German Credit](https://archive.ics.uci.edu/dataset/144) |
| 20 | Analyse des cycles économiques et financiers | Transversal : ACP / clustering | ACP, K-means, mélanges gaussiens | Avancé | [FRED — T10Y2Y](https://fred.stlouisfed.org/series/T10Y2Y), [DGS10](https://fred.stlouisfed.org/series/DGS10) |

## Projets recommandés selon le niveau

- **Débutant :** 5, 6, 7 et 13.
- **Intermédiaire :** 1, 2, 9, 12, 15, 17, 18 et 19.
- **Avancé :** 3, 4, 8, 10, 11, 14, 16 et 20.

## Consignes pédagogiques communes

1. Formuler une question précise et justifier son intérêt financier.
2. Charger les données depuis la source indiquée et documenter leurs dimensions et variables.
3. Effectuer le nettoyage, l’analyse exploratoire et les visualisations.
4. Construire un modèle de référence et au moins un modèle plus avancé.
5. Choisir un protocole d’évaluation approprié (stratifié, chronologique ou avec groupes).
6. Comparer les métriques : PR-AUC / ROC-AUC pour la classification, MAE / RMSE pour la régression, silhouette / stabilité pour le clustering.
7. Expliquer les limites : fuite de données, déséquilibre, dérive temporelle, représentativité et biais.
8. Livrer un notebook exécutable, des figures et un rapport synthétique.

> **Remarque :** plusieurs projets réutilisent volontairement un même jeu de données pour traiter des problématiques distinctes. Pour les projets financiers temporels, éviter la séparation aléatoire entraînement/test. Ces projets sont pédagogiques et ne doivent pas servir directement à des décisions financières réelles.
