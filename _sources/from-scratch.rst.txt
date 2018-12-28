Migrer depuis Scratch
=====================

Ce tutoriel compare une réalisation de Flappy Bird écrite en Scratch
avec une écrite avec Pygame Zero. Les programmes Scratch et Pygame Zero sont
en une certaine mesure très similaires.

La `version Pygame Zero`__ peut être trouvée dans le dépôt de Pygame Zero.

.. __: https://github.com/lordmauve/pgzero/blob/master/examples/flappybird/flappybird.py

Tu peux aussi télécharger la `version Scratch`__ depuis le même dépôt.

.. __: https://github.com/lordmauve/pgzero/raw/master/examples/flappybird/Flappy%20Bird.sb

La version Pygame Zero inclut la gestion du score, qui est omise dans les exemples de code
de cette page afin de simplifier la comparaison.

Le code Python montré ci-dessous est réorganisé pour plus de clarté au travers des exemples.


La scène
--------

Voici comment la scène est organisée dans le programme Scratch :

.. image:: _static/scratch/flappybird-stage.png

Il y a juste trois objets à part l'arrière plan : l'oiseau et les tuyaux du haut et du bas.

Cela correspond au programme Pygame Zero, en définissant ces objets à l'aide d'acteurs (``Actors``)::

   bird = Actor('bird1', (75, 200))
   pipe_top = Actor('top', anchor=('left', 'bottom'))
   pipe_bottom = Actor('bottom', anchor=('left', 'top'))

Avec Pygame Zero, nous devons nous assurer de dessiner ces objets. En général
cela donne un peu plus de flexibilité sur la façon de dessiner la scène:: 

   def draw():
       screen.blit('background', (0, 0))
       pipe_top.draw()
       pipe_bottom.draw()
       bird.draw()


Le mouvement des tuyaux
-----------------------

Les tuyaux se déplacent à une vitesse constante indépendante de l'oiseau.
Quand ils disparaissent du bord gauche de l'écran, ils réapparaissent à droite
et leur position verticale change aléatoirement.

En Scratch, cela peut être obtenu en créant deux scripts différents pour les tuyaux
du haut et du bas.

.. image:: _static/scratch/flappybird-top-start.png

.. image:: _static/scratch/flappybird-bottom-start.png

Pour résumer ce qui se passe ici :

* La condition ``x position < -240`` est vraie quand un tuyau disparaît à gauche
  de l'écran et c'est ce qui déclenche la remise à zéro des tuyaux. 
* La variable ``pipe_height`` est utilisée pour coordonner les deux tuyaux. Parce que
  l'espace entre eux doit rester le même, nous ne pouvons pas choisir les deux hauteurs
  aléatoirement. Donc un script contient la logique nécessaire et pas l'autre.
* La ligne ``set y position to pipe height +/- 230`` place un tuyaux au-dessus de
  ``pipe_height`` et l'autre en dessous ``pipe_height``.

