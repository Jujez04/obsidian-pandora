Java is a high-level, object-oriented programming language designed to solve real-life problems using the concept of **objects**. When we create an object, we are simulating a real-life entity in a program, including its properties and name. This concept is closely linked with _abstraction_. The programmer must establish a connection between the *problem model* and the *machine model*, manipulating objects that belong to the program. The object-oriented approach goes beyond simply writing lines of code and executing instructions; it creates a way of representing elements with this concept.

Everything about OOP programming can be summarized in these key points:

- **Everything is an object.** You can store data and _make requests_ to objects.
- **A program manipulates objects by sending messages to them.** For example, by calling a method.
- **Each object can be composed of other objects.**
- **Every object has a type,** which is called a _class,_ and objects are considered _instances_ of a class.
- **All objects of a particular type can receive the same messages,** or, in other words, they can call the same methods.

# Classes and Interfaces

The keyword **class** is used to declare a group of objects of the same type. Objects can be created from a declared class, and they will share category, data, and methods. This keyword is useful for mapping elements in a problem one by one.

The requests you can make of an object are defined by its _interface_: ![[Pasted image 20240822205732.png]] _This diagram follows the format of the UML (Unified Modeling Language)._

In this example, we declare a light bulb as an object with the class _Light_, and in its interface, we can call four methods that are useful to us: turning it _on_ or _off_, and making it _brighter_ or _dimmer_.

With the interface, we declare the _implementation_ of a class, specifying the methods that can be called on an object that belongs to a particular class.

The goal when creating an object is to design its class to provide the ideal services to solve our problem. Thinking in this way can improve the cohesiveness of the object. When an object has too much functionality, it can become redundant and even self-defeating, but the programmer has to find a way for defining every service in the space problem.

# Information hiding

In Java, it's useful to distinguish between _class creators_, who create classes for managing problems, and _client programmers_, who use these classes in their programs. The class creator decides what client programmers can use or see and what should be hidden. Hiding implementation details can resolve program bugs without affecting the client programmer's code.

Java uses explicit keywords for this purpose: #public, #private, and #protected to control what can and cannot be accessed by the client programmer.

# Reusing Implementation

Reusing implementations that are already created can be useful for solving problems without writing additional lines of code, but it’s not as easy as many might think. One way Java offers is to create new classes by combining existing ones or implementing new classes based on existing ones. This concept is called _composition_ (or _aggregation_, also referred to as a _has-a_ relationship). ![[Pasted image 20240826210943.png]] 

Composition provides great flexibility, allowing the creation of new objects without affecting other objects used in the client code. It also enables dynamic changes in the behavior of a program without the concern of corrupting existing functionality.

# Inheritance

Classes can be linked by _concept_, representing a more accurate mapping of our problem. We create new classes based on existing ones, modifying and adding features for our purposes. This process is called _inheritance_ and gives our program a _super_ class (or _parent_ class) and a _subclass_ (or _child_ class).

![[Pasted image 20240826211001.png]]

A child class inherits all characteristics from the parent class, building up a hierarchy that expresses different levels of the problem.

For example, consider a base class called _Shape_ with methods like `draw()`, `erase()`, `move()`, `getColor()`, and `setColor()`, which represent how we can manipulate a shape: drawing it, erasing it, moving it from one position to another, and getting or setting its color. ![[Pasted image 20240826211031.png]] Next, we create specific objects derived from this generic shape, such as _Circle_, _Square_, or _Triangle_. To add new methods to our class, we can override the methods of the _Shape_ class and modify them as needed.

There is a debate about overriding methods: Should inherited classes override _only_ base-class methods? The answer is nuanced. On one hand, a new class should be created as a pure substitute for the parent class if you want to inherit specific aspects of that component. This approach can be helpful for handling your problem, as changing the nomenclature of your components can improve code readability. On the other hand, adding new fields or methods in your subclass can be beneficial because you are extending the functionality of your classes. However, you should be cautious about the differences between the subclass and its parent class, as they are not the same class.

![[Pasted image 20240828164829.png]] For example, in a "cooling system" problem, you might need to replace an air conditioner with a heat pump. The heat pump class includes the cooling method shared with the air conditioner and an additional method for heating. This can become confusing because the thermostat may control the cooling system only for lowering the temperature, but the heat pump could heat up the environment, and the method might not be directly controlled or, worse, could become useless.

# Interchangeability with Polymorphism

When dealing with type hierarchies, you often want to treat an object not only as its specific type but also as its base type. Adding new types with new methods can be helpful for handling new situations, but there’s a problem: What happens when we want to treat a new type object as its base type? The compiler cannot determine at compile time which piece of code will be executed, so if we add a new method with a new type, the compilation will differ depending on which part is executed.

![[Pasted image 20240828170334.png]] For example, if we want to improve a bird controller for moving birds, it could be difficult to move them depending on the type. If we want to move a goose, it can fly or walk; but if we want to relocate a penguin, it can walk or swim.

In a non-OOP compiler, there is a process called _early binding_, where the function called is linked with an absolute address during runtime. Object-oriented languages solve this problem with _late binding_. The compiler ensures that the method exists and performs type checking on the arguments and return value, but it doesn’t know which part is going to be executed.

To perform late binding, Java uses a special bit of code in place of the absolute address. This code calculates the address of the method body using information stored in the object, changing behavior based on the result.

In other languages, you have to specify which classes should use polymorphism and which should not. In Java, the default behavior is _dynamic binding_.

Considering this behavior, we can treat a child component as a base class, ignoring all the specific details. Thus, the program becomes _extensible_. This code is _decoupled_ from type-specific information and is, therefore, simpler to write and understand.

