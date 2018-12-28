Objets pré-définis
==================

Pygame Zero fournit des objets pré-définis utiles pour t'aider
à construire des jeux facilement.


.. _screen:

Screen
------

.. toctree::
    :hidden:

    ptext

L'objet ``screen`` représente l'écran de ton jeu.

C'est une fine couche autour d'une surface Pygame qui te permet
facilement de dessiner des images à l'écran.

.. class:: Screen

    .. attribute:: surface

        La `surface Pygame`_ brute qui représente l'espace mémoire de l'écran. Tu peux
        l'utiliser pour les opérations graphiques avancées.

        .. _`surface Pygame`: https://www.pygame.org/docs/ref/surface.html

    .. method:: clear()

        Réinitialise l'écran en noir.

    .. method:: fill((rouge, vert, bleu))

        Rempli l'écran d'une couleur pleine.

    .. method:: blit(image, (gauche, haut))

        Dessine l'image sur l'écran à la position donnée.

        ``blit()`` accepte soit une surface ou une chaîne comme paramètre ``image``.
        Si ``image`` est une chaîne alors l'image au nom correspondant sera chargée
        depuis le répertoire ``images/``.

    .. method:: draw.line(début, fin, (r, v, b))

        Dessine une ligne de ``début`` à ``fin``.

    .. method:: draw.circle(pos, rayon, (r, v, b))

        Dessine un cercle.

    .. method:: draw.filled_circle(pos, rayon, (r, v, b))

        Dessine un disque.

    .. method:: draw.rect(rect, (r, v, b))

        Dessine le contour un rectangle.

        Prend un :ref:`Rect <rect>` en paramètre.

    .. method:: draw.filled_rect(rect, (r, v, b))

        Dessine un rectangle plein.

    .. method:: draw.text(texte, [pos], **kwargs)

        Dessine du texte.

        Il existe une API extrêmement riche pour positionner et formater du texte, regarder
        :doc:`ptext` pour plus de détails.

    .. method:: draw.textbox(texte, rect, **kwargs)

        Dessine du texte dimensionné pour remplir le :ref:`Rect` donné.

        Il existe une API extrêmement riche pour positionner et formater du texte, regarder
        :doc:`ptext` pour plus de détails.

.. _rect:

Rect
----

La classe `Pygame Rect`_ est disponible en tant que classe prédéfinie.
Elle peut être utilisée de nombreuses façons, de la détection de clics
dans une région au dessin d'une boîte sur l'écran:

Par exemple, tu peux dessiner une boîte avec::

    ROUGE = 200, 0, 0
    BOITE = Rect((20, 20), (100, 100))

    def draw():
        screen.draw.rect(BOITE, ROUGE)


.. _`Pygame Rect`: https://www.pygame.org/docs/ref/rect.html


Chargement de resources
-----------------------

Les objets ``images`` et ``sounds`` peuvent être utilisés pour charger des
images et des clips audio depuis des fichiers stockés dans les répertoires
``images`` et ``sounds`` respectivement. Pygame Zero va se charger de lire
ces resources à la demande et va les mémoriser afin d'éviter de les relire.

Tu doit généralement t'assurer que tes images sont nommées avec des lettres
minuscule, des chiffres et le caractère souligné ``_`` seulement.
Ils doivent aussi commencer par une lettre.

Les noms de fichiers suivants vont fonctionner correctement lors du
chargement de resources::

    alien.png
    alien_hurt.png
    alien_run_7.png

Ceux-ci ne fonctionneront pas::

    3.png
    3degrees.png
    my-cat.png
    sam's dog.png

Images
''''''

Pygame Zero peut charger des images aux formats ``.png``, ``.gif`` et ``.jpg``.
le format PNG est recommandé : il autorise des images de haute qualité et
gère la transparence.

Nous devons nous assurer que le répertoire ``images`` existe. Si ton projet
contient les fichiers suivants::

    space_game.py
    images/alien.png

Alors ``space_game.py`` peux dessiner l'image 'alien' à l'écran avec
ce code::

    def draw():
        screen.clear()
        screen.blit('alien', (10, 10))

Le nom passé à ``blit()`` est le nom du fichier image dans le répertoire
images, sans l'extension de fichier.

