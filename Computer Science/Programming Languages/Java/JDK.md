# Definition of the execution components

- **JDK (Java Development Kit)**: The JDK is a complete software development kit that includes the JRE, the compiler (`javac`), and various development tools. It is used by developers to write, compile, and debug Java applications.
- **JRE (Java Runtime Environment)**: The JRE is a subset of the JDK that includes the bare minimum needed for running Java applications. It does not include development tools like the compiler.
- **JVM (Java Virtual Machine)**: The JVM is a part of both the JRE and JDK. It is the engine that executes Java bytecode and is responsible for the platform independence of Java.

# Java platforms
There are some platforms for developing applications in Java and they are called *Java platforms*. They are different for their library set and the runtime environment (JVM).
* *Java ME* (Java-Platform, Micro Edition) : designed for resource-constrained devices (such as mobile phone, embedded systems,...)  and optimised to run in environment with limited memory and CPU functionalities. They dispose a lightweight runtime environment (*CLDC*, Connected Limited Device configurations) along with a set of APIs tools (MIDP)
* *Java SE* (Java-Platform, Standard Edition) : for the development of general-purpose software, server application and desktop applications. It provides a set of libraries and API to support application programming, including GUI functionalities, database access, networking,...
* *Jakarta EE* (Java-Platform, Enterprise Edition): provides great tools designed for developing distributed applications and enterprise-level systems, such as web applications and business system. It supports scalability, integration and service management
* *Java Card* : designed for the development of smart card and device with limited resources.

## Execution or Development Java Platform
Furthermore a Java platform can include or not a SDK (software development kit). Whether it provide these tools, that are included in the *Java Development Kit (JDK)*, otherwise that platform is a *Java Runtime Environment (JRE)*.

## JVM Implementations
There are some *implementations* of the JVM for each Java platform. For example : in Java SE we have some distributions of it with different behaviours, like OpenJDK (HotSpot), Eclipse, OpenJ9...

Each implementation has different *distribution* of the JVM. For example, the OpenJDK implementation there are Temurin (Adoptium), DragonWell (Alibaba), Corretto (Amazon),...
# Source code
 First of all, every Java program ends with the `.java` extension and can be written as plain text, even without an Integrated Development Environment (IDE).

# Bytecode
Once `javac` generates a `.class` file, this file will contain a set of instructions that belongs to the **Bytecode** representation. Bytecode is an intermediate level between the machine language and programming language, which consist of a set of instruction that the JVM can understand and execute.
# Elements of JDK
## javac

Next, we call the compiler, `javac`, which processes the source code.
The compiler does several tasks, like:
* Syntax Checking
* Semantic Analysis
* Production of an intermediate code file, that has a  `.class` extension

## javadoc
`javadoc` is a tool used for generating HTML documentation form Java source code. It extracts comments written in a specific format (JavaDoc comments) and creates a set of web pages that provide some information about classes, methods, fields, ... in the source code.
```bash
javadoc -d doc -sourcepath src -subpackages com.example
```
This command generates documentation in the `doc` directory for all Java files in the `src` directory, including sub-packages of `com.example`.

## jar
`jar` is a tool that is used to package multiple class and related files into a single file. 
```bash
jar cvf myapp.jar -C bin/ .
```
This command creates a JAR file named `myapp.jar` by including all files from the `bin` directory. The `c` flag stands for "create," `v` for "verbose," and `f` indicates the file name.
## javap
`javap` is a disassembler for Java class files. It provides a way to inspect the contents and bytecode of compiled Java classes.
```bash
javap -c HelloWorld
class HelloWorld {
  HelloWorld();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void main(java.lang.String[]);
    Code:
       0: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
       3: ldc           #13                 // String Hello, world! 2550
       5: invokevirtual #15                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
       8: return
}
```
This command disassembles the `MyClass` class and shows the bytecode (with the `-c` option) as well as other information.


# Run a program

## Functionalities
The **JVM** takes the `.class` file and execute it, where several steps occur :
1) *Loading* : the JVM takes the command entered and tries to looking for the corresponding `.class` file and loads the bytecode in memory, allocating only classes as needed.
2) *Linking* :
	1)  *Verification* : the JVM checks the bytecode ensuring that every instruction is valid and safe. This step is essential for checking errors that compromise the integrity of the JRE, such as access violation.
	2) *Preparation* : the JVM allocates static memory for variables defined in the class, initializing these to their default values.
	3) *Resolution* : the JVM resolves references to other classes, fields and methods in the bytecode, ensuring a direct access in memory to them
