Les *hooks* d'évènements
========================

Pygame Zero va automatiquement détecter et appeler les *hooks* (littéralement « crochet » ou « hameçon ») d'évènements que tu définis.
Cette approche vous épargne d'écrire vous même la mécanique de boucle d'évènements.

Les *hooks* d'une boucle de jeu
-------------------------------

Une boucle de jeu typique ressemble à peu près à ça::

    while game_has_not_ended():
        process_input()
        update()
        draw()

Le traitement des entrées est un peu plus complexe, mais Pygame Zero te permet facilement
d'écrire les fonctions ``update()`` et ``draw()`` dans ton programme de jeu.

.. function:: draw()

    Elle est appelée par Pygame Zero quand il a besoin de redessiner l'écran de jeu.

    ``draw()`` ne prend aucun argument.

    Pygame Zero tente de décider quand l'écran de jeu a besoin d'être redessiné
    afin d'éviter de le faire si rien n'a changé. À chaque pas de la boucle de jeu,
    il va dessiner l'écran dans les situations suivantes :

    * Si tu as défini une fonction ``update()`` (voir ci-dessous).
    * Si un événement d'horloge est émis.
    * Si un événement d'entrée a été déclenché.

    Une façon de se faire piéger est lorsque tu tentes de modifier ou animer
    quelques chose dans la fonction ``draw``. Par exemple, ce code est faux : l'extraterrestre
    n'est pas garanti de bouger tout le long de l'écran::

        def draw():
            alien.left += 1
            alien.draw()

    Le code correct utilise la fonction ``update()`` pour modifier ou animer les objets et ``draw``
    simplement pour dessiner à l'écran::

        def draw():
            alien.draw()

        def update():
            alien.left += 1

.. function:: update() ou update(dt)

    Elle est appelée par Pygame Zero pour faire progresser ta logique de jeu. Elle va être appelée
    continuellement 60 fois par seconde.

    Il y a deux façons d'écrire une fonction ``update``.

    Dans les jeux simples, tu peux supposer qu'un court laps de temps (une fraction de seconde)
    est passé entre chaque appel à ``update()``. Peut être que la durée de ce laps n'a pas
    d'importance : tu peux juste bouger les objets d'un nombre constant de pixels
    par images (ou les accélérer par une constante, etc.)

    Une approche plus sophistiquée est de baser le mouvement et les calculs physiques
    sur la quantité réelle de temps écoulée entre deux appels. Cela produit
    des animations plus fluides, mais les calculs induits peuvent être plus compliqués et tu 
    dois être attentif à ne pas créer de comportements non-prédictibles lorsque le laps
    de temps écoulé devient plus grand.

    Pour utiliser l'approche basée sur le temps, tu peux changer la fonction ``update`` en ajoutant
    un paramètre unique. Si ta fonction ``update`` prend un argument, Pygame Zero
    va lui passer le temps écoulé en secondes. tu peux l'utiliser pour adapter le calcul des mouvements.


Les *hooks* de gestion d'événements
-----------------------------------

Comme pour les *hooks* de la boucle de jeu, ton programme Pygame Zero peut répondre aux événements d'entrée
en définissant des fonctions avec des noms spécifiques.

Comme pour la fonction ``update()``, Pygame Zero va inspecter ta
fonction de gestion d'événements pour déterminer comment l'appeler. Tu n'as donc pas besoin d'avoir
d'arguments à ta fonction. Par exemple, Pygame Zero va être capable d'appeler toutes ces variantes
de la fonction ``on_mouse_down``::

    def on_mouse_down():
        print("Mouse button clicked")

    def on_mouse_down(pos):
        print("Mouse button clicked at", pos)

    def on_mouse_down(button):
        print("Mouse button", button, "clicked")

    def on_mouse_down(pos, button):
        print("Mouse button", button, "clicked at", pos)

Il le fait en regardant le nom des paramètres, ils doivent donc être épelés exactement comme
ci-dessus. Chaque *hook* d'événements a un ensemble différent de paramètres que tu peux
utiliser comme décrit ci-dessous.