Ou en utilisant l'API :ref:`actor`::

    alien = Actor('alien')

    def draw():
        alien.draw()

Dans les deux cas, il y a quelques restrictions sur les noms de fichiers:
ils ne peuvent seulement contenir des lettres latines minuscules,
des chiffres et le caractère souligné ``_``. Ceci afin d'éviter des problèmes
de compatibilité quand ton jeu est exécuté sur différents systèmes
d'exploitation qui ont différentes sensibilités à la casse.

Surfaces d'images
'''''''''''''''''

Tu peux également charger des images depuis le répertoire ``images``
en utilisant l'objet ``images``. Ceci t'autorise à travailler avec les
données de l'image elle-même, obtenir ces dimensions, etc.::

    forest = []
    for i in range(5):
        forest.append(
            Actor('tree', topleft=(images.tree.width * i, 0))
        )

Chaque image chargée est une ``Surface`` Pygame. Tu va généralement
utiliser ``screen.blit(...)`` pour les dessiner à l'écran. Elle fournit
également des méthodes pratiques pour obtenir la taille de l'image en pixels:

.. class:: Surface

    .. method:: get_width()

        Retourne la largeur de l'image en pixels.

    .. method:: get_height()

        Retourne la hauteur de l'image en pixels.

    .. method:: get_size()

        Retourne un tuple (largeur, hauteur) indiquant la taille en pixels de la surface.

    .. method:: get_rect()

        Retourne un objet :class:`Rect` qui est prérempli avec les limites de l'images
        comme si l'image était située à l'origine.

        Dans les faits, ceci est équivalent à::

            Rect((0, 0), image.get_size())


Sons
''''

Pygame Zero peut charger des sons aux formats ``.wav`` et ``.ogg``.
le format WAV est adapté aux petits effets sonores, tandis que OGG est
un format compressé qui convient mieux à la musique. Tu peux trouver
des fichiers .ogg et .wav libres de droits en ligne que tu peux utiliser
dans ton jeu.

Nous devons nous assurer que le répertoire ``sounds`` existe. si ton projet
contient les fichiers suivants::

    drum_kit.py
    sounds/drum.wav

Alors ``drum_kit.py`` peux jouer le son de batterie dès que la le bouton
de la souris est pressé avec le code suivant::

    def on_mouse_down():
        sounds.drum.play()

Chaque son chargé est un object Pygame ``Sound`` et a diverses méthodes
pour jouer et stopper le son, ainsi qu'obtenir sa longueur en secondes:

.. class:: Sound

    .. method:: play()

        Joue le son.

    .. method:: play(n)

        Joue le son en boucle ``n`` fois.

        :param n: Le nombre de fois à répéter. Si tu donnes ``-1`` comme nombre de répétition,
                    le son va se répéter indéfiniment (ou jusqu'à ce que tu appelles :meth:`.Sound.stop()`)

    .. method:: stop()

        Arrête de jouer le son.

    .. method:: get_length()

        Retourne la durée du son en secondes.

Tu devrait éviter d'utiliser l'objet ``sounds`` pour jouer de longs
morceaux de musique, parce que Pygame va complètement charger la
musique en mémoire avant de la jouer, ceci peut utiliser beaucoup
de mémoire, ainsi qu'introduire un délai au chargement de la musique.

.. _music:

Musique
-------

.. versionadded:: 1.1

.. warning::

    L'API musique est expérimentale et peux causer des problèmes sur certaines plateformes.

    En particulier:

    * le format MP3 peut ne pas être disponible dans toutes les distributions Linux.
    * certains fichier OGG Vorbis semble bloquer Pygame en consommant 100% du CPU.

    Dans le dernier cas, le problème peut être réglé en re-encodant
    (si possible avec un encodeur différent).


L'objet prédéfini ``music`` fournit le moyen de jouer de la musique depuis
le répertoire ``music/`` (à côté de tes répertoires ``images/`` et ``sounds/``,
si tu en as). Le système musical va charger la piste de musique par petits
morceaux pendant que la musique est jouée, évitant le problème de l'utilisation
de ``sounds`` pour jouer de long morceaux de musique.

Une autre différence avec les effets sonores est que une seule piste de musique
peut être jouée à la fois. Si tu joues une piste différente, la piste en cours
d'exécution et stoppée.


.. function:: music.play(nom)

    Joue une piste de musique depuis le fichier donné. La piste va jouer indéfiniment.

    Cela va remplacer la piste en cours et annuler toutes les pistes mises en attente avec ``queue()``.

    Tu n'as pas besoin d'inclure l'extension dans le nom de la piste, par exemple, pour jouer
    le fichier ``handel.mp3`` en boucle::

        music.play('handel')

.. function:: music.play_once(nom)

    Similaire à ``play()``, mais joue la musique une fois seulement.

.. function:: music.queue(nom)

    Similaire à ``play_once()``, mais au lieu de stopper la musique en cours, la piste
    va être mise sur liste d'attente pour être jouée une fois que la musique en cours sera finie
    (ou après les autres musiques mises en attente précédemment).

.. function:: music.stop()

    Arrête la musique.

.. function:: music.pause()

    Met la musique en pause temporairement. Elle peut être redémarrée en appelant
    ``unpause()``.

.. function:: music.unpause()

    Redémarre la musique.

.. function:: music.is_playing()

    Retourne vrai si la musique en en train d'être jouée (et n'est pas en pause), faux dans les autres cas.

