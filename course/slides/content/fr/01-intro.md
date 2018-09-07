% Scala Course
% Fabien Pennequin, Fabernovel Technologies

# Overview

## Pourquoi Scala ?

> A problem **well put** is **half solved**.

John Dewey


> A **scalable** programming language is one in which the **same** concepts can describe **small** as well as **large** parts.

Martin Odersky


## Scala en quelques mots

Scala est un **langage de programmation généraliste**

Scala est **orienté objet**

Scala est **statiquement typé**

Scala permet à la fois la **programmation fonctionnelle** et la **programmation impérative**

## Hello World

```scala
object Main extends App {
  println("Hello, World!")
}
```

## Syntaxe flexible

```scala
"The 'Hello world' string" should {
  "contain 11 characters" in {
    "Hello world" must have size (11)
  }
}
```

```scala
class PingPong extends Actor {
  def receive = {
    case x => sender ! x
  }
}
```

```scala
class ExprParser extends RegexParsers {
  def factor = "[0-9]+".r | "("~expr~")"
  def term = factor~("*" | "/")~factor
  def expr = term~("+" | "-")~term
}
```
