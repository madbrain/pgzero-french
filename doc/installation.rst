Installer Pygame Zero
=====================

Inclus avec Mu
--------------

L'`éditeur Mu <https://codewith.mu>`_, qui est destiné aux débutants, inclus une
version de Pygame Zero.

Vous devez `basculer vers le mode <https://codewith.mu/en/tutorials/1.0/modes>`_
Pygame Zero pour pouvoir l'utiliser. Écrivez alors un programme et
`utiliser le bouton Play <https://codewith.mu/en/tutorials/1.0/pgzero>`_ pour le lancer
avec Pygame Zero.

.. note::

    La version de Pygame Zero inclus avec Mu peut ne pas être la plus récente !
    Vous pouvez trouver quelle version est installée en exécutant le code suivant::

        import pgzero
        print(pgzero.__version__)


Installation indépendante
-------------------------

Avant tout, vous avez besoin que **Python 3** soit installé ! Il est généralement déjà installé
si vous utilisez **Linux** ou un **Raspberry Pi**. Vous pouvez le télécharger depuis
`python.org <https://www.python.org/>`_ sur d'autres systèmes.


Windows
'''''''

Pour installer Pygame Zero, utiliser **pip**. Depuis une `invite de ligne de commande`__, entrez

.. __: https://www.lifewire.com/how-to-open-command-prompt-2618089

::

    pip install pgzero


Mac
'''

Dans une fenêtre de terminal, entrez

::

   pip install pgzero


Linux
'''''

Dans une fenêtre de terminal, entrez

::

   sudo pip install pgzero


Certains systèmes Linux l'appelle ``pip3``, si la commande d'au-dessus affiche un message
du genre ``sudo: pip: command not found`` essayez alors::

    sudo pip3 install pgzero

Parfois pip n'est pas installé. Si c'est le cas, essayer la commande suivante avant
de ré-exécuter la commande précédente::


    sudo python3 -m ensurepip


.. _install-repl:

Installer le REPL
-----------------

:doc:`le REPL de Pygame Zero <repl>` est une fonctionnalité optionnelle. Elle peut être activée
lors de l'installation avec ``pip`` en ajouteant ``pgzero[repl]`` à la commande pip
line::

    pip install pgzero[repl]

Si vous n'êtes pas sûre d'avoir le REPL déjà installé, vous pouvez toujours exécuter cette
commande (cela ne cassera rien s'il est déjà installé !).