We call this process of treating a derived type as though it were its base type _upcasting_: ![[Pasted image 20240828174245.png]]

# The Singly Rooted Hierarchy

Unlike in other languages, such as C++, there is an ultimate base class for all objects created in Java, simply called _Object_. All interfaces that belong to the singly rooted hierarchy share the same fundamental type. All objects created are guaranteed to have certain functionality, are easily created on the heap, and argument passing is greatly simplified.

This concept is primarily used for the implementation of the _garbage collector_ and to preserve type definition in exception handling.

# Containers
Containers in Java are data structures used to store and organise objects. They are part of the Java Collections Framework and include several interfaces and classes to handle collections of objects efficiently.

- **List**: An ordered collection (sequence) that allows duplicates. Examples include `ArrayList`, `LinkedList`, and `Vector`.
- **Set**: A collection that does not allow duplicates. Examples include `HashSet`, `LinkedHashSet`, and `TreeSet`.
- **Map**: A collection of key-value pairs, where each key is unique. Examples include `HashMap`, `TreeMap`, and `LinkedHashMap`.
- **Queue**: A collection designed for holding elements prior to processing, typically following FIFO (First In, First Out). Examples include `LinkedList`, `PriorityQueue`, and `ArrayDeque`.

Containers manage the storage, retrieval, and ordering of objects, providing a structured way to handle data within a program.
# Object creation & lifetime
Object creation and lifetime management in Java involve how objects are instantiated, used, and eventually destroyed.

- **Instantiation**: Objects are created using the `new` keyword, which allocates memory for the object on the heap and calls the constructor to initialize it.
- **Constructors**: Special methods that initialize new objects. They have the same name as the class and can be overloaded to provide different initialization parameters.
- **Garbage Collection**: Java uses automatic garbage collection to manage memory. When an object is no longer referenced, it becomes eligible for garbage collection, freeing up memory.
- **Finalize Method**: An optional method (`finalize()`) that can be overridden to clean up resources before an object is garbage collected. However, its use is discouraged in favor of try-with-resources and other resource management techniques.

Understanding object creation and lifetime helps in managing resources efficiently and avoiding memory leaks.
# Exception handling
Exception handling in Java is a mechanism for managing runtime errors, ensuring that the program can handle unexpected situations without crashing.

- **Try-Catch Block**: Used to catch exceptions that occur during program execution. The `try` block contains the code that might throw an exception, and the `catch` block handles the exception.
- **Throw and Throws**: The `throw` keyword is used to explicitly throw an exception, while the `throws` keyword in a method signature indicates that the method may throw specific exceptions.
- **Finally Block**: A block that executes after the `try-catch` block, regardless of whether an exception was thrown. It’s typically used for resource cleanup.
- **Custom Exceptions**: Developers can create their own exceptions by extending the `Exception` or `RuntimeException` classes.
- **Checked vs. Unchecked Exceptions**: Checked exceptions must be handled or declared in the method signature, while unchecked exceptions (subclasses of `RuntimeException`) do not require explicit handling.

Exception handling ensures that errors are managed gracefully, allowing the program to recover or terminate safely.
# Concurrent programming
Concurrent programming in Java allows multiple threads to execute simultaneously, improving performance and responsiveness in multi-core systems.

- **Threads**: The basic unit of execution in Java, created either by extending the `Thread` class or implementing the `Runnable` interface.
- **Synchronisation**: A mechanism to control access to shared resources by multiple threads. The `synchronized` keyword can be used to prevent race conditions.
- **Executors**: A higher-level framework for managing a pool of threads, providing a way to decouple task submission from the mechanics of how each task is run.
- **Locks**: Objects that provide more extensive locking operations than can be obtained using synchronised methods and blocks. Examples include `ReentrantLock` and `ReadWriteLock`.
- **Concurrent Collections**: Thread-safe collections such as `ConcurrentHashMap`, `CopyOnWriteArrayList`, and `BlockingQueue` that are designed for use in multi-threaded environments.

Concurrent programming enables the development of responsive, scalable applications by effectively managing multiple threads of execution.
# Java and the Internet
Java's platform-independent nature and built-in network libraries make it well-suited for developing Internet applications.

- **Applets**: Small Java programs that run in a web browser, though largely obsolete today in favor of more modern technologies.
- **Servlets**: Java programs that run on a server, handling requests and generating dynamic web content. Servlets are the foundation of Java's server-side web development.
- **JavaServer Pages (JSP)**: A technology for developing dynamic web pages using Java on the server side, allowing the embedding of Java code in HTML.
- **Networking APIs**: Java provides classes like `Socket`, `ServerSocket`, and `URL` to handle communication over TCP/IP networks, making it easier to create networked applications.
- **Web Services**: Java supports the creation of RESTful and SOAP-based web services, enabling integration and communication between different systems over the web.
- **Java Web Start**: A technology that allows the deployment of standalone Java applications over a network, providing automatic updates and security features.

Java's robust network capabilities and platform independence make it a popular choice for web and networked applications.

# Memory and storage
In Java, it is useful to consider these types of storage:
- **Registers**: These are inside the processor and are allocated as needed.
- **The Stack**: This resides in the general random-access memory (RAM) area but has direct support from the processor via its _stack pointer_. Inside this storage there are all java variables.
- **The Heap**: This is a general-purpose pool of memory in the RAM area, where Java objects live. The compiler doesn't know how long storage must remain on the heap, providing great flexibility. The objects are inside this storage.
- **Constant Storage**: Constant values are often placed directly in the program code, which is safe since they cannot change.
- **Non-RAM Storage**: Data outside the program can persist while the program is running. For example, _streamed objects_ (where objects are converted into a stream of bytes) or _persistent objects_ (stored on disk and retained even when a program is not running).