3) *Initialization* : the JVM calls the corresponding constructor and executes any static variable in the class.
4) *Execution* : the JVM starts executing bytecode instructions in two ways : 
	1) *Interpretation* : every instruction is interpreted line-by-line (straightforward but less efficient)
	2) *Just-In-Time Compilation* : the JVM applies JIT compilation. It analyses the bytecode and compiles frequently executed pieces into machine code. Once JIT occurred, the compiled code is stored for future use. (More expensive but more efficient) 


![[Pasted image 20240921170151.png]]

# JShell (from JDK 9+)

`JShell` is an interactive tool that can change our command line to compile only Java instructions. From the bash we can use the command `jshell` It can be useful for some examples.
### Key Features of JShell

1. **Interactive Shell**: You can type Java code directly and see the results immediately, which is great for quick tests or learning.
    
2. **Snippets**: You can run individual statements, expressions, or even complete methods and classes.
    
3. **Code Completion**: JShell supports code completion, helping you write code faster and with fewer errors.
    
4. **Variable Persistence**: Variables defined in one command can be used in subsequent commands, making it easier to build up functionality incrementally.
    
5. **Imports**: You can import classes and packages as needed, just like in a regular Java program.
    
6. **History**: JShell keeps a history of your commands, allowing you to easily reference and reuse previous code.
### Useful JShell Commands

- `/help`: Lists available commands.
- `/exit`: Exits JShell.
- `/vars`: Shows all defined variables.
- `/methods`: Lists all defined methods.

# Basic compilation 

Key steps for compiling a Java program:
* Write in a `.java` file with a text editor.
* Compile with the command : 
```bash
javac Name.java
```
* If it compiles, the bytecode `Name.class` is created
* Execute with the command :
```bash
java Name
```
* JVM looks for the class `Name` and it executes the main method.
* If the main class has dependencies, it is necessary to compile self-sufficient classes.

# Advanced compilation
It's a good practice to divide source code and bytecode in two different directories : 
* `src` for source code `.java`
* `bin` for compiled classes `.class`
`javac` also offers a command to compile in a destination directory and this is with `-d DESTINATION_DIRECTORY` :
```bash
javac -d bin src/Example.java
```
This type of command can compile more than one source code in one step :
```bash
javac -d bin src/*.java
```
Each `.class` file will be created in a sub-directory that matches the path declared by the package.

There is a functionality of the JVM that works in a way where it doesn't expect the full path, it is called the *Fully-Qualified Class Name (FQCN)*. Also the JVM works with an environment variable called `CLASSPATH` and it tells to the compiler where it can find classes. The `CLASSPATH` variable helps the JVM locate these files by specifying paths to directories, JAR files, or both.

There is a default classpath that contains automatically the JRE, but other classpath can be added with the command `-cp` or `-classpath`, followed by a list of paths that contains our libraries : 
```bash
javac -d bin -cp ../example/bin src/*.java
```
It will use the classes in the directory `bin` of `example`.

For example :
* Linux/MacOS : `javac -d bin -cp "lib1:lib2:lib3" src/*.java`
* Windows : `javac -d bin -cp "lib1;lib2;lib3" src/*.java`
These example tells us that the JVM has to compile everything in the `src` directory, linking them with all classes in the directories `lib1`, `lib2`  and `lib3`
Then we have to insert the command : 
* Linux/MacOS : `java -cp "bin:lib1:lib2:lib3" MyClass`
* Windows : `java -cp "bin;lib1;lib2;lib3" MyClass`

Every source code has to be collocated inside packages in a way that represents our file system. When we have to compile something inside a package we have to specify the full name of the class.

If there are some problem with the compilation of our classes, there are two commands that can be helpful :
* `javac -help`
* `java -help`

Every time we compile programs we are passing arguments located in the string `args`, in other words the argument of the main method.
For example : the command `javac -d bin MyClass.java` passes three arguments --> `-d`, `bin` and `MyClass.java`

