# Points à retenir 

Le *Machine Learning* permet de faire réaliser une tâche par un ordinateur sans avoir à le programmer explicitement mais en lui donnant des exemples de la tâche à réaliser. On parle alors d’Apprentissage Automatique ou Machine Learning.

L’*apprentissage supervisé* consiste à faire apprendre la machine à partir d’exemple annotés, c’est-à-dire un couple (entrée, sortie attendue)
Exemples : classification, régression, prédiction de séquences, apprentissage de stratégie 

L’*apprentissage non supervisé* consiste à utiliser la machine pour explorer les données et en faire ressortir des structures ou groupes d’éléments similaire. 
Exemples : clustering, visualisation, représentation en dimension réduite 

La *classification* consiste à apprendre à associer un exemple à un label
	Exemple : classification d’un mail en spam/non spam

La *régression consiste* à apprendre à associer un exemple à une valeur
	Exemple : prédiction de la valeur d’un espace publicitaire sur internet

Pour entrainer un algorithme de classification ou régression, il faut :

- Prendre un échantillon *aléatoire* de données issues du problème à traiter et les annoter
- Séparer les données en 3 ensembles : 
  - Ensemble d’*entrainement* : pour estimer les paramètres de l’algorithme
  - Ensemble de *validation* ou *développement*: pour comparer plusieurs algorithmes ou variantes d’un même algorithme
  - Ensemble de *test* : pour estimer les performances de l’algorithme choisi.
- Procéder par itération : 
  - Entrainer un modèle simple (régression logistique) pour avoir une idée de la complexité du problème
  - Entrainer un modèle plus complexe (svm, random forest, xgboost, MLP)
  - *Optimiser les valeurs de hyperparamètres* des algorithmes complexes sur l’ensemble de validation (grid search ou random search)
- Etudier l’impact de l’augmentation de la taille de l’ensemble d’apprentissage pour savoir si collecter plus de données serait utile (learning curves)
- Evaluer la variance de la mesure de performance sur l’ensemble de validation : si la variance est trop importante, les différences observées ne sont pas significatives, il faut plus d’exemples en validation.
	

