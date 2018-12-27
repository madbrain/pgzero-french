Les principes de Pygame Zero
============================


Veuillez lire attentivement ce qui suit avant de contribuer.

Étant donné que Pygame Zero s'adresse essentiellement aux débutants, nous devons faire très attention à ne pas introduire de difficultés pour les programmeurs qui n'ont pas encore appris à les surmonter.

.. _accessibility:

Rendre accessible
-----------------

L'objectif principal de Pygame Zero est d'être accessible aux programmeurs débutants. La conception de l'API en est évidemment influencée.

Ceci s'applique également à des éléments comme les exigences matérielles : Pygame Zero a choisi à l'origine de ne prendre en charge que les entrées du clavier et de la souris, afin d'être accessible à tous les utilisateurs.


Être prudent
------------

À l'amorce du développement de Pygame Zero, Richard et moi (Daniel) avons effectué plusieurs allers-retours sur diverses fonctionnalités. Nous les avons ajoutées, les avons essayées et les avons retirées.

Les fonctionnalités doivent être rejetées si elles sont trop expérimentales ou si elles peuvent prêter à confusion.

Cela s'applique également aux autres éléments comme la prise en charge de l'OS : nous interdisons les noms de fichiers qui ne sont pas susceptibles d'être compatibles entre les systèmes d'exploitation.

Fonctionnel
-----------

Pygame Zero couvre pratiquement l'intégralité de Pygame - mais nous n'exposons pas toutes les fonctionnalités. Nous n'exposons que les fonctions qui fonctionnent vraiment bien sans peine et masquons une partie des autres éléments qui fonctionnent moins bien ou qui nécessitent des étapes supplémentaires.


Minimiser les temps d'exécution
-------------------------------

En fin de compte, Pygame Zero est un environnement de développement de jeux et l'adéquation des performances est un élément clé.

Il n'est pas vraiment judicieux de faire une vérification coûteuse à chaque image pour détecter un écueil potentiel. Au lieu de cela, nous pouvons effectuer un contrôle au départ, ou vérifier seulement quand une exception est levée pour la diagnostiquer et rapporter plus d'informations.


Des erreurs limpides
--------------------

Lorsque des exceptions sont levées par Pygame Zero, elles doivent délivrer des messages d'erreur clairs qui expliquent le problème.


Bien documenter
---------------

Comme tous les projets, Pygame Zero nécessite une bonne documentation. Les demandes d'intégration (pull requests) sont plus susceptibles d'être acceptées si elles sont accompagnées des documentations adéquates.

Essayez d'éviter les phrases compliquées et les termes techniques dans la documentation, afin qu'elle soit plus facilement lisible par des programmeurs inexpérimentés.


Minimiser les changements radicaux
----------------------------------

Dans les structures de l'éducation, les utilisateurs n'ont pas toujours le contrôle sur la version d'une bibliothèque qu'ils utilisent. Ils ne savent pas comment installer ou mettre à jour afin d'obtenir la dernière version.

Il est ici plus important d'obtenir des fonctionnalités correctes du premier coup que dans beaucoup d'autres projets.
