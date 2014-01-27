========
Tutoriel
========

.. TODO
.. 
..  - Comment indexer un array ou un dictionnaire?
..  - Comment definir des plages sur un array?  e.g. x[5:] ou y[2:10]
..  - Vous laisser sans voix avec les macros!
..  - [Where's my banana???|TOFIX]
..  - Mentionner l'interoperabilite entre fichier .hy et .py et vice et versa!

Bienvenue dans ce tutoriel pour la langage Hy!

Pour faire court, Hy est un dialecte Lisp, qui transpile sa structure vers
Python... literalement convertissant en AST Python!
(En des termes moins gracieux, Hy, le cache-misere pour python [lisp-stick on a python! en Anglais])

Plutot sympa car cela implique plusieurs choses a propros de Hy:

 - Un lisp tres pythonic
 - Pour les lispeurs, un formidable moyen de combiner les immensese capacites de lisp dans le large
   ecosysteme de Python (oui, vous pouvez desormais ecrire des webapps Django en lisp!)
 - Pour les pythonistas, ne pas sortir de sa zone de comfort pour explorer lisp!
   comfort of python!
 - Pour les autres: un langage agreable plein de bonnes idees!


Introduction rapide a lisp pour les pythonistas
===============================================

Peut-etre n'avez vous jamais utilise lisp auparavant, mais python oui!

En fait, un "hello world" en hy est tres simple. Voyons voir:

.. code-block:: clj

   (print "hello world")

Vous avez bien vu?  Facile! Vous l'avez surement devine, c'est la meme chose que ce qui suit en python::

  print "hello world"

Pour introduire un peu de maths basiques, on pourrait ecrire:

.. code-block:: clj

   (+ 1 3)

Qui renverrait 4 en bon equivalent du:

.. code-block:: clj

   1 + 3

Ce qui saute aux yeux, c'est que le premier element de la liste est la fonction a appeler
le reste etant les arguments fournis a cette derniere.
Plus generalement, en hy (comme dans la plupart des lisps) l'operateur + accepte un nombre
variable d'arguments:

.. code-block:: clj

   (+ 1 3 55)

Qui renverrait 59.

Peut-etre vous avez deja entendu parle de lisp avant, sans en connaitre plus a son sujet.
Lisp n'est pas aussi complique que vous pouvez l'imaginer, de plus hy herite de python, ce qui en fait
un vehicule adequat pour apprendre lisp.  Ce qui ressort tout suite de lisp
c'est le nombre eleve de parentheses.  Cela peut rendre les choses confuses
au depart, mais en fait non.  Prenons une simple expression mathematique
pleine de parentheses a saisir dans l'interprete hy:

.. code-block:: clj

   (setv result (- (/ (+ 1 3 88) 2) 8))

Ce qui renverrait 38.  Pourquoi donc?  Voila l'equivalent ecrit en python cette fois:::
  
  result = ((1 + 3 + 88) / 2) - 8

Si vous aviez a evaluer l'exemple precedent en python,
vous procederiez en evaluant chaque sous-expression.
Hy ne fait pas exception a cette regle.  Commencons par la version
en python::

  result = ((1 + 3 + 88) / 2) - 8
  # simplifie en...
  result = (92 / 2) - 8
  # simplifie en...
  result = 46 - 8
  # simplifie en...
  result = 38

Et maintenant, meme chose, en hy:

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

C'est le principe premier de lisp... lisp est un porte-manteau pour "list
processing"... la structure meme du programme repose sur des listes imbriquees.
(Si vous connaissez les listes en python,
imaginez la structure ci-dessus, mais cette fois ecrite avec des crochets,
cette structure permet de reveler sa dualite aussi bien en tant que programme que donnee.)
Avec un peu de pratique tout deviendra tres clair,
alors commencons par ecrire et tester un programme python tres simple avant d'en devoiler
la traduction en hy::

  def simple_conversation():
      print "Hello!  I'd like to get to know you.  Tell me about yourself!"
      name = raw_input("What is your name? ")
      age = raw_input("What is your age? ")
      print "Hello " + name + "!  I see you are " + age + " years old."
  
  simple_conversation()
  
L'execution du programme ci-dessus nous donnerait la sortie suivante::

  Hello!  I'd like to get to know you.  Tell me about yourself!
  What is your name? Gary
  What is your age? 38
  Hello Gary!  I see you are 38 years old.

Et maintenant, l'equivalent en hy:

.. code-block:: clj

   (defn simple-conversation []
      (print "Hello!  I'd like to get to know you.  Tell me about yourself!")
      (setv name (raw_input "What is your name? "))
      (setv age (raw_input "What is your age? "))
      (print (+ "Hello " name "!  I see you are "
                 age " years old.")))

   (simple-conversation)

Pour lire cette traduction, gardez en tete que le premier element de chacune de ses listes
est la fonction (ou macro... nous y viendrons ulterieurement)
appelee et les suivants ses arguments; il ne sera pas trop difficile de comprendre le sens de celle-ci.
(comme vous l'avez probablement devine, defn est le mot-cle pour definir des FoNctions)

Malgre tout, beaucoup sont troubles par tant de parentheses, voila donc quelques conseils
pour rendre les choses plus aisees: pensez a indenter votre code, utilisez un editeur
supportant le 'parenthesis matching [TOFIX]' (indique les paires de parentheses visuellement)
, la tache vous en sera facilitee.

Avoir un source represente par une structure de donnee tres simple telle que celle sur laquelle
repose le noyau de lisp possede certains avantages.
Premierement, parser devient tres simple, la structure complete du programme vous est presentee
de maniere claire. (en hy il existe une etape supplementaire de conversion vers un AST python...
dans d'autres lisps plus "pure" tels que Common Lips ou Emacs Lisp, la difference entre ce que
vous voyez et ce qui est execute est moindre.)

Deuxiemement, les macros: avec un programme base sur une structure simple, ecrire du code qui
ecrit du code devient aise, et avec ca la possibilite d'ajouter de nouveaux traits linguistiques
tres rapidement. Avant hy, les choses etaient bien moins simples pour les pythonistas... desormais
vous pouvez aussi profiter du pouvoir qu'elles procurent (evitez juste de viser votre pied)!


Hy est un lisp parfume au python (ou est-ce l'inverse?)
=======================================================

Hy cible l'AST de python, vous decouvrerez rapidement que vous avez acces
a toutes les possibilites offertes par ce dernier.

Acces complet aux types de donnees et librairies standards de python en hy.
Testons ceci dans l'interprete hy::

  => [1 2 3]
  [1, 2, 3]
  => {"dog" "bark"
  ... "cat" "meow"}
  ...
  {'dog': 'bark', 'cat': 'meow'}
  => (, 1 2 3)
  (1, 2, 3)

(Vous constaterez qu'a ce moment meme, la quotation de Common Lisp:

.. code-block:: clj

   '(1 2 3)

ne fonctionne pas. Utilisez plutot les crochets comme dans l'exemple ci-dessus.)

Vous avez aussi acces aux methods des types primitifs::

  => (.strip " fooooo   ")
  "fooooo"

Qu'est-ce dont ? Oui, la meme choses que::

  " fooooo   ".strip()

Exactement... lisp avec la 'dot notation'[TOFIX]!  Si cette chaine est liee a une variable
on peut aussi saisir ce qui suit:

.. code-block:: clj

   (setv this-string " fooooo   ")
   (this-string.strip)

Et les conditionnelles ?:

.. code-block:: clj

   (if (try-some-thing)
     (print "this is if true")
     (print "this is if false"))

Le premier argument est un test logique, ensuite l'expression a evaluer si vrai,
puis celle si faux. Cette clause 'else' est optionnelle.

Pour des conditionnelles plus complexes, il n'y pas de forme 'elif' en hy. Son equivalent
s'appelle 'cond'. En python, ce qui vous ecririez ainsi::

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

On voit ici que cond .... [TOFIX] entre une expression dont on evalue
sa valeur de verite (vraie ou fausse) et une expression resultante si
cette derniere est vraie. Accessoirement, la notion de clause 'else' est
implementee en testant 'true'. Cette tautologie implique qu'il y'aura
toujours au moins une clause evaluee.
     
.. [Pour le TOFIX precedent]
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

Si on desire executer plusieurs instructions dans le corps des branches,
comment faire ?

Voici l'idiome adequat en hy:

.. code-block:: clj

   (if (try-some-thing)
     (do
       (print "this is if true")
       (print "and why not, let's keep talking about how true it is!))
     (print "this one's still simply just false"))

La forme "do" enrobe plusieurs expressions evaluees en sequence.
Les habitues de lisps reconnaitront en 'do' un equivalent de la forme
historiue 'progn'.
Cette enrobage d'apparence superflu vient de la culture lisp, les sequences
d'instructions sont releguees au second plans. On y favorise l'evaluation d'expression
imbriquees. [TOFIX|WEENY-COMMENT]

Les commentaires commencent par un point-virgule:

.. code-block:: clj

  (print "this will run")
  ; (print "but this will not")
  (+ 1 2 3)  ; we'll execute the addition, but not this comment!

Boucler n'est pas complique, mais est exprime differemment. En python,
on ecrirait::
  
  for i in range(10):
      print "'i' is now at " + str(i)

Dont l'equivalent en hy serait:

.. code-block:: clj

  (for (i (range 10))
     (print (+ "'i' is now at " (str i))))


Vous pouvez aussi importer et utiliser les librairies python. Par exemple:

.. code-block:: clj

   (import os)
  
   (if (os.path.isdir "/tmp/somedir")
     (os.mkdir "/tmp/somedir/anotherdir")
     (print "Hey, that path isn't there!"))

Les gestionnaires de context (context managers) python ('with')
s'utilisent de la facon suivante:

.. code-block:: clj 
 
     (with [f (file "/tmp/data.in")] 
       (print (.read f))) 

traduction du code python::

  with file("/tmp/data.in") as f:
    print f.read()
 
Bien sur, il on peut exprimer des listes en comprehension!
Ce qu'en python serait ecrit::

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
  ; Listons toutes les cases d'un jeu d'echec.
  
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


Python supporte
  
Python has support for various fancy argument and keyword arguments.
In python we might see::

  >>> def optional_arg(pos1, pos2, keyword1=None, keyword2=42):
  ...   return [pos1, pos2, keyword1, keyword2]
  ... 
  >>> optional_arg(1, 2)
  [1, 2, None, 42]
  >>> optional_arg(1, 2, 3, 4)
  [1, 2, 3, 4]
  >>> optional_arg(keyword1=1, pos2=2, pos1=3, keyword2=4)
  [3, 2, 1, 4]

The same thing in Hy::

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

See how we use kwapply to handle the fancy passing? :)

There's also a dictionary-style keyword arguments construction that
looks like:

.. code-block:: clj

  (defn another_style [&key {"key1" "val1" "key2" "val2"}]
    [key1 key2])

The difference here is that since it's a dictionary, you can't rely on
any specific ordering to the arguments.

Hy also supports ``*args`` and ``**kwargs``.  In Python::

  def some_func(foo, bar, *args, **kwargs):
    import pprint
    pprint.pprint((foo, bar, args, kwargs))

The Hy equivalent:

.. code-block:: clj

  (defn some_func [foo bar &rest args &kwargs kwargs]
    (import pprint)
    (pprint.pprint (, foo bar args kwargs)))

Finally, of course we need classes!  In python we might have a class
like::

  class FooBar (object):
     def __init__(self, x):
         self.x = x

     def get_x(self):
         return self.x


In Hy:

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


You can also do class-level attributes.  In Python::

  class Customer(models.Model):
      name = models.CharField(max_length=255)
      address = models.TextField()
      notes = models.TextField()

In Hy:

.. code-block:: clj

  (defclass Customer [models.Model]
    [[name (kwapply (models.CharField) {"max_length" 255})]
     [address (models.TextField)]
     [notes (models.TextField)]])


Protips!
========

Hy also features something known as the "threading macro", a really neat
feature of Clojure's. The "threading macro" (written as "->"), is used
to avoid deep nesting of expressions.

The threading macro inserts each expression into the next expression's first
argument place.

Let's take the classic:

.. code-block:: clj

    (loop (print (eval (read))))

Rather then write it like that, we can write it as follows:

.. code-block:: clj

    (-> (read) (eval) (print) (loop))

Now, using `python-sh <http://amoffat.github.com/sh/>`_, we can show
how the threading macro (because of python-sh's setup) can be used like
a pipe:

.. code-block:: clj

    => (import [sh [cat grep wc]])
    => (-> (cat "/usr/share/dict/words") (grep "-E" "^hy") (wc "-l"))
    210

Which, of course, expands out to:

.. code-block:: clj

    (wc (grep (cat "/usr/share/dict/words") "-E" "^hy") "-l")

Much more readable, no? Use the threading macro!