.. function:: on_mouse_down([pos], [button])

    Appelée lorsqu'un bouton de la souris est pressé.

    :param pos: un tuple (x, y) donnant la position du pointeur de souris lorsque le bouton a été pressé.
    :param button: une valeur de l'énumération :class:`mouse` indiquant quel bouton a été pressé.

.. function:: on_mouse_up([pos], [button])

    Appelée lorsqu'un un bouton de la souris est relâché.

    :param pos: un tuple (x, y) donnant la position du pointeur de souris lorsque le bouton a été relâché.
    :param button: une valeur de l'énumération :class:`mouse` indiquant quel bouton a été relâché.

.. function:: on_mouse_move([pos], [rel], [buttons])

    Appelée lorsque la souris est bougée.

    :param pos: un tuple (x, y) donnant la position du pointeur de souris à laquelle la souris a bougé.
    :param rel: un tuple (delta_x, delta_y) représentant le déplacement relatif du pointeur de souris.
    :param buttons: un ensemble de valeurs de l'énumération :class:`mouse` indiquant quels boutons
                    étaient pressés durant le déplacement.


Pour gérer le glissement de la souris (mouvement avec bouton pressé), utilise un code tel que le suivant::

    def on_mouse_move(rel, buttons):
        if mouse.LEFT in buttons:
            # la souris a été glissée, faire quelque chose avec `rel`
            ...


.. function:: on_key_down([key], [mod], [unicode])

    Appelée lorsqu'une touche est pressée.

    :param key: un entier indiquant quelle touche a été pressée (voir
                :ref:`ci-dessous <buttons-and-keys>`).
    :param unicode: lorsque cela a du sens, le caractère qui a été entré. Toutes les touches
                    ne produisent pas un caractère affichable - beaucoup sont des caractères
                    de contrôle. Dans le cas où la touche ne correspond pas à un caractère Unicode,
                    cela sera la chaîne vide.
    :param mod: un masque de bits représentant les touches mortes également pressées.

.. function:: on_key_up([key], [mod])

    Appelée lorsqu'une touche est relâchée.

    :param key: un entier indiquant quelle touche a été relâchée (voir
                :ref:`ci-dessous <buttons-and-keys>`).
    :param mod: un masque de bits représentant les touches mortes pressées.


.. function:: on_music_end()

    Appelée lorsque une :ref:`piste de musique <music>` se termine.

    Note qu'elle ne sera pas appelée si la piste audio est configurée pour boucler.


.. _buttons-and-keys:

Boutons et touches
''''''''''''''''''

Les objets pré-définis ``mouse`` et ``keys`` peuvent être utilisés pour déterminer quels boutons
ou touches étaient pressés dans les événements ci-dessus.

Note que les événements de la molette de la souris apparaissent comme des boutons pressés avec les constantes
``WHEEL_UP``/``WHEEL_DOWN``.

.. class:: mouse

    Une énumération pré-défini de boutons qui peut être reçue par les *hooks* ``on_mouse_*``.

    .. attribute:: LEFT
    .. attribute:: MIDDLE
    .. attribute:: RIGHT
    .. attribute:: WHEEL_UP
    .. attribute:: WHEEL_DOWN