.. function:: music.fadeout(durée)

    Diminue progressivement le volume et stoppe la musique en cours d'exécution.

    :param duration: La durée en secondes pendant laquelle le volume va diminuer.
                    Par exemple, pour diminuer le son pendant une demi seconde, appelle
                    ``music.fadeout(0.5)``.

.. function:: music.set_volume(volume)

    Défini le volume de la musique.

    Cela prend un nombre décimal entre 0 (pas de son) and 1 (volume maximum).

.. function:: music.get_volume()

    Retourne le volume courant de la musique.


Si tu as démarré la lecture de la piste de musique en utilisant
:func:`music.play_once()`, tu peux utiliser le
:func:`hook on_music_end() <on_music_end>` pour faire quelque chose
quand le morceau se termine, par exemple, pour choisir aléatoirement le
prochain morceau.


.. _clock:

Horloge
-------

Souvent pendant l'écriture d'un jeu, tu veux planifier des événements
pour qu'ils se produisent un moment plus tard. Par exemple, nous voudrions
qu'un super chef extraterrestre apparaisse après 60 secondes.
Ou peut-être qu'un bonus apparaisse toutes les 20 secondes.

Plus subtiles sont les situations où tu veux retarder une action pour
une durée plus courte. Par exemple, tu pourrais avoir une arme laser
qui prend 1 secondes à se recharger.

Nous pouvons utiliser l'objet ``clock`` pour programmer l'appel d'une
fonction à se faire dans le future.

Commençons par définir une fonction ``fire_laser`` que nous voulons
exécuter dans le future::

    def fire_laser():
        lasers.append(player.pos)

Ensuite lorsque le bouton de tir est pressé, nous allons demander à l'objet
``clock`` de l'appeler pour nous, après exactement 1 seconde::

    def on_mouse_down():
        clock.schedule(fire_laser, 1.0)

Note que ``fire_laser`` est elle-même une fonction, sans parenthèse,
car elle n'est pas appelée ici ! L'horloge se chargera de le faire pour nous.

(C'est une bonne habitude d'écrire les durées en secondes avec une décimale,
comme ``1.0``. C'est ainsi plus évident en se relisant que cela
représente une durée et pas un compte de quelques chose.)

L'objet ``clock`` fournit les méthodes utiles suivantes:

