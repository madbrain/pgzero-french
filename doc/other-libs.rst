Autres bibliothèques comme Pygame Zero
======================================

Pygame Zero a démarré une tendance pour les bibliothèques "zéro" Python.
Nos amis ont créé ces superbes bibliothèques. Certaines d'entre elles
peuvent être combinées avec Pygame Zero!


Network Zero
------------

`Network Zero`_ simplifie la découverte et la communication en réseau
de plusieurs machines ou processus sur une même machine.

.. caution::

    Si vous voulez utiliser Network Zero avec Pygame Zero,
    soyez sûr qu'il ne **bloque** pas (arrête tout en attendant des messages).
    Cela va interrompre Pygame Zero qui va arrêter les animations
    à l'écran ou même répondre aux entrées. Donnez toujours aux options
    ``wait_for_s`` ou ``wait_for_reply_s`` la valeur de ``0`` secondes.


.. _`Network Zero`: https://networkzero.readthedocs.io


GUI Zero
--------

`GUI Zero`_ est une bibliothèque pour créer des interfaces graphiques avec
fenêtres, boutons, curseurs, champs texte, etc..

Parce que GUI Zero et Pygame Zero utilisent des approches différentes
pour dessiner à l'écran, ils ne sont pas utilisables ensemble.


.. _`GUI Zero`: https://lawsie.github.io/guizero/


GPIO Zero
---------

`GPIO Zero`_ est une bibliothèque pour contrôler les appareils connectés
aux broches d'entrées/sorties générales (GPIO) sur un `Raspberry Pi`_.

GPIO Zero marche généralement dans son propre *thread*, ce qui signifie
qu'il va généralement bien fonctionner avec Pygame Zero.

.. caution::

    En copiant les exemples de GPIO Zero, ne copiez pas les appels de fonction ``time.sleep()``
    ou les boucles ``while True:``, car ils vont empêcher Pygame Zero d'animer l'écran
    ou répondre aux entrées. Utiliser plutôt les fonctions :ref:`clock` pour appeler
    périodiquement des fonctions, ou la fonction :func:`update()` pour vérifier une
    valeur à chaque *frame*.

.. _`GPIO Zero`: https://gpiozero.readthedocs.io/
.. _`Raspberry Pi`: https://www.raspberrypi.org/


Adventurelib
------------

`Adventurelib`_ est une bibliothèque qui simplifie l'écriture de jeux
orientés texte (mais qui ne fait pas tout à votre place!).

Écrire des jeux orientés texte demande des aptitudes différentes que d'écrire
des jeux graphiques. Adventurelib est destiné à des développeurs Python d'un
niveau légèrement plus avancé que celui de Pygame Zero.

Adventurelib ne peut actuellement pas être combiné avec Pygame Zero.


.. _Adventurelib: https://adventurelib.readthedocs.io/


Blue Dot
--------

`Blue Dot`_ vous permet de contrôler vos projets Raspberry Pi sans fils en utilisant
un appareil Android comme une télécommande Bluetooth.

Blue Dot tourne généralement dans son propre *thread*, ce qui signifie
qu'il va généralement bien fonctionner avec Pygame Zero.

.. caution::

    Évitez les appels à la fonction ``time.sleep()``, les boucles ``while True:``
    et les méthodes bloquantes de Blue Dot ``wait_for_press`` et ``wait_for_release``,
    car celles-ci vont empêcher Pygame Zero d'animer l'écran ou de répondre aux entrée.
    Utiliser plutôt les fonctions :ref:`clock` pour appeler
    périodiquement des fonctions, ou la fonction :func:`update()` pour vérifier une
    valeur à chaque *frame*.


.. _`Blue Dot`: https://bluedot.readthedocs.io/


.. tip::

    Vous connaissez d'autres bibliothèques qui ont leur place ici ?

    `Ouvrer un problème <https://github.com/lordmauve/pgzero/issues/new>`_ sur le
    *tracker* de problèmes pour nous le faire savoir !
