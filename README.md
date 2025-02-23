Dans le cadre d’un projet, j’ai cherché à prédire la gravité des accidents en France métropolitaine à l’aide de modèles de Machine Learning. Pour cela, j’ai conçu un script Python dédié au traitement des données. La base de données utilisée provient de data.gouv.fr et regroupe les accidents de la route sur les années 2005, 2010 et 2015. Afin d’exploiter efficacement ces données, j’ai transformé toutes les variables qualitatives en variables quantitatives discrètes en utilisant la fonction astype(int). Les variables temporelles ont également été adaptées : les dates ont été converties en nombre de jours écoulés depuis le 1er janvier 2000, et les heures en minutes écoulées depuis minuit. Ces transformations permettent aux modèles d’interpréter plus facilement les informations temporelles sans les traiter comme des valeurs purement catégoriques. Concernant les coordonnées géographiques, j’ai multiplié la latitude et la longitude par un milliard afin d’éviter les erreurs d’arrondi et de garantir une meilleure précision numérique. Après ces transformations, la base de données a été exportée au format CSV via la fonction to_csv(), prête à être utilisée pour l’entraînement des modèles de classification. Cette préparation des données vise à optimiser les performances des algorithmes et à améliorer la précision des prédictions.

Après avoir traité les données, j’ai utilisé trois modèles de Machine Learning : Random Forest, Gradient Boosting et CatBoost. Étant donné que ma base était très déséquilibrée, j’ai appliqué la technique SMOTE pour la rééquilibrer. Plus précisément, j’ai multiplié par 20 la catégorie “tué” et par 1,25 la catégorie “blessé léger” afin de garantir une meilleure représentation des classes minoritaires.

Pour évaluer la performance de chaque modèle, j’ai analysé plusieurs éléments : la matrice de confusion, la distribution des classes prédites et l’importance des variables (features importance). Ces visualisations permettent de mieux comprendre le comportement des algorithmes et d’identifier le modèle le plus performant pour la prédiction de la gravité des accidents.

Globalement, les trois modèles renvoient des résultats similaires.
  
  Catboost : 

   Meilleur score: 0.6842754502315026
              precision    recall  f1-score   support

             0       0.58      0.35      0.44      9218
             1       0.72      0.83      0.77     22128
             2       0.85      0.87      0.86     17801
           
        accuracy                           0.75     49147
       macro avg       0.72      0.68      0.69     49147
    weighted avg       0.74      0.75      0.74     49147

  ![confusion_matrix](https://github.com/user-attachments/assets/7ccd26d4-1e24-4aca-a92b-48573963ebdc)

 À l’aide du tableau et de la matrice de confusion, on constate que le modèle est très performant pour la prédiction des catégories “tué” et “blessé léger”, mais peu efficace pour la catégorie “hospitalisé”.
    
![catboost](https://github.com/user-attachments/assets/0788554d-7088-4119-af5d-551a487ab474)

Les variables les plus influentes sont :
F10 (latitude) et F11 (longitude). La localisation de l’accident joue un rôle crucial dans la gravité. Certaines zones pourraient être plus dangereuses que d’autres (routes à risque, absence d’infrastructures de sécurité…).
F6 (date de l’accident). Cela peut être lié aux variations saisonnières ou à des tendances temporelles des accidents graves.

Random Forest :

Meilleur score: 0.6663858942896249
              precision    recall  f1-score   support

             0       0.60      0.24      0.34      9293
             1       0.73      0.87      0.79     22290
             2       0.87      0.94      0.90     17564

        accuracy                           0.77     49147
       macro avg       0.73      0.68      0.68     49147
    weighted avg       0.75      0.77      0.74     49147

![confusion_matrix](https://github.com/user-attachments/assets/f88465d4-9ed4-423f-9639-898f1f8cecd5)

![Poids_variables](https://github.com/user-attachments/assets/312f09de-e6aa-47e5-87fa-766246e5521f)

Cette fois, la variable avec le plus de poids est catr, qui représente la catégorie de la route, suivie par la latitude, la longitude, la luminosité et la date de l’accident.

On constate que le modèle Random Forest donne des résultats très proches de ceux de CatBoost, cependant, le score est légèrement inférieur à celui de CatBoost.

Gradiant Boosting :

Meilleur score: 0.7504667144826463
              precision    recall  f1-score   support

             0       0.58      0.35      0.44      9218
             1       0.72      0.83      0.77     22128
             2       0.85      0.87      0.86     17801

        accuracy                           0.75     49147
       macro avg       0.72      0.68      0.69     49147
    weighted avg       0.74      0.75      0.74     49147
    

![gradiant_boosting](https://github.com/user-attachments/assets/97063605-8d0c-446b-8743-7953ece767cd)

On voit que ce modèle ne rencontre pas de difficulté à prédire les “tué” et les “blessé léger”, mais toujours très peu pour la catégorie “hospitalisé”.

![poids_variables](https://github.com/user-attachments/assets/d51600da-7c66-4c28-9c10-5fcf26c67d99)

Comme pour CatBoost, le poids des variables est le même.

Pour conclure, on constate que ces trois modèles possèdent des résultats très similaires, même si le modèle Gradient Boosting présente un score significativement supérieur.