.. class:: Clock

    .. method:: schedule(callback, delai)

        Programme `callback` à être appelée après le délai donné.

        Un nombre répété d'appels à ``schedule`` va programmer l'appel
        à la fonction autant de fois.

        :param callback: Une fonction qui ne prend aucun paramètre.
        :param delai: Le délai en secondes à attendre avant que la fonction ne soit appelée.

    .. method:: schedule_unique(callback, delai)

        Programme `callback` à être appelée une fois après le délai donné.

        Si `callback` avait déjà été programmée, cela l'annule et la reprogramme. Cela
        s'applique aussi si elle était programmée plusieurs fois: après l'appel à
        ``schedule_unique``, la fonction sera programmée exactement une fois.

        :param callback: Une fonction qui ne prend aucun argument.
        :param delai: Le délai en secondes à attendre avant que la function ne soit appelée.

    .. method:: schedule_interval(callback, intervalle)

        Programme `callback` pour être appelée de façon répétée.

        :param callback: Une fonction qui ne prend aucun argument.
        :param intervalle: L'intervalle en secondes entre chaque appel à `callback`.

    .. method:: unschedule(callback)

        Déprogramme l'appel à `callback` s'il a été précédemment programmé (soit parce qu'il
        a été programmé avec ``schedule()`` et n'a pas encore été exécuté ou
        parce qu'il a été programmé pour se répéter avec ``schedule_interval()``.


Note que l'horloge de Pygame Zero ne tient que des références faibles
aux fonctions que tu lui donnes. Il ne déclenchera pas d'événements programmés
si les objets et méthodes ne sont pas référencés ailleurs. Cela empêche
l'horloge de garder des objets vivants et de continuer à déclencher des
événements inattendus après qu'ils sont normalement morts.

La contre-partie aux références faibles et que tu ne pourras pas programmer
des lambdas ou tout autre objet créé uniquement pour être programmé.
Tu devras garder une référence à l'objet.

.. _actor:

Acteurs
-------

Une fois que tu as pleins d'images se déplacant dans le jeux, il est
intéressant d'avoir quelque chose qui regroupe à un seul endroit l'image
et où elle se trouve à l'écran. Nous allons appeler chaque image se déplaçant
à l'écran un acteur (``Actor``). Tu peux créer un acteur en fournissant
au moins le nom d'une image (depuis le répertoire d'images décrit au-dessus).
Pour dessiner un extraterrestre::

    alien = Actor('alien', (50, 50))

    def draw():
        screen.clear()
        alien.draw()

tu peux déplacer l'acteur en changeant son attribut ``pos`` dans la fonction ``update``::

    def update():
        if keyboard.left:
            alien.x -= 1
        elif keyboard.right:
            alien.x += 1

et tu peux changer l'image utilisée pour déssiner l'acteur en changeant
son attribut ``image`` pour un nouveau nom d'image::

    alien.image = 'alien_hurt'

Les acteurs ont tous les mêmes attributs et méthodes que :ref:`Rect <rect>`,
en incluant les méthodes telles que `.colliderect()`__ qui peut être utilisée
pour tester si deux acteurs se percutent.

.. __: https://www.pygame.org/docs/ref/rect.html#pygame.Rect.colliderect


Positionner les acteurs
'''''''''''''''''''''''

Si tu donnes une nouvelle valeur à un des attributs de position, alors l'acteur
va se déplacer. Par exemple::

    alien.right = WIDTH

va positionner l'extraterrestre de façon à ce que sa droite est la
valeur ``WIDTH``.

De façon similaire, tu peux aussi donner la position initiale de l'acteur
dans le constructeur, en donnant l'un des mots-clés suivant en argument:
``pos``, ``topleft``, ``topright``, ``bottomleft``, ``bottomright``,
``midtop``, ``midleft``, ``midright``, ``midbottom`` ou ``center``:

.. image:: _static/actor/anchor_points.png

Cela peut être fait pendant la création ou en affectant une paire
de coordonnées (x, y). Par exemple::

    WIDTH = 200
    HEIGHT = 200

    alien = Actor('alien', center=(100,100))

    def draw():
        screen.clear()
        alien.draw()

.. image:: _static/actor/alien_center.png

En changeant ``center=(100, 100)`` en ``midbottom=(100, 200)`` cela te donnes:

.. image:: _static/actor/alien_midbottom.png

Si tu ne spécifies pas de position initiale, l'acteur sera positionné par
défaut dans le coin en haut à gauche (équivalent à ``topleft=(0, 0)``).

.. _anchor:

Point d'ancrage
'''''''''''''''

