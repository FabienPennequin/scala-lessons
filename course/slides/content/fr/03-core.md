# Commencer la programmation

## Read-Eval-Print Loop (REPL)

Le moyen le plus rapide pour essayer Scala est d'utiliser le REPL (Read-Eval-Print-Loop)

```bash
$ sbt console
Starting scala interpreter...
Welcome to Scala 2.12.6 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152).
Type in expressions for evaluation. Or try :help.

scala> 
```

(Vérifier que [sbt](http://www.scala-sbt.org) est installé sur votre ordinateur)

# Valeurs et Types

## De quoi est fait un programme ?

- Un programme est l'**expression** d'une solution à un **problème** donné

- Nous avons besoin d'une façon d'exprimer et de se référer aux **éléments du problème**


## Les éléments les plus simple d'un programme : les valeurs litérales

> Quel est la réponse à “La grande question sur la vie, l'univers et le reste” ?

```scala
scala> 42
res0: Int = 42
```

> Quel est mon nom ?

```scala
scala> "Fabien"
res1: String = Fabien
```

- `42` et `"Fabien"` sont **valeurs** composées d'une seule **valeur litérale**.

## Valeurs composées : Les opérateurs

> Combien fait trois plus deux ?

```scala
scala> 3 + 2
res2: Int = 5
```

> Quel est le résultat de la concaténation des textes “Hello ” et “world!” ?

```scala
scala> "Hello " ++ "world!"
res3: String = Hello world!
```

- Les valeurs peuvent être combinées en utilisant des **opérateurs** pour construire des valeurs plus complexes.

## Valeurs composées : les appels de méthode

> Quelle est la taille du texte “Hello world!” ?

```scala
scala> "Hello world!".size
res4: Int = 12
```

- Les **méthodes** sont **appliquées** sur des valeurs en utilisant la **dot notation**.

> Quelle est la liste des nombre compris entre 1 et 10 ?

```scala
scala> 1.to(10)
res5: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

- Les méthodes peuvent avoir des **paramètres**. Ils sont fournis entre parenthèses.

## Les opérateurs sont des méthodes

En réalité, les opérateurs sont juste des méthodes utilisant un symbole comme nom :

```scala
scala> 3.+(2)
res6: Int = 5
```

La **notation infixe** peut être utilisé avec n'importe quel nom de méthode :

```scala
scala> 1 to 10
res7: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
```

- L'**unification** des méthodes et des opérateurs permet de rendre le langage plus simple.

## Exercices

- Utiliser la méthode `abs` pour récupérer la valeur absolue de `-42`.

- Utiliser la méthode `toUpperCase` pour obtenir le texte “Hello world!” en majuscules.

## Passons à un niveau d'abstraction supplémentaire : les valeurs non litérales

Jusqu'à présent, vous travailliez essentiellement avec des chiffres et du texte.
Mais comment définir des éléments de plus haut niveau, comme des **images** ?

```scala
scala> Circle(42)
res8: doodle.core.Circle = Circle(42.0)
```

`Circle` est un **constructeur** qui prend un paramètre (le rayon) et renvoie un objet représentant un cercle.

```scala
scala> Rectangle(30, 50)
res9: doodle.core.Rectangle = Rectangle(30.0, 50.0)
```

## Manipuler des images : Affichage et mise en page

- Afficher une image avec `draw`:

```scala
scala> draw(Rectangle(30, 50))

```

- Mettre en page les images avec `beside`, `above` and `under`:

```scala
scala> Rectangle(60, 100) beside Circle(30)
res10: doodle.core.Image = Beside(Rectangle(60.0,100.0),Circle(30.0))

scala> draw(Rectangle(60, 100) beside Circle(30))

```

## Types

- Toutes les valeurs ont un **type**:
    - `42` a le type `Int`,
    - `"foo"` a le type `String`.


- Les types permettent de **classer** les valeurs;
    - `0` et `42` sont des valeurs de type `Int`.

## Les types vous guide

- Le `type checking` est la étape qui vérifie la cohérence des types de votre programme.

- Cette vérification vous empêche de combiner des valeurs d'une mauvaise façon :

```scala
scala> 1 to "10"
<console>:20: error: type mismatch;
 found   : String("10")
 required: Int
              1 to "10"
                   ^
```

- L'exécution du programme est un processus en deux étapes :
    # Le compilateur transforme votre code scala en bytecode pour la JVM,
    # La JVM exécute le bycode
- Le `type checking` est effectué pendant la première étape : les erreurs sont détectées rapidement.


## Quelques types communs

- `Int` : entier signé de 32-bit
- `Double` : un nombre à virgule flottante de 64-bit IEEE-754 (exemple : `12.34`).
- `Boolean` : une valeur booléenne (`true` et `false`)
- `String` : un texte

## Le type qui représente l'absence de valeur

- Les méthodes comme `println` ou` draw` ne renvoient aucune valeur significative
- Au lieu de cela, ils effectuent des **effets de bord** (**side-effects** en anglais)
- Leur type de retour est `Unit`
- Il n'y a qu'une seule valeur de type `Unit` : `()`


## Exercice

- Dessiner un point d'exclamation.

![](images/exclamation.png)

## Exercise

- Dessiner un haltère.

![](images/barbell.png)

## Manipuler les images : les couleurs

Utiliser `fillColor` pour remplir une image avec une couleur donnée :

```scala
draw(Rectangle(30, 80) fillColor Color.black)
```

Exemples de couleurs disponibles : `red`, `blue`, `green`, `black`, `white`, `gray` and `brown`.

## Exercise

- Ajouter de la couleur à votre haltère.

![](images/barbell-color.png)

# Les définitions

## Définitions de valeurs

Considérer le programme suivant :

```scala
draw(
  (Rectangle(25, 100) fillColor Color.black) beside
  (Rectangle(200, 20) fillColor Color.grey) beside
  (Rectangle(25, 100) fillColor Color.black)
)
```

- Il est difficile à **lire** car l'expression est très grande
- Il est difficile à **maintenir** : si vous souhaitez modifier la taille des poids de l'haltère, vous devez modifier le code à **deux** endroits.

## Définitions de valeurs (2)

```scala
val weight = Rectangle(25, 100) fillColor Color.black

val bar = Rectangle(200, 20) fillColor Color.grey

draw(weight beside bar beside weight)
```

- Les deux premières lignes sont des **définitions de valeurs**.
    - Elles introduisent de nouveaux **noms** (`weight` et `bar`),
    - Elles **associent** ces noms aux valeurs à droite du symbole “=”.

- Les noms peuvent être utilisés à la place de n'importe quelle valeur.

- Le type des définitions est **inféré** par le compilateur.

## Définitions de valeurs (3)

Maintenant, modifier les poids de l'haltère nécessite de changer le code qu'à un seul endroit :

```scala
val weight = Rectangle(15, 100) fillColor Color.black

val bar = Rectangle(200, 20) fillColor Color.grey

draw(weight beside bar beside weight)
```

## Exercise

- Dessiner deux haltères l'une au-dessus de l'autre. La première doit avoir des poids de petite taille, tandis que le second doit avoir des poids très grands.

![](images/barbells.png)

## Définitions de méthodes

Considérer le programme suivant :

```scala
val heavyWeight = Rectangle(30, 100) fillColor Color.black
val smallWeight = Rectangle(15, 100) fillColor Color.black
val bar = Rectangle(200, 20) fillColor Color.grey

val smallBarbell = smallWeight beside bar beside smallWeight
val heavyBarbell = heavyWeight beside bar beside heavyWeight

draw(smallBarbell above heavyBarbell)
```

Notez les similitudes entre les définitions de `heavyWeight` et `smallWeight`. Comment pourriez-vous les rendre plus **générique** ?

## Définitions de méthodes (2)

```scala
def weight(width: Int) = Rectangle(width, 100) fillColor Color.black

val heavyWeight = weight(30)
val smallWeight = weight(15)
```

- La première ligne est une ***définition de méthode***. Contrairement aux défintions de valeurs, elle peut avoir des **paramètres**.
- Les paramètres sont spécifiés entre parenthèses et séparés par une virgule.
- Le type d'un paramètre doit être donnée explicitement.

## Principe d'abstraction

> Chaque fonctionnalité importante d'un programme ne doit être implémentée qu'à un seul endroit dans le code source.
>
> Lorsque des fonctions similaires sont réalisées par des éléments de code distincts, il est généralement bénéfique de les combiner en une seule, **en extrayant** les parties variables.

Benjamin C. Pierce. *Types and Programming Languages*. MIT Press 2002.

## Exercise

- Définir une méthode `barbell` qui prend en paramètre l'image d'un poids d'haltère et retourne l'image d'une haltère avec ce poids.

- Ré-écrire votre précédent programme en utilisant les méthodes `barbell` et `weight`.

<!--
def barbell(weight: Image) =
  weight beside (Rectangle(200, 20) fillColor Color.grey) beside weight

def weight(width: Int) = Rectangle(width, 100) fillColor Color.black

val heavyBarbell = barbell(weight(30))
val smallBarbell = barbell(weight(15))

draw(smallBarbell above heavyBarbell)
-->

## Les éléments de programmation

- Le langage de programmation nous permet :
    - d'écrire des valeurs littérales représentant des **éléments simples** (`42`,` "Hello" `, etc.)
    - de **combiner** ces valeurs (en utilisant des méthodes)
    - d'**abstraire** des valeurs, en introduisant un nom pour y faire référence
        - Ces noms peuvent eux-mêmes abstraire des parties de leur définition, en utilisant des paramètres
- Ces moyens d'abstraction et de composition vous permettent de généraliser les programmes et de les combiner


# Boucles et conditions

## Exercise

- Dessiner cinq haltères empilés les unes sur les autres.

## Boucles et conditions

Prenons le programme suivant :

```scala
val unit = barbell(weight(15))
val barbell1 = unit
val barbell2 = unit above barbell1
val barbell3 = unit above barbell2
val barbell4 = unit above barbell3
val barbell5 = unit above barbell4
```

Note : chaque valeur $barbell_n$ est construite en mettant une barre au-dessus de la valeur $barbell_{n - 1}$. Comment **généraliser** ce motif ?

## Boucles et conditions

Fondamentalement, nous voulons exprimer ce qui suit :

- $barbell_n =$
    - $unit$ if $n = 1$,
    - $unit$ $above$ $barbell_{n - 1}$ otherwise.

## Expressions conditionnelles

```scala
val unit = barbell(weight(15))
def barbells(n: Int) =
  if (n == 1) unit
  else unit above ???
```

- Vous pouvez écrire des **expressions conditionnelles** en utilisant `if` et `else`
- Le résultat d'un conditionnel dépend de la valeur `Boolean` de la condition

## Expressions booléennes

- Littéraux booléens : `true` and `false`
- Opérateur de comparaison : `42 == 42`, `42 != 0`, `42 > 0`
- Les expressions booléennes peuvent être combinées avec `||` (disjonction), `&&` (conjonction), et `!` (négation)
    - `true || false == !false`

## Méthodes récursives

```scala
val unit = barbell(weight(15))
def barbells(n: Int): Image =
  if (n == 1) unit
  else unit above barbells(n - 1)
```

- Le type de retour pour les méthodes **récursives** est obligatoire.

## Méthodes récursives (2)

```scala
scala> barbells(3)
res11: doodle.core.Image = Above(…)
```

- Quelles sont les étapes d'évaluation de l'expression `barbells(3)` ?

>   - Étapes d'évaluation :
>
>     ```scala
>     barbells(3)
>     if (3 == 1) unit else unit above barbells(3 - 1)
>     unit above barbells(2)
>     unit above (if (2 == 1) unit else unit above barbells(2 - 1))
>     unit above (unit above barbells(1))
>     unit above (unit above (if (1 == 1) unit else unit above barbells(1 - 1)))
>     unit above (unit above unit)
>     ```

## Exercise

- Définir une méthode `circles` qui prend un nombre `n` comme paramètre et renvoie une image montrant `n` cercles concentriques de tailles croissantes.

~~~ scala
def circles(n: Int): Image = ???
~~~

![](images/circles.png)

<!--
def circles(n: Int): Image = {
  val circle = Circle(25 + 15 * n)
  if (n == 1) circle
  else circle on circles(n - 1)
}
-->

# Définitions de niveau supérieure et portées lexicales
Top-Level Definitions and Lexical Scopes

## Définition d'objets

- Dans Scala, les méthodes et les valeurs doivent être définies dans une définition de niveau supérieur :

~~~ scala
object Loops {
  def circles(n: Int): Image = ???
}
~~~

- Ce code contient une **définition d'objet**, qui porte le nom `Loops`
- `Loops` se réfère à un **objet** avec une méthode `circles`
- La méthode `circles` peut être appelé de l'extérieur en utilisant la `dot notation` : `Loops.circles(42)`

## Packages

- Les définitions d'objets peuvent être organisées sous forme de **packages**:

~~~ scala
package core

object Loops { … }
~~~

~~~ scala
package std

object Lists { … }
~~~

## Visibilité des packages

~~~ scala
package core

object Loops {
  def circles(n: Int): Image = ???
}
~~~

~~~ scala
package core

object Definitions {
  Loops.circles(42)
}
~~~

- Les noms définis dans un package sont visibles dans tout le code défini dans le même package.

## Visibilité des packages (2)

~~~ scala
package std

object Lists {
  core.Loops.circles(42)
}
~~~

- Les noms définis dans un package ne sont pas visible dans le code des autres packages
- Ils doivent être **entièrement qualifiés** pour être résolus par le compilateur

## Imports

~~~ scala
package std

import core.Loops

object Lists {
  Loops.circles(42)
}
~~~

- L'**Import** permet d'accéder aux noms sans devoir les qualifier

## Blocs et définitions locales

~~~ scala
def barbells(n: Int): Image = {
  val unit = barbell(weight(15))
  if (n == 1) unit
  else unit above barbells(n - 1)
}
~~~

- Les méthodes et les valeurs peuvent également être définies dans un **bloc**
- Ces définitions locales ne sont pas visibles de l'extérieur du bloc

## Exercise

- Créer une méthode `spiral` qui prend en paramètre un nombre `n` et qui retourne une image avec `n` cercles de taille croissante et disposés en forme de spirale : 

![](images/spiral.png)

<!--
def spiral(n: Int): Image = {
  val size = 10 + n * 2
  val dist = 50 + n * 5
  val angle = Angle.degrees((n * 36) % 360)
  val circle = Circle(size).at(dist * angle.sin, dist * angle.cos)
  if (n == 1) circle else circle on spiral(n - 1)
}
-->

## Exercise

- Créer une méthode `sierpinski` qui prend en paramètre un nombre `n` et qui retourne une image avec `n` itérations du 
[Triangle Sierpiński](https://fr.wikipedia.org/wiki/Triangle_de_Sierpi%C5%84ski disposés en forme de spirale) :

![](images/sierpinski.png)

<!--
def sierpinski(n: Int): Image = {
  if(n == 1) {
    Triangle(10, 10) fillColor Color.black
  } else {
    val smaller = sierpinski(n - 1)
    smaller above (smaller beside smaller)
  }
}
-->

# En résumé

## Les définitions

~~~ scala
object <name> {
  <statements>
}
~~~

~~~ scala
val <name> = <expr>
~~~

~~~ scala
def <name>(<p1>, <p2>, …) = <expr>
~~~

## Les expressions

~~~ scala
if (<expr>) <expr> else <expr>
~~~

~~~ scala
<name>(<expr>, <expr>, …)
~~~

~~~ scala
<expr>.<name>(<expr>, …)
~~~

~~~ scala
{ <expr> ; … ; <expr> }
~~~

# Modèle d'évaluation

## Modèle d'évaluation

Considérez le programme suivant :

```scala
def foo = 1 + 1
val bar = 1 + 1
```

Quel est la différence entre `foo` et `bar` ?

> - La partie à droite de `def` est évaluée **à chaque fois** que le nom est appelé
>     - `foo` se réfère à l'expression `1 + 1`
> - La partie droite de `val` est évaluée **une seule fois**, au moment où elle est définie
>     - `bar` se réfère à la valeur `2`

## Termination

Peut-on garantir que l'évaluation va se terminer ?

```scala
def loop: Nothing = loop
```

<!--
## Linear and Tail Recursion

TODO
-->
