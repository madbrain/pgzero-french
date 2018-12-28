Introduction à Pygame Zero
==========================

.. highlight:: python
    :linenothreshold: 5

Créer une fenêtre
-----------------

D'abord, crée un fichier vide appelé ``intro.py``.

Vérifie que cela démarre et ouvre une fenêtre vide en utilisant la commande::

    pgzrun intro.py

Tout dans Pygame Zero est optionnel: un fichier vide est un programme Pygame Zero valide !

Tu peux quitter le jeu en cliquant sur le bouton de fermeture de la fenêtre ou en pressant les touches
``Ctrl-Q`` (``⌘-Q`` sur Mac). Si le jeu se bloque pour une raison indéterminée, tu
peux le terminer en pressant ``Ctrl-C`` dans la fenêtre de terminal.


Dessiner un arrière-plan
------------------------

Ensuite, ajoutons une fonction :func:`draw` et définissons les dimensions de la fenêtre. Pygame Zero
va appeler cette fonction à chaque fois qu'il a besoin de dessiner l'écran.

Dans ``intro.py``, ajoute ce qui suit::

    WIDTH = 300
    HEIGHT = 300

    def draw():
        screen.fill((128, 0, 0))

Relance ``pgzrun intro.py`` et l'écran devrait maintenant afficher un carré rouge !

Que fait ce programme??

``WIDTH`` et ``HEIGHT`` contrôlent la largeur et la hauteur de la fenêtre. Le programme
définie la taille de la fenêtre à 300 pixels dans chaque direction.

``screen`` est une variable pré-définie qui représente la fenêtre d'affichage. Il y a
:ref:`collection de fonctions pour dessiner des images et des formes <screen>`. L'appel à la méthode
``screen.fill()`` remplit l'écran avec une couleur unie,
définie comme un tuple de couleur ``(rouge, vert, bleu)``.
``(128, 0, 0)`` sera un rouge moyennement foncé.
Essaye de changer ces valeurs avec des nombres compris entre 0 et 255 et regarde les couleurs que tu peux créer.

Ajoutons maintenant un *sprite* que nous pouvons animer.


Dessiner un *sprite*
--------------------

Avant de pouvoir dessiner quoique ce soit, nous allons avoir besoin de l'image d'un extraterrestre. Tu peux
cliquer avec le bouton de droite sur celle ci-dessous et l'enregistrer ("Enregistrer l'image sous..." ou quelque chose du genre).

.. image:: _static/alien.png

(Cette image a un paramètre de transparence, ou "alpha", qui est super pour les jeux !
Mais elle est conçue pour un arrière-plan sombre, il se peut donc que tu ne vois pas
le casque de l'extraterrestre avant qu'il soit affiché dans le jeu).

.. tip::

    Tu peux trouver plein d'images gratuites, incluant celle-ci, sur `kenney.nl
    <https://kenney.nl/assets?q=2d>`_. Celle-ci vient de
    `Platformer Art Deluxe pack
    <https://kenney.nl/assets/platformer-art-deluxe>`_.

Tu dois enregistrer le fichier au bon endroit pour que Pygame Zero puisse le trouver.
Crée un répertoire appelé ``images`` et enregistre l'image à l'intérieur en l'appelant
``alien.png``. Les noms de fichiers doivent être en minuscule, sinon Pygame Zero va se plaindre
en t'alertant d'un risque potentiel d'incompatibilité sur certaines plateformes.

Si tu as fait tout cela, ton projet devrait ressembler à ceci:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    └── intro.py

``images/`` est un répertoire standard dans lequel Pygame Zero va chercher tes images.

Il y a une classe pré-définie appelée :class:`Actor` que tu peux utiliser pour représenter
une image (*sprite*) qui doit être dessinée à l'écran.

Définissons-en une maintenant. Change le fichier ``intro.py`` afin qu'il contienne::

    alien = Actor('alien')
    alien.pos = 100, 56

    WIDTH = 500
    HEIGHT = alien.height + 20

    def draw():
        screen.clear()
        alien.draw()

Ton extraterrestre devrait maintenant apparaître à l'écran ! En donnant la chaîne ``'alien'``
à la classe ``Actor``, il charge automatiquement l'image correspondante. L'objet obtenu a des attributs
comme la position et la taille. Ceci nous permet de définir la hauteur de la fenêtre (``HEIGHT``) 
basée sur la hauteur de l'extraterrestre.

La méthode ``alien.draw()`` dessine le *sprite* sur l'écran à sa position courante.


Déplacer l'extraterrestre
-------------------------

Positionnons l'extraterrestre en dehors de l'écran, change la ligne ``alien.pos`` comme suit::

    alien.topright = 0, 10

Note comment tu peux définir ``topright`` pour déplacer l'extraterrestre par son
coin haut-droit. Si le bord droit de l'extraterrestre est positionné à ``0``, alors
il sera en dehors de l'écran juste à gauche. Maintenant faisons le bouger.
Ajoute les lignes suivantes à la fin du fichier::

    def update():
        alien.left += 2
        if alien.left > WIDTH:
            alien.right = 0

Pygame Zero va appeler la fonction :func:`update` une fois par *frame*. En déplaçant
l'extraterrestre par un petit nombre de pixels à chaque *frame*, cela va le faire glisser au travers de
l'écran. Une fois qu'il a atteint le bord droit de l'écran, nous le repositionnons à gauche.