.. class:: keys

    Une énumération pré-défini de touches qui peut être reçue par les *hooks* ``on_key_*``.

    .. attribute:: BACKSPACE
    .. attribute:: TAB
    .. attribute:: CLEAR
    .. attribute:: RETURN
    .. attribute:: PAUSE
    .. attribute:: ESCAPE
    .. attribute:: SPACE
    .. attribute:: EXCLAIM
    .. attribute:: QUOTEDBL
    .. attribute:: HASH
    .. attribute:: DOLLAR
    .. attribute:: AMPERSAND
    .. attribute:: QUOTE
    .. attribute:: LEFTPAREN
    .. attribute:: RIGHTPAREN
    .. attribute:: ASTERISK
    .. attribute:: PLUS
    .. attribute:: COMMA
    .. attribute:: MINUS
    .. attribute:: PERIOD
    .. attribute:: SLASH
    .. attribute:: K_0
    .. attribute:: K_1
    .. attribute:: K_2
    .. attribute:: K_3
    .. attribute:: K_4
    .. attribute:: K_5
    .. attribute:: K_6
    .. attribute:: K_7
    .. attribute:: K_8
    .. attribute:: K_9
    .. attribute:: COLON
    .. attribute:: SEMICOLON
    .. attribute:: LESS
    .. attribute:: EQUALS
    .. attribute:: GREATER
    .. attribute:: QUESTION
    .. attribute:: AT
    .. attribute:: LEFTBRACKET
    .. attribute:: BACKSLASH
    .. attribute:: RIGHTBRACKET
    .. attribute:: CARET
    .. attribute:: UNDERSCORE
    .. attribute:: BACKQUOTE
    .. attribute:: A
    .. attribute:: B
    .. attribute:: C
    .. attribute:: D
    .. attribute:: E
    .. attribute:: F
    .. attribute:: G
    .. attribute:: H
    .. attribute:: I
    .. attribute:: J
    .. attribute:: K
    .. attribute:: L
    .. attribute:: M
    .. attribute:: N
    .. attribute:: O
    .. attribute:: P
    .. attribute:: Q
    .. attribute:: R
    .. attribute:: S
    .. attribute:: T
    .. attribute:: U
    .. attribute:: V
    .. attribute:: W
    .. attribute:: X
    .. attribute:: Y
    .. attribute:: Z
    .. attribute:: DELETE
    .. attribute:: KP0
    .. attribute:: KP1
    .. attribute:: KP2
    .. attribute:: KP3
    .. attribute:: KP4
    .. attribute:: KP5
    .. attribute:: KP6
    .. attribute:: KP7
    .. attribute:: KP8
    .. attribute:: KP9
    .. attribute:: KP_PERIOD
    .. attribute:: KP_DIVIDE
    .. attribute:: KP_MULTIPLY
    .. attribute:: KP_MINUS
    .. attribute:: KP_PLUS
    .. attribute:: KP_ENTER
    .. attribute:: KP_EQUALS
    .. attribute:: UP
    .. attribute:: DOWN
    .. attribute:: RIGHT
    .. attribute:: LEFT
    .. attribute:: INSERT
    .. attribute:: HOME
    .. attribute:: END
    .. attribute:: PAGEUP
    .. attribute:: PAGEDOWN
    .. attribute:: F1
    .. attribute:: F2
    .. attribute:: F3
    .. attribute:: F4
    .. attribute:: F5
    .. attribute:: F6
    .. attribute:: F7
    .. attribute:: F8
    .. attribute:: F9
    .. attribute:: F10
    .. attribute:: F11
    .. attribute:: F12
    .. attribute:: F13
    .. attribute:: F14
    .. attribute:: F15
    .. attribute:: NUMLOCK
    .. attribute:: CAPSLOCK
    .. attribute:: SCROLLOCK
    .. attribute:: RSHIFT
    .. attribute:: LSHIFT
    .. attribute:: RCTRL
    .. attribute:: LCTRL
    .. attribute:: RALT
    .. attribute:: LALT
    .. attribute:: RMETA
    .. attribute:: LMETA
    .. attribute:: LSUPER
    .. attribute:: RSUPER
    .. attribute:: MODE
    .. attribute:: HELP
    .. attribute:: PRINT
    .. attribute:: SYSREQ
    .. attribute:: BREAK
    .. attribute:: MENU
    .. attribute:: POWER
    .. attribute:: EURO
    .. attribute:: LAST

De plus, tu peux accéder a un ensemble de constantes représentant les touches mortes :

.. class:: keymods

    Constantes représentant les touches mortes qui ont été pressées durant les événements 
    ``on_key_up``/``on_key_down``.

    .. attribute:: LSHIFT
    .. attribute:: RSHIFT
    .. attribute:: SHIFT
    .. attribute:: LCTRL
    .. attribute:: RCTRL
    .. attribute:: CTRL
    .. attribute:: LALT
    .. attribute:: RALT
    .. attribute:: ALT
    .. attribute:: LMETA
    .. attribute:: RMETA
    .. attribute:: META
    .. attribute:: NUM
    .. attribute:: CAPS
    .. attribute:: MODE

