# Modéliser le monde

## Introduction

- Considérons la méthode `barbell` qui créé une haltère :

~~~ scala
def barbell(load: Int, length: Int): Image = {
  val weight = Rectangle(load, 100) fillColor Color.black
  val bar = Rectangle(length, 20) fillColor Color.grey
  weight beside bar beside weight
}

val lightBarbell = barbell(10, 180)
val heavyBarbell = barbell(20, 200)
~~~

- Vous ne pouvez pas faire grand chose avec vos haltères à part les dessiner

## Introduction (2)

- Comment définir une haltère plus lourde qu'une autre ?

~~~ scala
def weigh(barbell: Image): Image = ???
~~~

> - La modélisation des haltères avec des images rend difficile leur manipulation

## Introduction (3)

- Comment définir une haltère plus lourde qu'une autre ?

~~~ scala
def weighLoad(load: Int) = load + 10
def weighLength(length: Int) = length + 20

val lightBarbellLoad = 10
val lightBarbellLength = 180

val heavyBarbellLoad = weighLoad(lightBarbellLoad)
val heavyBarbellLength = weighLength(lightBarbellLength)
~~~

> - Cette approche est compliqué en terme d'utilisation.

## Définition de Case Class

~~~ scala
case class Barbell(load: Int, length: Int) {
  def weigh: Barbell = Barbell(load + 10, length + 20)
}

val lightBarbell = Barbell(10, 180)
val heavyBarbell = lightBarbell.weigh
~~~

- Ce code contient une **définition de case class** qui introduit :
    - the **type** `Barbell`,
    - the **constructeur** `Barbell`

## Définition de Case Class (2)

~~~ scala
case class Barbell(load: Int, length: Int) {
  def weigh: Barbell = Barbell(load + 10, length + 20)
}

val lightBarbell = Barbell(10, 180)
val heavyBarbell = lightBarbell.weigh
~~~

- Le type `Barbell` a trois **membres**: `load`, `length` and `weigh`
- `lightBarbell` et `heavyBarbell` sont des **instances** de `Barbell`

## Exercise

- Implémenter la méthode suivante :

~~~ scala
def barbellImage(barbell: Barbell): Image = ???
~~~

## Exercise

- Ajouter une méthode `lighten` au type `Barbell`

## Exercise

- Un haltère est des accessoires de fitness possibles
- Considerons également un tapsi comme un autre type d'appareil de fitness:

![](mat.png)

- Un tapis à une largeur (`width`) et une longueur (`length`). Définir une `case class` pour lui.

## Exercise

- Supposons que dans votre monde, il n'existe que deux accessoires de fitness : un tapis et un haltère
- Écrire une méthode `fitnessDeviceImage` qui prend en paramètre un accessoire de fitness et retourne son image

## Variantes de type

- Comment modéliser qu'un appareil de fitness peut être soit un tapis, soit un haltère et rien d'autre ?

~~~ scala
sealed trait FitnessDevice
case class Barbell(load: Int, length: Int) extends FitnessDevice
case class Mat(width: Int, length: Int) extends FitnessDevice
~~~

- Ce code contient la **définition d'un sealed trait**, qui introduit le type `FitnessDevice` (mais **sans** constructeur)
- Les définitions de `case class` qui **étendent** `FitnessDevice` définissent les **variantes** de `FitnessDevice`
    - The variants of a sealed type must be defined within the same source file

## Pattern Matching

Vous pouvez manipuler les `case classes` et les `sealed traits` en utilisant le **pattern matching**:

~~~ scala
def name(fitnessDevice: FitnessDevice): String = fitnessDevice match {
  case Barbell(load, length) => "Barbell"
  case Mat(width, length) => "Mat"
}
~~~

## Exercise

- Implémenter la méthode suivante :

~~~ scala
def fitnessDeviceImage(fitnessDevice: FitnessDevice): Image = ???
~~~

## Modéliser le monde

La combinaison de `case class`et de `sealed trait`permet de modéliser l'information en terme de relation **est** ou **a** :

- "un accessoire de fitnes *est* un haltère *ou* un tapis"
    - Le _sealed trait_ `FitnessDevice` est étendu par `Barbell` and `Mat`
- "un haltère *a* un poids *et* et une longueur"
    - La _case class_ `Barbell` a les membres `load` et `length`
- "un tapis *a* une largeur *et* une longueur"
    - La _case class_ `Mat` a les membres `width` et `length`

## Exercise

- Modéliser une domaine suivante en scala :
    - Une forme géométrique *est* une cercle *ou* un rectangle *ou* un triangle,
    - Un cercle *a* un rayon,
    - Un rectangle *a* une largeur (`width`) et une longueur (`height`)
    - Un triangle *a* une largeur (`width`) et une hauteur (`height`)

## Exercise

- Modéliser une domaine suivante en scala :
    - Un cours *a* un nom *et* un niveau de difficulté
    - Un niveau de difficulté *est* soit “débutant”, “intermédiaire” *ou* “avancé”

- Peut-on définir un cours appelé “Programmer en Scala” destiné aux débutants ?

## Case Objects

~~~ scala
sealed trait DifficultyLevel
case class Beginner() extends DifficultyLevel
case class Intermediate() extends DifficultyLevel
case class Advanced() extends DifficultyLevel
~~~

Lorsqu'une `case class` n'a pas de paramètre, nous pouvons simplement utilisé un `case object` à la place :

~~~ scala
sealed trait DifficultyLevel
case object Beginner extends DifficultyLevel
case object Intermediate extends DifficultyLevel
case object Advanced extends DifficultyLevel
~~~

## Exercise

- Modéliser le domaine suivante en scala :
    - Une expression est un nombre ou une addition 
    - Un nombre a une valeur numérique
    - Une addition a une expression à droite et à gauche

## En résumé

~~~ scala
case class <name>(<member1>, <member2>, …)
~~~

~~~ scala
sealed trait <name1>
case class <name2>(…) extends <name1>
case class <name3>(…) extends <name1>
…
~~~

~~~ scala
<expr> match {
  case <name2>(…) => <expr>
  case <name3>(…) => <expr>
}
~~~
