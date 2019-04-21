Contribuer à Pygame Zero
========================

.. highlight:: none

Le projet Pygame Zero est hébergé sur GitHub:

    https://github.com/lordmauve/pgzero

.. _report-issue:

Rapporter un bogue ou une demande
---------------------------------

Vous pouvez rapporter un bogue ou une demande de fonctionnalité
que vous pensez être nécessaire à Pygame Zero en utilisant
le `Github issue tracker`_.

Voici quelques conseils à garder à l'esprit avant de procéder :

* Vous pourriez ne pas être le premier ! Vous devriez vérifier si
  quelqu'un n'a pas déjà rapporté le problème en cherchant parmi la liste
  des problèmes existants, à la fois ouverts ou fermés.

* Les développeurs ont besoin de savoir quelle version de Pygame Zero
  vous utilisez et sur quel système d'exploitation (Windows, Mac, Linux, etc.)
  et sa version (Windows 10, Ubuntu 16.04, etc.) vous le faites tourner.


.. _`Github issue tracker`: https://github.com/lordmauve/pgzero/issues


Comment créer une `pull request`
--------------------------------

Vous pouvez contribuer des changements à Pygame Zero en créant
une `pull request`.

C'est une bonne idée de d'abord :ref:`rapporter un problème <report-issue>`
afin de pouvoir discuter si votre changement a du sens.

Github dispose `d'aide sur comment créer une pull request`__,
mais voici une version rapide :

.. __: https://help.github.com/articles/creating-a-pull-request/

1. Soyez sûr d'être authentifié sur Github.
2. Allez à `page Github de Pygame Zero`_.
3. Cliquez sur "Fork" pour créer votre copie personnelle du dépôt.
4. Dupliquez ce dépôt sur votre ordinateur::

        git clone git@github.com:monlogin/pgzero.git

   Rappelez vous de remplacer ``monlogin`` par votre login Github.

5. Créez une branche dans laquelle vous allez apporter les changements.
   Choisissez un nom de branche qui décrit le changement que vous voulez apporter. ::

        git checkout -b ma-nouvelle-branche master

6. Écrivez le changement que vous souhaitez apporter.
7. Ajouter les fichiers que vous souhaitez valider::

        git add pgzero

8. Valider la révision des fichiers avec un message explicite::

        git commit -m "Correction du problème #42 en renommant les paramètres"

   Vous pouvez répéter les étapes 6 à 8 autant de fois que nécessaires jusqu'à obtenir la fonctionnalité désirée.

9. Poussez la nouvelle révision sur votre dépôt distant. ::

        git push --set-upstream origin ma-nouvelle-branche

10. Aller sur la page Github de votre dépôt and cliquez sur
    le bouton "Créer une pull request".


.. _`page Github de Pygame Zero`: https://github.com/lordmauve/pgzero


Installer une version en développement
--------------------------------------

Il est possible d'installer une version en cours de développement
en utilisant `pip`. Depuis le répertoire racine de votre copie locale des sources, lancez::

    pip3 install --editable .

La version installée va maintenant contenir tous les changements que vous
apporterez au code source.

Autrement, si vous ne voulez pas l'installer du tout, vous pouvez
le lancer ainsi::

   python3 -m pgzero <name of pgzero script>

Par exemple::

   python3 -m pgzero examples/basic/demo1.py


Comment lancer les tests
------------------------

Les tests peuvent être exécutés avec::

    python3 setup.py test


.. _translating:

Aider à traduire la documentation
---------------------------------

L'API de Pygame Zero sera toujours en anglais, mais vous pouvez
diffuser Pygame Zero  à plus d'utilisateurs dans le monde si la
documentation est disponible dans leur langue.

Si vous parlez couramment une autre langue, vous pouvez contribuer
en traduisant tout ou partie de la documentation.

La documentation est écrite en reStructuredText_, qui est un language textuel
à balises destiné à la documentation technique. Essayez autant que possible de
préserver le formatage actuel. reStructuredText n'est pas très difficile une
fois que vous êtes habitué.

Créer une traduction consiste à créer un dépôt séparé sur Github avec
une copie de la documentation, réécrite (au moins en partie) dans
la langue que vous souhaitez contribuer. L'avantage est que vous pouvez
travailler sur la traduction à votre propre rythme, sans soumettre
des pull requests au projet ``pgzero`` lui même.
Consultez le `guide de traduction`_ sur `Read The Docs` pour plus de détails.

Si cela vous semble abordable, voici comment vous pouvez procéder:

1. D'abords, ouvrez un problème sur le `pgzero issue tracker`_.
   Vous devriez chercher s'il n'existe pas un autre problème concernant
   la traduction que vous voulez entreprendre, avant d'en ouvrir un nouveau.
   Cela garantira que vous ne travaillerez pas sur une traduction que
   quelqu'un d'autre aurait déjà faite (vous pourriez peut-être
   plutôt collaborer).
2. Créer a nouveau dépôt Github, appeler le pgzero-*langue*,
   par exemple ``pgzero-spanish`` si vous aller traduire en espagnol.
3. Dupliquez ce dépôt sur votre ordinateur.
4. Téléchargez le répertoire ``doc/`` de Pygame Zero et valider une révision
   dans votre projet. Vous pouvez le faire en l'extrayant du
   `fichier ZIP du dépôt`_. Vous avez uniquement besoin du répertoire
   ``doc/``, vous pouvez effacer les autres fichiers.
5. Maintenant, vous pouvez travailler sur les fichiers .rst dans le
   répertoire ``doc`` en les traduisant avec votre éditeur préféré.
   Vous devriez valider des révision régulièrement et les pousser sur Github.
6. Postez un commentaire avec un lien vers votre dépôt dans le problème Github
   créer à l'étape 1. Vous pouvez le faire dès que vous avez de la matière
   à montrer, cela va aider d'autres personnes à contribuer à vos traductions
   s'ils sont intéressés.
7. `Configurer la documentation pour la construire sur Read The Docs`__.
   Encore une fois, postez un commentaire sur le problème Github quand
   cela fonctionne.
8. Nous pouvons alors relier la nouvelle documentation traduite avec
   la documentation officielle de Pygame Zero.

.. _reStructuredText: http://www.sphinx-doc.org/en/master/rest.html
.. _`guide de traduction`: https://docs.readthedocs.io/en/latest
                         /localization.html#project-with-multiple-translations
.. _`pgzero issue tracker`: https://github.com/lordmauve/pgzero/issues/new
.. _`fichier ZIP du dépôt`: https://github.com/lordmauve/pgzero/archive/master.zip

.. __: https://docs.readthedocs.io/en/latest/getting_started.html#import-your-docs

Notez que Pygame Zero va être mis à jour et la documentation va être changée
en conséquence. En utilisant Git il est possible de voir ce qui a changé dans
la documentation originale, afin que vous puissiez apporter les changements
nécessaires dans la documentation traduite.
