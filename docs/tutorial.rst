========
Tutoriel
========

.. TODO
.. 
..  - Comment indexer un array ou un dictionnaire?
..  - Comment définir des plages sur un array?  e.g. x[5:] ou y[2:10]
..  - Vous laisser sans voix avec les macros!
..  - [Where's my banana???|TOFIX]
..  - Mentionner l'interoperabilite entre fichier .hy et .py et vice et versa!

Bienvenue dans ce tutoriel pour la langage Hy!

Pour faire court, Hy est un dialecte Lisp, qui transpile sa structure vers
Python... littéralement convertissant en AST Python!
(En des termes moins gracieux, Hy, le cache-misère pour python [lisp-stick on a python! en Anglais])

Plutôt sympa car cela implique plusieurs choses a propos de Hy:

 - Un lisp très pythonic
 - Pour les lispeurs, un formidable moyen de combiner les immenses capacités de lisp dans le large
   écosystème de Python (oui, vous pouvez désormais écrire des webapps Django en lisp!)
 - Pour les pythonistas, ne pas sortir de sa zone de confort pour explorer lisp!
 - Pour les autres: un langage agréable plein de bonnes idées!


Introduction rapide a lisp pour les pythonistas
===============================================

Peut-être n'avez vous jamais utilise lisp auparavant, mais python oui!

En fait, un "hello world" en hy est très simple. Voyons voir:

.. code-block:: clj

   (print "hello world")

Vous avez bien vu?  Facile! Vous l'avez sûrement devine, c'est la même chose que ce qui suit en python::

  print "hello world"

Pour introduire un peu de maths basiques, on pourrait écrire:

.. code-block:: clj

   (+ 1 3)

Qui renverrait 4 en bon équivalent du:

.. code-block:: clj

   1 + 3

Ce qui saute aux yeux, c'est que le premier élément de la liste est la fonction a appeler
le reste étant les arguments fournis a cette dernière.
Plus généralement, en hy (comme dans la plupart des lisps) l'opérateur + accepte un nombre
variable d'arguments:

.. code-block:: clj

   (+ 1 3 55)

Qui renverrait 59.

Peut-être vous avez déjà entendu parle de lisp avant, sans en connaître plus a son sujet.
Lisp n'est pas aussi complique que vous pouvez l'imaginer, de plus hy hérite de python, ce qui en fait
un véhicule adéquat pour apprendre lisp.  Ce qui ressort tout suite de lisp
c'est le nombre élevé de parenthèses.  Cela peut rendre les choses confuses
au départ, mais en fait non.  Prenons une simple expression mathématique
pleine de parenthèses a saisir dans l'interprète hy:

.. code-block:: clj

   (setv result (- (/ (+ 1 3 88) 2) 8))

Ce qui renverrait 38.  Pourquoi donc?  Voila l'équivalent écrit en python cette fois:::
  
  result = ((1 + 3 + 88) / 2) - 8

Si vous aviez a évaluer l'exemple précèdent en python,
vous procéderiez en évaluant chaque sous-expression.
Hy ne fait pas exception a cette règle.  Commençons par la version
en python::

  result = ((1 + 3 + 88) / 2) - 8
  # simplifie en...
  result = (92 / 2) - 8
  # simplifie en...
  result = 46 - 8
  # simplifie en...
  result = 38

Et maintenant, même chose, en hy:

.. code-block:: clj

   (setv result (- (/ (+ 1 3 88) 2) 8))
   ; simplifie en...
   (setv result (- (/ 92 2) 8))
   ; simplifie en...
   (setv result (- 46 8))
   ; simplifie en...
   (setv result 38)

Comme vous l'avez probablement devine, l'expression "setv" signifie
assigner 38 a la variable "result".

Alors?  Pas si difficile!

C'est le principe premier de lisp... lisp est un portemanteau pour "list
processing"... la structure même du programme repose sur des listes imbriquées.
(Si vous connaissez les listes en python,
imaginez la structure ci-dessus, mais cette fois écrite avec des crochets,
cette structure permet de révéler sa dualité aussi bien en tant que programme que donnée.)
Avec un peu de pratique tout deviendra très clair,
alors commençons par écrire et tester un programme python très simple avant d'en dévoiler
la traduction en hy::

  def simple_conversation():
      print "Hello!  I'd like to get to know you.  Tell me about yourself!"
      name = raw_input("What is your name? ")
      age = raw_input("What is your age? ")
      print "Hello " + name + "!  I see you are " + age + " years old."
  
  simple_conversation()
  
L'exécution du programme ci-dessus nous donnerait la sortie suivante::

  Hello!  I'd like to get to know you.  Tell me about yourself!
  What is your name? Gary
  What is your age? 38
  Hello Gary!  I see you are 38 years old.

Et maintenant, l'équivalent en hy:

.. code-block:: clj

   (defn simple-conversation []
      (print "Hello!  I'd like to get to know you.  Tell me about yourself!")
      (setv name (raw_input "What is your name? "))
      (setv age (raw_input "What is your age? "))
      (print (+ "Hello " name "!  I see you are "
                 age " years old.")))

   (simple-conversation)

Pour lire cette traduction, gardez en tête que le premier élément de chacune de ses listes
est la fonction (ou macro... nous y viendrons ultérieurement)
appelée et les suivants ses arguments; il ne sera pas trop difficile de comprendre le sens de celle-ci.
(comme vous l'avez probablement devine, defn est le mot-clé pour définir des Fonctions)

Malgré tout, beaucoup sont troubles par tant de parenthèses, voila donc quelques conseils
pour rendre les choses plus aisées: pensez a indenter votre code, utilisez un éditeur
supportant le 'parenthesis matching [TOFIX]' (indique les paires de parenthèses visuellement)
, la tache vous en sera facilitée.

Avoir un source représente par une structure de donnée très simple telle que celle sur laquelle
repose le noyau de lisp possède certains avantages.
Premièrement, parser devient très simple, la structure complète du programme vous est présentée
de manière claire. (en hy il existe une étape supplémentaire de conversion vers un AST python...
dans d'autres lisps plus "pure" tels que Common Lips ou Emacs Lisp, la différence entre ce que
vous voyez et ce qui est exécute est moindre.)

Deuxièmement, les macros: avec un programme base sur une structure simple, écrire du code qui
écrit du code devient aise, et avec ça la possibilité d'ajouter de nouveaux traits linguistiques
très rapidement. Avant hy, les choses étaient bien moins simples pour les pythonistas... désormais
vous pouvez aussi profiter du pouvoir qu'elles procurent (évitez juste de viser votre pied)!


Hy est un lisp parfume au python (ou est-ce l'inverse?)
=======================================================

Hy cible l'AST de python, vous découvrirez rapidement que vous avez accès
a toutes les possibilités offertes par ce dernier.

Accès complet aux types de données et librairies standards de python en hy.
Testons ceci dans l'interprète hy::

  => [1 2 3]
  [1, 2, 3]
  => {"dog" "bark"
  ... "cat" "meow"}
  ...
  {'dog': 'bark', 'cat': 'meow'}
  => (, 1 2 3)
  (1, 2, 3)

(Vous constaterez qu'a ce moment même, la quotation de Common Lisp:

.. code-block:: clj

   '(1 2 3)

ne fonctionne pas. Utilisez plutôt les crochets comme dans l'exemple ci-dessus.)

Vous avez aussi accès aux méthodes des types primitifs::

  => (.strip " fooooo   ")
  "fooooo"

Qu'est-ce dont ? Oui, la même choses que::

  " fooooo   ".strip()

Exactement... lisp avec la 'dot notation'[TOFIX]!  Si cette chaîne est liée a une variable
on peut aussi saisir ce qui suit:

.. code-block:: clj

   (setv this-string " fooooo   ")
   (this-string.strip)

Et les conditionnelles ?:

.. code-block:: clj

   (if (try-some-thing)
     (print "this is if true")
     (print "this is if false"))

Le premier argument est un test logique, ensuite l'expression a évaluer si vrai,
puis celle si faux. Cette clause 'else' est optionnelle.

Pour des conditionnelles plus complexes, il n'y pas de forme 'elif' en hy. Son équivalent
s'appelle 'cond'. En python, ce qui vous écririez ainsi::

  somevar = 33
  if somevar > 50:
      print "That variable is too big!"
  elif somevar < 10:
      print "That variable is too small!"
  else:
      print "That variable is jussssst right!"

donnerait en hy:

.. code-block:: clj

   (cond
    [(> somevar 50)
     (print "That variable is too big!")]
    [(< somevar 10)
     (print "That variable is too small!")]
    [true
     (print "That variable is jussssst right!")])

On voit ici que cond .... [TOFIX] entre une expression dont on évalue
sa valeur de vérité (vraie ou fausse) et une expression résultante si
cette dernière est vraie. Accessoirement, la notion de clause 'else' est
implémentée en testant 'true'. Cette tautologie implique qu'il y aura
toujours au moins une clause évaluée.
     
.. [Pour le TOFIX précèdent]
   What you'll notice is that cond switches off between a some statement
   that is executed and checked conditionally for true or falseness, and
   then a bit of code to execute if it turns out to be true.  You'll also
   notice that the "else" is implemented at the end simply by checking
   for "true"... that's because true will always be true, so if we get
   this far, we'll always run that one!

Dans un cas du style:   
   
.. code-block:: clj

   (if some-condition
     (body-if-true)
     (body-if-false))

Si on désire exécuter plusieurs instructions dans le corps des branches,
comment faire ?

Voici l'idiome adéquat en hy:

.. code-block:: clj

   (if (try-some-thing)
     (do
       (print "this is if true")
       (print "and why not, let's keep talking about how true it is!))
     (print "this one's still simply just false"))

La forme "do" enrobe plusieurs expressions évaluées en séquence.
Les habitues de lisps reconnaîtront en 'do' un équivalent de la forme
historique 'progn'.
Cette enrobage d'apparence superflu vient de la culture lisp, les séquences
d'instructions sont reléguées au second plans. On y favorise l'évaluation d'expression
imbriquées. [TOFIX|WEENY-COMMENT]

Les commentaires commencent par un point-virgule:

.. code-block:: clj

  (print "this will run")
  ; (print "but this will not")
  (+ 1 2 3)  ; we'll execute the addition, but not this comment!

Boucler n'est pas complique, mais est exprime différemment. En python,
on écrirait::
  
  for i in range(10):
      print "'i' is now at " + str(i)

Dont l'équivalent en hy serait:

.. code-block:: clj

  (for (i (range 10))
     (print (+ "'i' is now at " (str i))))


Vous pouvez aussi importer et utiliser les librairies python. Par exemple:

.. code-block:: clj

   (import os)
  
   (if (os.path.isdir "/tmp/somedir")
     (os.mkdir "/tmp/somedir/anotherdir")
     (print "Hey, that path isn't there!"))

Les gestionnaires de contexte (context managers) python ('with')
s'utilisent de la façon suivante:

.. code-block:: clj 
 
     (with [f (file "/tmp/data.in")] 
       (print (.read f))) 

traduction du code python::

  with file("/tmp/data.in") as f:
    print f.read()
 
Bien sur, il on peut exprimer des listes en compréhension!
Ce qu'en python serait écrit::

  odds_squared = [
    pow(num, 2)
    for num in range(100)
    if num % 2 == 1]

En hy, donnerait:

.. code-block:: clj

  (setv odds-squared
    (list-comp
      (pow num 2)
      (num (range 100))
      (= (% num 2) 1)))


.. code-block:: clj

  ; Ici, un exemple honteusement repris d'une doc Clojure:
  ; Listons toutes les cases d'un jeu d'échec.
  
  (list-comp
    (, x y)
    (x (range 8)
     y "ABCDEFGH"))
  
  ; [(0, 'A'), (0, 'B'), (0, 'C'), (0, 'D'), (0, 'E'), (0, 'F'), (0, 'G'), (0, 'H'),
  ;  (1, 'A'), (1, 'B'), (1, 'C'), (1, 'D'), (1, 'E'), (1, 'F'), (1, 'G'), (1, 'H'),
  ;  (2, 'A'), (2, 'B'), (2, 'C'), (2, 'D'), (2, 'E'), (2, 'F'), (2, 'G'), (2, 'H'),
  ;  (3, 'A'), (3, 'B'), (3, 'C'), (3, 'D'), (3, 'E'), (3, 'F'), (3, 'G'), (3, 'H'),
  ;  (4, 'A'), (4, 'B'), (4, 'C'), (4, 'D'), (4, 'E'), (4, 'F'), (4, 'G'), (4, 'H'),
  ;  (5, 'A'), (5, 'B'), (5, 'C'), (5, 'D'), (5, 'E'), (5, 'F'), (5, 'G'), (5, 'H'),
  ;  (6, 'A'), (6, 'B'), (6, 'C'), (6, 'D'), (6, 'E'), (6, 'F'), (6, 'G'), (6, 'H'),
  ;  (7, 'A'), (7, 'B'), (7, 'C'), (7, 'D'), (7, 'E'), (7, 'F'), (7, 'G'), (7, 'H')]


Python supporte passage d'arguments sophistiques, vous pourriez voir ceci::

  >>> def optional_arg(pos1, pos2, keyword1=None, keyword2=42):
  ...   return [pos1, pos2, keyword1, keyword2]
  ... 
  >>> optional_arg(1, 2)
  [1, 2, None, 42]
  >>> optional_arg(1, 2, 3, 4)
  [1, 2, 3, 4]
  >>> optional_arg(keyword1=1, pos2=2, pos1=3, keyword2=4)
  [3, 2, 1, 4]

La même chose en Hy::

  => (defn optional_arg [pos1 pos2 &optional keyword1 [keyword2 42]]
  ...  [pos1 pos2 keyword1 keyword2])
  => (optional_arg 1 2)
  [1 2 None 42]
  => (optional_arg 1 2 3 4)
  [1 2 3 4]
  => (kwapply (optional_arg)
  ...         {"keyword1" 1
  ...          "pos2" 2
  ...          "pos1" 3
  ...          "keyword2" 4})
  ... 
  [3, 2, 1, 4]

Vous avez remarque l'usage de kwapply pour appréhender l'appel le plus sophistique? :)

Une syntaxe orientée dictionnaire existe, voici à quoi elle ressemble:

.. code-block:: clj

  (defn another_style [&key {"key1" "val1" "key2" "val2"}]
    [key1 key2])

La différence étant ici qu'un dictionnaire ne vous permet pas d'ordonner
les arguments.

Hy accepte aussi ``*args`` et ``**kwargs``.  En Python::

  def some_func(foo, bar, *args, **kwargs):
    import pprint
    pprint.pprint((foo, bar, args, kwargs))

L'équivalent en Hy:

.. code-block:: clj

  (defn some_func [foo bar &rest args &kwargs kwargs]
    (import pprint)
    (pprint.pprint (, foo bar args kwargs)))

Pour finir, n'oublions pas les classes!  En python nous pourrions déclarer
une classe de la façon suivante::

  class FooBar (object):
     def __init__(self, x):
         self.x = x

     def get_x(self):
         return self.x


En Hy:

.. code-block:: clj

  (defclass FooBar [object]
    [[--init--
      (fn [self x]
        (setv self.x x)
        ; Currently needed for --init-- because __init__ needs None
        ; Hopefully this will go away :)
        None)]
  
     [get-x
      (fn [self]
        self.x)]])


Les attributs de classe sont aussi permis.  En Python::

  class Customer(models.Model):
      name = models.CharField(max_length=255)
      address = models.TextField()
      notes = models.TextField()

En Hy:

.. code-block:: clj

  (defclass Customer [models.Model]
    [[name (kwapply (models.CharField) {"max_length" 255})]
     [address (models.TextField)]
     [notes (models.TextField)]])


Protips!
========

Hy dispose également de "threading macro", macro d'en-filetage s'il
fallait tenter une francisation, empruntée a l'écosystème Clojure.
La "threading macro" (écrite "->"), est employée pour s'abstraire
des imbrication trop profondes.

La threading macro passe chaque expression en tant que premier argument de
l'expression suivante.

Prenons le classique:

.. code-block:: clj

    (loop (print (eval (read))))

Plutôt que de l'écrire ainsi, on peut utiliser l'idiome suivant:

.. code-block:: clj

    (-> (read) (eval) (print) (loop))

A présent, muni de `python-sh <http://amoffat.github.com/sh/>`_, on peut 
constater comment la threading macro (grâce a la conception de python-sh)
peut être utilisée tel un pipe:

.. code-block:: clj

    => (import [sh [cat grep wc]])
    => (-> (cat "/usr/share/dict/words") (grep "-E" "^hy") (wc "-l"))
    210

Qui, bien sur, est expansée en:

.. code-block:: clj

    (wc (grep (cat "/usr/share/dict/words") "-E" "^hy") "-l")

La lisibilité en est améliorée, non? Utilisez la threading macro!