Ce code devient bien plus simple avec Pygame Zero. Nous pouvons écrire une seule fonction
qui met à jour les deux tuyaux. En fait, je l'ai découpée d'une façon différente pour montrer
clairement que les traitements se font pour les deux tuyaux en même temps::

   import random

   WIDTH = 400
   HEIGHT = 708
   GAP = 130
   SPEED = 3

   def reset_pipes():
       pipe_gap_y = random.randint(200, HEIGHT - 200)
       pipe_top.pos = (WIDTH, pipe_gap_y - GAP // 2)
       pipe_bottom.pos = (WIDTH, pipe_gap_y + GAP // 2)

   def update_pipes():
       pipe_top.left -= SPEED
       pipe_bottom.left -= SPEED
       if pipe_top.right < 0:
           reset_pipes()

Une petite différence ici est que j'extrais les valeurs que je compte réutiliser
sous la forme de "constantes", écrite en MAJUSCULE. Cela me permet de les changer à un seul endroit
lorsque je veux ajuster le jeu. Par exemple dans le code au dessus, je pourrais élargir ou rétrécir
l'espacement entre les deux tuyaux seulement en changeant ``GAP`` (écart).

La plus grosse différence est qu'il n'y a pas de ``répéter indéfiniment`` dans le code Python.
C'est la grosse différence entre Scratch et la plupart des languages de programmation textuel:
vous devez mettre à jour le jeu d'un pas d'animation et ensuite retourner.
Retourner donne à Pygame Zero la chance de faire des chose comme traiter
les entrées ou redessiner l'écran. Boucler indéfiniment et le jeu resterait juste bloqué,
donc toute boucle doit se finir rapidement.

Pygame Zero appelle la fonction ``update()`` quand il veux que tu mettes à jour
l'animation d'un pas, donc nous avons juste besoin d'appeler ``update_pipes()``::

   def update():
      update_pipes()


L'oiseau
--------

La méthode décrite ci-dessus sur comment traduire Scratch en programme Python
s'applique aussi pour le comportement de l'oiseau. Regardons d'abord le code Python cette fois.

Le code de mise à jour de l'oiseau est organisé en une fonction appelée
``update_bird()``. La première chose que cette fonction contient est le code de gestion
des déplacement de l'oiseau selon la gravité::

   GRAVITY = 0.3

   # Initial state of the bird
   bird.dead = False
   bird.vy = 0

   def update_bird():
       uy = bird.vy
       bird.vy += GRAVITY
       bird.y += bird.vy
       bird.x = 75

Voici une formule simple de gravité :

* Gravité signifie **accélération constante vers le bas**.
* Accélération est une variation de **vitesse**.
* Vitesse est une variation de **position**.

Pour représenter cela, nous avons besoin d'une variable ``bird.vy`` qui est la vitesse
de l'oiseau dans la direction ``y``. C'est une nouvelle variable que nous définissons,
pas quelque chose que Pygame Zero nous fournit.

* Gravité signifie accélération constante vers le bas: ``GRAVITY`` est supérieure à 0.
* L'accélération est une variation de vitesse: ``GRAVITY`` est ajoutée à ``bird.vy``
* La vitesse est une variation de position: ``bird.vy`` est ajoutée à ``bird.y``

Note que l'oiseau ne bouge pas horizontalement ! Sa position ``x`` reste à 75
tout au long du jeu. Nous simulons le mouvement horizontal en déplaçant les tuyaux vers lui.
Ça donne l'impression d'une caméra se déplaçant en suivant l'oiseau. Donc il n'y a pas 
besoin d'une variable ``vx`` dans le jeu.

La section suivante fait battre les ailes de l'oiseau::

       if not bird.dead:
           if bird.vy < -3:
               bird.image = 'bird2'
           else:
               bird.image = 'bird1'

Cela vérifie que l'oiseau se déplace vers le haut ou le bas. Nous affichons l'image ``bird2``
s'il se déplace vers le haut et sinon l'image ``bird1``. (-3 a été choisi
après plusieurs essais pour que cela ait l'air convainquant).

La section suivante vérifie si l'oiseau a percuté un tuyau::

       if bird.colliderect(pipe_top) or bird.colliderect(pipe_bottom):
           bird.dead = True
           bird.image = 'birddead'

Si c'est le cas, nous positionnons ``bird.dead`` à ``True``. C'est une **valeur booléenne** signifiant qu'elle
vaut soit vrai (``True``) soit faux (``False``). Nous pouvons utiliser cela pour facilement vérifier si l'oiseau est en vie.
Si ce n'est pas le cas, il ne répondra plus aux commandes du joueur.

Et la dernière section vérifie si l'oiseau est arrivé en dehors, en bas ou en haut,
de l'écran du jeu. Si c'est le cas, il réinitialise l'oiseau::

       if not 0 < bird.y < 720:
           bird.y = 200
           bird.dead = False
           bird.vy = 0
           reset_pipes()

Que fait ``reset_pipes()`` ici ? Comme j'ai organisé le code des tuyaux en
une fonction séparée, je peux juste l'appeler à tout moment lorsque je veux réinitialiser les tuyaux.
Dans ce cas, cela fait un jeu plus intéressant car il laisse une chance au joueur 
de réagir quand l'oiseau est réinitialisé à sa position d'origine.

Encore une fois, cela doit être appelé à chaque *frame*, donc nous l'ajoutons à ``update()``::

   def update():
      update_walls()
      update_bird()

La dernière partie du code de l'oiseau est celle qui permet de répondre aux commandes du joueur.
Quand nous pressons une touche, l'oiseau bat des ailes vers le haut. Pygame Zero va appeler
la fonction ``on_key_down()`` - si vous l'avez définie - dès qu'une touche est pressée::

   FLAP_VELOCITY = -6.5

   def on_key_down():
       if not bird.dead:
           bird.vy = FLAP_VELOCITY

Ici, si l'oiseau n'est pas mort, nous définissons ``vy`` avec un nombre négatif :
dans Pygame Zero cela signifie qu'il commence à se déplacer vers le haut.

Tu devrais être capable de trouver plein de similitudes entre le code Python et
ce code Scratch :

.. image:: _static/scratch/flappybird-bird-start.png
.. image:: _static/scratch/flappybird-bird-space.png


Les grosses différence entre Scratch et Pygame Zero sont les suivantes :

* Tu ne peux pas boucler indéfiniment avec Pygame Zero, juste mettre à jour pour un pas et retourner.
* Les coordonnées sont différentes. Dans Pygame Zero, le coin en haut à gauche de l'écran est
  ``x = 0, y = 0``. La direction ``x`` va de gauche à droite comme avant, mais
  ``y`` va vers le bas de l'écran ! C'est pour cela que ``GRAVITY`` est un nombre positif et
  ``FLAP_VELOCITY`` est un nombre négatif dans le code Python.
* ``bird.dead`` est un booléen, donc nous pouvons écrire du code comme ``if not bird.dead``
  au lieu de ``dead = 0`` en Scratch.


Résumé
------

Beaucoup des concepts disponibles dans Scratch se traduisent directement dans Pygame Zero.

Voici quelques points de comparaison:

+-------------------------------------+--------------------------------------------+
| Dans Scratch                        | Dans Pygame Zero                           |
+=====================================+============================================+
| ``ajouter 1 à y`` (up)              | ``bird.y -= 1``                            |
+-------------------------------------+--------------------------------------------+
| ``ajouter -1 à y`` (down)           | ``bird.y += 1``                            |
+-------------------------------------+--------------------------------------------+
| ``basculer sur le costume [nom]``   | ``bird.image = 'name'``                    |
+-------------------------------------+--------------------------------------------+
| ``si dead = 0``                     | ``if not bird.dead:``                      |
+-------------------------------------+--------------------------------------------+
| ``mettre dead to 0``                | ``bird.dead = False``                      |
+-------------------------------------+--------------------------------------------+
| ``si touche le Top?``               | ``if bird.colliderect(pipe_top)``          |
+-------------------------------------+--------------------------------------------+
| ``Quand le drapeau est cliqué``...  | Met le code dans la fonction ``update()``. |
| ``répéter indéfiniment``            |                                            |
+-------------------------------------+--------------------------------------------+
| ``Quand la touche [X] est pressée`` | ``def on_key_down():``                     |
+-------------------------------------+--------------------------------------------+
| ``nombre aléatoire entre a et b``   | ``import random`` pour charger le module   |
|                                     | ``random``, puis ``random.randint(a, b)``  |
+-------------------------------------+--------------------------------------------+
| (0, 0) est le centre de la scène    | (0, 0) est le coin en haut à gauche de la  |
|                                     | fenêtre                                    |
+-------------------------------------+--------------------------------------------+

Dans certains cas, le code est plus simple en Python car il peut être organisé
de façon à être plus lisible.

La puissance des acteurs de Pygame Zero fait aussi que la manipulation des coordonnées est plus simple. 
Nous pouvons utiliser les positions ancre (``anchor``) pour positionner les tuyaux et nous avons été capables
de voir si un tuyau était en dehors de l'écran en vérifiant ``pipe_top.right < 0`` plutôt que
``if x position < -240``.
