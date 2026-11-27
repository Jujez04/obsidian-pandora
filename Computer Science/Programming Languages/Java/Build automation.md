In general case, building a software system is not "programming" as someone might think, but also includes code quality check, improving performances, tests and more. There are some tools that can do human work in a more efficient way and that are based on *build automation*. These tools can automate every quality process, building a software that maintains software. There are two types of automators : 
* *Imperative* : realised in script code, they are incredibly flexible and configurable but they do a complex adaptation.
* *Declarative* : realised with a configuration file, they are portable and comprehensive, but they are limited in configurations and flexibility.

## "Convention over configuration"
This principle stands that building software can be simplified, remaining with only the necessary code to define a standard. This one can be modified anytime to adapt with the machine, or to improve a development process, or to reduce repetitions, or even improving portability.

# Hybrid automation tools
These systems try to unify declarative systems with imperative ones.
The configuration file is a *script*, implemented with a programming language. This script can provide all the characteristics of other systems to the hybrid automator. 

Two examples are Sbt, that is based on Scala, and Gradle, that is implemented with Kotlin/Groovy. 
They have to implement also *Domain-Specific languages*, in a way that tools will build a language to a specific language to improve flexibility. (?)

## Gradle
It is a modern hybrid automation tool based on Groovy programming language and it supports Java, C/C++, Kotlin,...

First of all there are three concepts to be understood:
* *task* : the most *single* and *atomic* operation that can be done.
* *project* : There's a directory that contains `build.gradle.kts` and/or `settings.gradle.kts`, in other words *build files* that defines a project.
* *plugin* : software that includes tasks 

Helpful tasks : 
* `gradle tasks` --> produce a list of tasks in the CLI
* `gradle tasks --all` --> produce a list of all tasks in the project
* `gradle compileJava` -->compiles Java files

Let's had a closer look to the build files :
`settings.gradle.kts` defines a name of our project, it contains (optional): 
```
rootProject.name = "minimal-build"
```
`build.gradle.kts` contains plugins :
```
plugins { java }
```

With this example, Gradle will compile only Java files in the `src/main/java` directory and produce bytecode in `build/classes/java/main`

For avoiding update problems, there is a set of scripts that will download the correct version of Gradle and it is called *Gradle wrapper*. As a downside, there are new scripts that must be executed : 
```bash
gradlew      #Linux/macOS
gradlew.bat  #Windows
```
These scripts are inside a directory `gradle/wrapper` and are divided in two files `gradle/wrapper/gradle-wrapper.properties` and `gradle/wrapper/gradle-wrapper.jar`. Pay attention with Linux/macOS, for the fact that in a first place `gradlew` couldn't be an executable file, then use `chmod +x gradlew` to enable execution.

Wrapper can be downloaded from here : 
[https://github.com/DanySK/Gradle-Wrapper/archive/refs/heads/master.zip](https://github.com/DanySK/Gradle-Wrapper/archive/refs/heads/master.zip)


