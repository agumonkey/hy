===============
 Hacker hy
===============

.. highlight:: bash

Rejoignez notre ruche (hyve en anglais)!
==============

Venez hacker hy!

Passez nous voir a ``#hy`` sur ``irc.freenode.net``!

Parlez-en sur Twitter avec le hashtag ``#hy`` !

Bloggez-en [TOFIX]!

   Ne le taggez  pas sur le mur du voisin (sauf accord préalable)!


Hacker!
=======

Procédez a ce qui suit:

1. créez un `environnement virtuel (virtual environment en anglais)
   <https://pypi.python.org/pypi/virtualenv>`_::

       $ virtualenv venv

   et activez le::

       $ . venv/bin/activate

   ou bien utilisez `virtualenvwrapper <http://virtualenvwrapper.readthedocs.org/en/latest/#introduction>`_
   pour gérer vos environnements ::

       $ mkvirtualenv hy
       $ workon hy

2. récupérez le code source::

       $ git clone https://github.com/hylang/hy.git

   ou bien votre fork::

       $ git clone git@github.com:<YOUR_USERNAME>/hy.git
3. installez le::

       $ cd hy/
       $ pip install -e .

4. installez les modules nécessaires au développement::

       $ pip install -r requirements-dev.txt

5. Codez des choses extraordinaires; faites hurlez quelqu'un d'émoi/horreur au fruit de votre
   labeur.


Tester!
=======

Les tests se trouvent dans ``tests/``. Nous utilisons `nose
<https://nose.readthedocs.org/en/latest/>`_.

Pour lancer les tests::

    $ nosetests

Écrivez des tests---tests are good[TOFIX]!

De plus, il est bon de faire tourner les tests sur toutes les plates-formes supportées ainsi
que de vérifier la conformité pep8::

    $ tox

Documenter!
===========

La documentation se trouve dans ``docs/``. Nous utilisons `Sphinx
<http://sphinx-doc.org/>`_.

Pour générer la documentation en HTML::

    $ cd docs
    $ make html

Écrivez de la documentation---docs are good[TOFIX]! Même celle-ci!


Règles de développement du Noyau (Core en anglais)
==================================================

Toute soumission se doit d'être acceptée (ACK) par 2 membres différents de l'équipe
de base d'Hylang. Toute aide supplémentaire est évidemment la bienvenue, mais
il faut un minimum de 2 approbations pour une soumission.

Si un membre de base envoie un PR (Pull Request), veuillez en trouver 2 autres.
L'idée étant que le premier peut travailler avec l'auteur de la PR, et le second
accepte le `change set` en entier.

Si le patch concerne la documentation, n'hésitez pas a merge avec un seul ACK.
La couverture est baisse, autant éviter de mettre trop de barrières.


Équipe de base
=========

L'équipe de base de hy est constituée des développeurs suivants.

.. include:: coreteam.rst