Tes fonctions ``draw()`` et ``update()`` marchent de façon similaire mais ont été conçues pour des buts différents.
La fonction ``draw()`` dessine l'extraterrestre à sa position courante
tandis que la fonction ``update()`` est utilisée pour le déplacer à l'écran.


Gérer les clics
-----------------

Faisons en sorte que le jeu fasse quelque chose quand tu cliques sur l'extraterrestre. Pour faire cela, nous
avons besoin de définir une fonction appelée :func:`on_mouse_down`. Ajoute ce code au fichier source::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            print("Eek !")
        else:
            print("Tu m'as manqué !")

Tu peux démarrer le jeu et essayer de cliquer sur l'extraterrestre et à côté.

Pygame Zero est intelligent dans la façon d'appeler tes fonctions. Si tu ne définis pas
ta fonction avec un paramètre ``pos``, Pygame Zero va l'appeler sans position.
Il y a aussi un paramètre ``button`` pour ``on_mouse_down``. Nous aurions pu écrire::

    def on_mouse_down():
        print("Tu cliques !")

ou::

    def on_mouse_down(pos, button):
        if button == mouse.LEFT and alien.collidepoint(pos):
            print("Tu m'as touché !")



Sons et images
--------------

Maintenant faisons apparaître l'extraterrestre blessé. Enregistre ces fichiers:

* `alien_hurt.png <_static/alien_hurt.png>`_ - enregistre le en tant que ``alien_hurt.png``
  dans le répertoire ``images``.
* `eep.wav <_static/eep.wav>`_ - crée un répertoire appelé ``sounds`` et enregistre
  le fichier en tant que ``eep.wav`` dans ce répertoire.

Ton projet doit maintenant ressembler à ceci:

.. code-block:: none

    .
    ├── images/
    │   └── alien.png
    │   └── alien_hurt.png
    ├── sounds/
    │   └── eep.wav
    └── intro.py

``sounds/`` est un répertoire standard dans lequel Pygame Zero va chercher
tes fichiers audio.

Maintenant changeons la fonction ``on_mouse_down`` pour utiliser ces nouvelles ressources::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()

Maintenant, quand tu cliques sur l'extraterrestre, tu devrais entendre un son et le *sprite* devrait
se changer en un extraterrestre pas content.

Il y a cependant un bogue dans le jeu, l'extraterrestre ne redevient jamais
joyeux (mais le son se fait entendre à chaque clic). Réparons ça tout de suite.


L'horloge
---------

Si tu es habitué à Python en dehors de la programmation de jeux, tu peux connaître
la méthode ``time.sleep()`` qui attend un certain délai. Tu peux être tenté d'écrire
ton programme comme suit::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            alien.image = 'alien_hurt'
            sounds.eep.play()
            time.sleep(1)
            alien.image = 'alien'

Malheureusement, cela n'est pas utilisable dans un jeu. ``time.sleep()``
bloque toute activité, nous voulons que le jeu continue de fonctionner et de s'animer.
En fait nous devons sortir de la fonction ``on_mouse_down`` et laisser le jeu décider quand
réinitialiser l'extraterrestre au cours de son activité normale, tout en continuant d'appeler vos
fonctions ``draw()`` et ``update()``.

Ce n'est pas difficile avec Pygame Zero, car il y a la classe pré-définie
:class:`Clock` qui peut ordonnancer l'appel à des fonctions dans le futur.

D'abord, réorganisons notre programme. Nous pouvons créer des fonctions pour
définir l'apparence de l'extraterrestre blessé et aussi le remettre dans son état normal::

    def on_mouse_down(pos):
        if alien.collidepoint(pos):
            set_alien_hurt()


    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()


    def set_alien_normal():
        alien.image = 'alien'

Cela ne va rien changer pour l'instant. ``set_alien_normal()`` ne sera pas appelée.
Mais changeons ``set_alien_hurt()`` en utilisant l'horloge afin que
``set_alien_normal()`` soit appelée un moment plus tard.::

    def set_alien_hurt():
        alien.image = 'alien_hurt'
        sounds.eep.play()
        clock.schedule_unique(set_alien_normal, 0.5)

``clock.schedule_unique()`` va faire en sorte que ``set_alien_normal()`` soit appelée
après ``0.5`` seconde. ``schedule_unique()`` empêche aussi que l'appel à la même fonction
soit ordonnancé plus d'une fois, comme quand par exemple tu cliques très rapidement.

Essaye et tu verras l'extraterrestre revenir à l'état normal après 0.5 seconde. Essaye
de cliquer rapidement et vérifie que l'extraterrestre ne revienne dans l'état normal que 0.5 seconde
après le dernier clic.

``clock.schedule_unique()`` accepte comme intervalle de temps à la fois des entiers et des décimaux.
Dans ce tutoriel nous utilisons un nombre décimal pour l'illustrer mais
essaye d'utiliser les deux pour voir l'effet que produit chaque valeur.


Résumé
------

Nous avons vu comment charger et dessiner des *sprites*, jouer des sons,
gérer les actions de l'utilisateur et utiliser une horloge pré-définie.

Tu voudrais sûrement améliorer le jeu pour compter les points ou faire bouger
l'extraterrestre de façon plus erratique.

Il y a encore plus de fonctionnalités qui font que Pygame Zero est facile à utiliser.
Regarde la documentation :doc:`des objets pré-définis <builtins>` pour apprendre
le reste de l'API.
