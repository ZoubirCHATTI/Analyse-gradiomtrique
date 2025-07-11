# Analyse et Visualisation de Données Géophysiques de Tipaza

Ce projet est dédié à l'analyse et à la visualisation de données géophysiques (magnétiques) provenant de la région de Tipaza. Il utilise des bibliothèques Python courantes telles que `pandas`, `numpy`, `matplotlib` et `scikit-learn` pour le traitement, l'analyse et la représentation graphique des données.

## Description du Projet

L'objectif principal de ce projet est de traiter et d'analyser des données gradiomagnétiques afin d'identifier des anomalies et de visualiser les variations spatiales des champs mesurés. Le processus inclut le chargement des données, l'exploration initiale, la détection et le traitement des valeurs aberrantes (outliers) à l'aide de la méthode IQR (Interquartile Range) et de l'algorithme Isolation Forest, ainsi que la création de grilles de données pour la visualisation sous forme de cartes de contour.

Ce projet est utile pour les géophysiciens, les chercheurs ou toute personne intéressée par l'analyse de données spatiales et la détection d'anomalies dans des ensembles de données géoscientifiques.



## Installation

Pour exécuter ce notebook, vous aurez besoin d'un environnement Python avec les bibliothèques suivantes installées. Vous pouvez les installer via `pip` :

```bash
pip install pandas numpy matplotlib scikit-learn scipy
```

## Exécution:

Pour l'exécution du code il suffit d'appuyer sur:  

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/ZoubirCHATTI/Analyse-gradiomtrique/HEAD?filepath=notebook/gradio_Tipaza.ipynb)



## Structure des Données
   
Le notebook charge un fichier Excel nommé `G_data.xlsx`. D'après l'analyse initiale (`data.info()` et `data.head()`), le fichier de données contient 1008 entrées et 4 colonnes, toutes de type `float64` et sans valeurs manquantes. Les colonnes sont :

-   `X`: Coordonnée X (géographique).
-   `y`: Coordonnée Y (géographique).
-   `Z`: Coordonnée Z (altitude).
-   `G`: Valeur gradiomagnétique mesurée.

Un aperçu des premières lignes (`data.head()`) montre des valeurs numériques pour ces colonnes, par exemple :

```
              X             y        Z      G
0  451081.30610  4.049865e+06  50.6361 -12.77
1  451082.28125  4.049865e+06  50.7006  -1.33
2  451083.25640  4.049865e+06  50.7651  -0.20
3  451084.28020  4.049865e+06  50.7990   1.51
4  451085.30400  4.049865e+06  50.8329   1.02
```

La colonne `G` est la variable d'intérêt pour l'analyse des outliers, avec des statistiques descriptives (`data['G'].describe()`) indiquant une plage de valeurs allant de -279.51 à 566.25.



## Méthodes d'Analyse

Le notebook utilise deux méthodes principales pour le traitement des données et la détection des outliers :

### 1. Méthode IQR (Interquartile Range)

La méthode IQR est utilisée pour identifier et filtrer les valeurs aberrantes dans la colonne `G`. La fonction `Remove_outliers_IQR` est définie pour calculer les bornes inférieure et supérieure (Q1 - 1.6 * IQR et Q3 + 1.6 * IQR) et retourner les données qui se situent dans ces bornes. Cette méthode est appliquée à la colonne `G` pour nettoyer le jeu de données.

### 2. Isolation Forest

L'algorithme Isolation Forest de `sklearn.ensemble` est employé pour une détection plus avancée des anomalies. Il est particulièrement efficace pour isoler les points de données anormaux en construisant des arbres d'isolement. Le modèle est initialisé avec un `contamination` de 0.1 (indiquant la proportion attendue d'outliers dans les données) et un `random_state` pour la reproductibilité. L'algorithme attribue un score à chaque point de données, où -1 indique une anomalie et 1 indique une donnée normale. Les données sont ensuite filtrées pour ne conserver que les points normaux.

## Visualisations

Le notebook génère plusieurs visualisations pour illustrer les données et les résultats de l'analyse :

-   **Affichage des données brutes (Contour Plot) :** Une carte de contour des données gradiomagnétiques brutes (`G`) est générée en utilisant `matplotlib.pyplot.contourf` après avoir créé une grille (`X`, `Y`, `Z`) à partir des coordonnées `x`, `y` et `G` via `scipy.interpolate.griddata` avec la méthode d'interpolation linéaire. Cela permet de visualiser la distribution spatiale des valeurs `G` avant tout traitement.

-   **Affichage après application de la méthode IQR (Contour Plot) :** Une carte de contour similaire est créée pour les données traitées par la méthode IQR. Cette visualisation utilise une palette de couleurs `coolwarm` et des niveaux de contour ajustés pour mieux mettre en évidence les variations après le nettoyage des outliers. L'image est sauvegardée sous le nom `Gradiomagnetic data__IQR processing__.png`.

-   **Affichage des anomalies détectées par Isolation Forest (Scatter Plot) :** Un nuage de points est généré pour visualiser les anomalies identifiées par l'algorithme Isolation Forest. Les points sont colorés en fonction de leur statut (anomalie ou normal), permettant une identification visuelle rapide des outliers. Bien que le code utilise `cmap='coolwarm'`, un avertissement peut apparaître si la colormapping n'est pas directement applicable aux données binaires des anomalies.

## Dépendances

Les principales bibliothèques Python utilisées dans ce notebook sont :

-   `pandas`: Pour la manipulation et l'analyse des données (chargement de fichiers Excel, DataFrames).
-   `numpy`: Pour les opérations numériques et les calculs matriciels.
-   `matplotlib`: Pour la création de visualisations statiques (cartes de contour, nuages de points).
-   `scikit-learn`: Spécifiquement `sklearn.ensemble.IsolationForest` pour la détection des outliers.
-   `scipy`: Spécifiquement `scipy.interpolate.griddata` pour l'interpolation des données et la création de grilles pour les cartes de contour.

**Auteur :** Zoubir CHATTI