Les acteurs ont une "position d'ancrage", qui est une façon pratique de
positionner les acteurs dans la scène. Par défaut, le point d'ancrage est
le centre, si bien que l'attribut ``.pos`` représente le centre de l'acteur
(de même pour les coordonnées ``x`` et ``y``). Il est commun de définir
le point d'ancrage à une autre partie du sprite (peut être les pieds afin
de pouvoir facilement faire tenir l'acteur sur quelque chose)::

    alien = Actor('alien', anchor=('center', 'bottom'))
    spaceship = Actor('spaceship', anchor=(10, 50))

``anchor`` est défini comme le tuple ``(xanchor, yanchor)``, où les valeurs
peuvent être des décimaux ou les chaînes ``left``, ``center``/``middle``,
``right``, ``top`` ou ``bottom``.


.. _rotation:

Rotation
''''''''

.. versionadded:: 1.2

L'attribut ``.angle`` de l'acteur controle la rotation du sprite, en degrés,
dans le sens trigonométrique (sens inverse des aiguilles d'une montre).

Le cente de rotation est le :ref:`point d'ancrage <anchor>` de l'acteur.

Note que cela va changer la hauteur (``width``) et la largeur (``height``)
de l'acteur.

Par exemple, pour faire tourner lentement un astéroïde dans l'espace::

    asteroid = Actor('asteroid', center=(300, 300))

    def update():
        asteroid.angle += 1

Pour le faire tourner dans l'aute sens, nous écririons ``update()`` ainsi::

    def update():
        asteroid.angle -= 1

Un autre exemple: nous pourrions faire qu'un acteur ``ship`` soit toujours
tourné vers la souris. Comme :meth:`~Actor.angle_to()` retourne 0 pour
la droite, l'acteur `ship` devrait se tourner vers la droite::

    ship = Actor('ship')

    def on_mouse_move(pos):
        ship.angle = ship.angle_to(pos)

.. image:: _static/rotation.svg
    :alt: Diagram showing how to set up sprites for rotation with angle_to()

Souviens toi que les angles bouclent:  0 degrés == 360 degrés == 720 degrés.
De même -180 degrés == 180 degrés.


Distance et angle
'''''''''''''''''

Les acteurs ont des méthodes très pratiques pour calculer leur distance
ou angle avec d'autres acteurs ou des paires de coordonnées ``(x, y)``.

.. method:: Actor.distance_to(cible)

    Retourne la distance depuis la position de l'acteur à la `cible` en pixels.


.. method:: Actor.angle_to(cible)

    Retourne l'angle depuis la position de l'acteur à la `cible` en degrés.

    Cela va retouner un nombre entre -180 et 180 degrés. La droite est 0 degrés
    et les angles augmentent en allant dans le sens horaire inverse (sens trigonométrique).

    Donc:

    * La gauche est 180 degrés.
    * Le haut est 90 degrés.
    * Le bas est -90 degrés.


Le clavier
----------

Tu as sans doute remarqué que nous avons utilisé l'objet ``keyboard``
dans le code au-dessus. Si tu veux savoir quelles touches du clavier sont
pressées, tu peux lire les attributs de l'objet pré-défini ``keyboard``.
Si, par exemple, la flêche gauche est maintenue enfoncée, alors
``keyboard.left`` sera vrai (``True``), sinon il sera faux (``False``).

Il y a un attribut pour chaque touche, quelques exemples::

    keyboard.a  # La touche 'A'
    keyboard.left  # La touche de navigation gauche
    keyboard.rshift  # La touche majuscule droite
    keyboard.kp0  # La touche '0' sur le pavé numérique
    keyboard.k_0  # La touche prinicipale '0'

L'ensemble des constantes de touche est donné dans la documentation
`Buttons and Keys`_, mais les attributs sont en minuscule, parce que ceux
sont des variables et non des constantes.

.. deprecated:: 1.1

    Le noms en majuscule ou préfixés (ie. ``keyboard.LEFT`` ou
    ``keyboard.K_a``) sont maintenant dépréconisés: utilise plutôt
    des noms d'attributs en minuscule.

.. _`Buttons and Keys`: hooks.html#buttons-and-keys

.. versionadded:: 1.1

    Tu peux aussi interroger l'état des touches en utilisant les constantes des touches elles-même::

        keyboard[keys.A]  # Vrai si la touche 'A' est pressée
        keyboard[keys.SPACE]  # Vrai si la barre d'espace est pressée


Animations
----------

Tu peux animer pratiquement tous dans pygame en utilisant la fonction
pré-définie ``animate()``. Par exemple, pour bouger un :ref:`acteur <actor>`
depuis sa position courante à l'écran vers la position ``(100, 100)``::

    animate(alien, pos=(100, 100))

.. function:: animate(objet, tween='linear', duration=1, on_finished=None, **cibles)

    Anime les attributs de `objet` depuis leurs valeurs courantes vers celles spécifiées
    par mots-clés dans `cibles`.

    :param tween: Le type de *tweening* à utiliser.
    :param duration: La durée de l'animation en secondes.
    :param on_finished: Fonction appelée à la fin de l'animation.
    :param targets: Le valeurs cibles des attributs à animer.

L'argument `tween` peut être l'un des suivants:

+--------------------+------------------------------------------------------+
| 'linear'           | Anime avec une vitesse constante du début à la fin   |
+--------------------+------------------------------------------------------+
| 'accelerate'       | Démarre doucement et accélère à la fin               |
+--------------------+------------------------------------------------------+
| 'decelerate'       | Démarre rapidement et ralentit à la fin              |
+--------------------+------------------------------------------------------+
| 'accel_decel'      | Accélère jusqu'à la moitié puis ralentit à la fin    |
+--------------------+------------------------------------------------------+
| 'end_elastic'      | Oscille légèrement à la fin                          |
+--------------------+------------------------------------------------------+
| 'start_elastic'    | Oscille légèrement au démarrage                      |
+--------------------+------------------------------------------------------+
| 'both_elastic'     | Oscille aux deux extrémités                          |
+--------------------+------------------------------------------------------+
| 'bounce_end'       | Accélère jusqu'à la fin et rebondit                  |
+--------------------+------------------------------------------------------+
| 'bounce_start'     | Rebondit au démarrage                                |
+--------------------+------------------------------------------------------+
| 'bounce_start_end' | Rebondit aux deux extrémités                         |
+--------------------+------------------------------------------------------+

La fonction ``animate()`` retourne une instance de la classe ``Animation``:

.. class:: Animation

    .. method:: stop(complete=False)

        Arrête l'animation et la complète optionnellement jusqu'à sa valeur finale.

        :param complete: Donne à l'attribut animé sa valeur cible.

    .. attribute:: running

        Vaut vrai si l'animation est en cours d'exécution. Il vaudra faux si la
        durée de l'animation est dépassée ou si la méthode ``stop()`` a été appelée avant.

    .. attribute:: on_finished

        Tu peux définir cet attribut avec une fonction qui sera appelée
        à la fin normale de l'animation. Le paramètre ``on_finished`` de la
        fonction ``animate()`` défini aussi cet attribut. Elle ne sera pas appelée si
        ``stop()`` est appelée. Cette fonction ne prend pas de paramètres.


Générateur de sons
------------------

.. versionadded:: 1.2

Pygame Zero peut jouer des sons en utilisant un synthétiseur intégré.

.. function:: tone.play(hauteur, duration)

    Joue une note à la hauteur donnée pour la durée donnée.

    La durée est en secondes.

    La `hauteur` peut être donnée comme un nombre, dans ce cas cela
    sera la fréquence de la note en hertz.

    Sinon, la hauteur peut être définie comme une chaîne représentant
    le nom d'une note et un octave. Par exemple:

    * ``'E4'`` sera un mi au 4ème octave.
    * ``'A#5'`` sera un la dièse au 5ème octave.
    * ``'Bb3'`` sera un si bémol au 3ème octave.

Créer des notes, en particulier de longues notes, prend du temps (jusqu'à
plusieurs millisecondes). Tu peux créer des notes à l'avance afin de ne pas
ralentir ton jeu pendant qu'il s'exécute:

.. function:: tone.create(hauteur, duration)

    Crée et renvoie un objet `Sound`.

    Les paramètres sont les mêmes que ceux de `play()`, décrit au-dessus.

Ceci peut être utilisé dans un programme Pygame Zero comme ceci::

    beep = tone.create('A3', 0.5)

    def on_mouse_down():
        beep.play()
