
# Créer des programmes autonomes

## Créer un programme autonome à partir d'un fichier source

- Écrire le programme suivant dans un fichier nommé `Main.scala`:

```scala
object Main extends App {

  println("Hello world!")

}
```

## Créer un programme autonome à partir d'un fichier source (2)

- Compiler le :

```bash
$ scalac Main.scala
```

(ceci devrait créer des fichiers `*.class`)

- Éxecuter le :

```bash
$ scala Main
"Hello world!"
```

## Processus de build

- Que se passe-t-il si votre programme contient 100 fichiers sources ?

- Comment recompiler uniquement les sources impactées par une modification donnée ?

- Que se passe-t-il si votre projet dépend d'une librairie tierce ?

- Comment gérer le cycle de vie de votre projet (tests, packaging, publication, ...) ?

## sbt

[sbt](http://www.scala-sbt.org/) est un outil de build pour les projets écrits en scala.

## sbt : Démarrage rapide 

- Lancer la commande `$ sbt <command>` depuis le dossier de votre projet pour éxecuter une commande sbt depuis votre _shell_

- Lancer `$ sbt` depuis le dossier de votre projet pour ouvrir le prompt sbt

- Lancer des commandes sbt depuis le prompt

    - `> run` pour lancer votre projet
    - `> console` pour ouvrir le REPL scala dans le contexte de votre projet
    - `> test` pour lancer les tests de votre projet
    - `> compile` pour compiler votre projet sans le lancer.

## sbt : Démarrage rapide  (2)

- Le processus de _build_ du projet est défini dans un fichier nommé `build.sbt`
- Ce fichier défini essentiellement les **paramètres** du projet :

    ```scala
    name := "my-project"
    
    version := "1.0"
    
    libraryDependencies += "com.chuusai" %% "shapeless" % "2.1.0"
    ```

- Les valeurs des paramètres sont des expressions scala
