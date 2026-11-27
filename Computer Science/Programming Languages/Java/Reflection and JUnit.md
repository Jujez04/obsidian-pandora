Reflection in Java is a powerful feature that allows a program to inspect or modify the runtime behavior of applications. It’s part of the `java.lang.reflect` package and is commonly used to analyse or manipulate classes, fields, methods, and constructors during runtime, which is usually not possible in a statically-typed language like Java.

Using reflection, you can obtain information about a class's structure, such as its fields, methods, constructors, and modifiers (like `public`, `private`, etc.). This is done with the `Class` object in Java, which you can obtain using :
```Java
Class<?> clazz = myObject.getClass();
//in alternative
Class<?> clazz = Class.forName("com.example.MyClass");
```

We already talk about how the JVM sees inner classes inside the `/bin` directory, but with the class object we can imagine that every object in the virtual table is pointed by the corresponding `Class<?>` object.

There is a functionality that is linked with how the JVM reads `.class` files with `javap` and it's called `verbose` mode : 
```bash
javap -v Counter.class
#verbose mode
```
With this command we will visualize the *classfile* :
```Java
Classfile .../ Counter . class
	Last modified Sep 20 , 2018; size 361 bytes
	MD5 checksum 2 c941be1d3cfd60de02143767ce146cc
	Compiled from "Counter.java"
class Counter
	SourceFile : " Counter . java "
	minor version : 0
	major version : 51
	flags : ACC_SUPER
Constant pool :
	#1 = Methodref                #4.#16  //java/lang/Object."<init>":()V
	#2 = Fieldref                 #3.#17 // Counter.countValue :I
	#3 = Class                    #18 // Counter
	#4 = Class                    #19 // java / lang / Object
	#5 = Utf8                     countValue
	#6 = Utf8                     I
	#7 = Utf8                     <init>
	#8 = Utf8                     ()V
	#9 = Utf8                     Code
	#10 = Utf8                    LineNumberTable
	#11 = Utf8                    getValue
	#12 = Utf8                    ()I
	#13 = Utf8                    increment
	#14 = Utf8                    SourceFile
	#15 = Utf8                    Counter.java
	#16 = NameAndType             #7:#8 // " <init >":() V
	#17 = NameAndType             #5:#6 // countValue :I
	#18 = Utf8                    Counter
	#19 = Utf8                    java/lang/Object

public Counter() {
    flags: ACC_PUBLIC
    Code:
        stack = 2, locals = 1, args_size = 1
    0: aload_0
    1: invokespecial #1 // Method java/lang/Object."<init>":()V
    4: aload_0
    5: iconst_0
    6: putfield #2 // Field countValue:I
    9: return
LineNumberTable:
    line 7: 0
    line 8: 4
    line 9: 9

public int getValue() {
    flags: ACC_PUBLIC
    Code:
        stack = 1, locals = 1, args_size = 1
    0: aload_0
    1: getfield #2 // Field countValue:I
    4: ireturn
LineNumberTable:
    line 13: 0

public void increment() {
    flags: ACC_PUBLIC
    Code:
        stack = 3, locals = 1, args_size = 1
    0: aload_0
    1: dup
    2: getfield #2 // Field countValue:I
    5: iconst_1
    6: iadd
    7: putfield #2 // Field countValue:I
    10: return
LineNumberTable:
    line 18: 0
    line 19: 10
}
```
# How the JVM works

The JVM loads classes in memory for the fact that it's a *metaprogram*: *metaprogramming* is a computer programming technique in which computer programs have the ability to treat other programs as their data (Wikipedia).

Java’s _Reflection API_ integrates with the JVM’s class-loading system. The JVM uses the `java.lang.ClassLoader` to load classes based on a pattern known as "_by need_," meaning classes are loaded only when first used. This on-demand loading facilitates dynamic features like _hot-swapping_ (modifying a class at runtime), adding classes during execution, and inspecting runtime classes.

The JVM will search files in the file system, including the classpath, JAR files and files that comes from internet. When files are located by the JVM, it will prepare a data structure in the virtual table, where methods and fields are defined.

# initializer and non-static initializer
There are some parts of our code that can be compiled by taking advantage from the `static` keyword. Actually these parts can be compiled before every part of the program. They are sometimes used to initialize some static fields and are called *initializer* : 
```Java
static {
    System.out.println("Static initializer executed...");
}
```
On the other hand there is the *non*-*static* *initializer* and are used for compiling before the constructor, initializing instance fields :
```Java
{ 
	System.out.println("Non-static initializer executed..."); 
}
```

An example of both :
```Java
public class Initializers {
	
	static { // static initializer
		System.out.println("Static initializer executed...");
		SET = Set.of(10,20,30); //static fields
	}
	
	{	// non-static initializer
		System.out.println("Non-static initializer executed...");
		set = Set.of(10,20,30,40);
	}
	
	Initializers(){
		System.out.println("Constructor executed...");
	}
	
	private static final Set<Integer> SET;
	private final Set<Integer> set;
	// use of non-static initializers with anonymous classes
	private final Set<Integer> set2 = new HashSet<>() {{ add(1); add(2); }};

	public static void main(String[] args) {
		System.out.println("Let's create an object..");
		Initializers i = new Initializers();
		System.out.println(i.set2);
	}
}
```

# Reflection API
Reflection API uses the package `java.lang.reflect` for dynamic programming. There are some possibilities in developing new feature of this package, for the fact that actually this package can be optimized in a way that could be useful for our applications. In *reflection* mode we have a more flexible way for compiling a program, like creating that isn't available at runtime, but as a downside it's more slow than the normal one and there are some security problems.

For a better explanation, advantages in using *reflection* mode are :
- Use classes that aren't available at runtime
- `String` as an identificator for classes
- Bypass compiler's tests
- Extensibility in using classes loaded during the execution for proving new application features, adding them in the CLASSPATH
- Inspect libraries and classes with IDE

An example of dynamic execution :
```Java
import java.lang.reflect.*;

public class DynamicExecution {

	private static final String Q_CLASS = "Insert fully-qualified class name: ";
	private static final String Q_METH = "Insert name of method to call: ";
	private static final String L_OK = "Everything was ok! The result is..";
	private static final String E_RET = "Wrong return type";

	public static void main(String[] s) throws Exception {
		while (true) {
			System.out.println(Q_CLASS);
			final String className = System.console().readLine();
			System.out.println(Q_METH);
			final String methName = System.console().readLine();
			final Class<?> cl = Class.forName(className);	
			final Constructor<?> cns = cl.getConstructor(); 
			final Method met = cl.getMethod(methName);      
			if (!met.getReturnType().isAssignableFrom(String.class)) {
				throw new NoSuchMethodException(E_RET);		
			}
			final Object o = cns.newInstance();             
			final String result = (String) met.invoke(o);   
			System.out.println(L_OK);
			System.out.println(result);
			System.out.println();
		}
	}
}

public class DynamicExecution2 {
    public static void main(String[] s) throws Exception {
        // Prompt user to enter class and method names
        final String className = "com.example.MyClass";
        final String methodName = "myMethod";
        
        // Load class and method
        final Class<?> clazz = Class.forName(className);	
        final Constructor<?> constructor = clazz.getConstructor(); 
        final Method method = clazz.getMethod(methodName);      
        
        // Create instance and invoke method
        final Object instance = constructor.newInstance();             
        method.invoke(instance);   
    }
}

```


# Java Annotation Framework

Annotations in Java can be done with `@` followed by the name of what type of annotation we want and belongs to the package `java.lang`. They will give an instruction to the compiler on what type of piece of code is going to do, but naturally they are considered as comments. With an IDE, if the piece of code doesn't correspond the type of the annotation written before, it will highlighted as a compilation error. At runtime, using reflection, it's possible to verify what type of annotation is written. Additionally annotations gives a better readability, flexibility and can improve the behaviour of the IDE in producing code by *refactoring* or by *source action*.

## @Override
The `@Override` annotation is useful for avoiding signature method mistake during overriding. `javac` will refuse every type of code if the overriding isn't done appropriately. 

Example :
```Java
class MyClass extends SuperClass{

	 @Override
	 public void myMethod(){
	 ...
	 }
}
```
## @Suppress warning
`@SuppressWarning` will declare that the next block of code is correct and the IDE mustn't show any warnings. In addition, we can declare what type of warning will be turned off.

Example :
```Java
public class Vector<X>{
    
    private final static int INITIAL_SIZE = 10;
    
    private Object[] elements; 
    private int size;	
    
    public Vector(){	
    	this.elements = new Object[INITIAL_SIZE];
    	this.size = 0;
    }
    
    public void addElement(X e){
    	if (this.size == elements.length){
    	    this.expand();
    	}
    	this.elements[this.size] = e;
    	this.size++;
    }
    
    @SuppressWarnings("unchecked")
    public X getElementAt(int position){
    	return (X)this.elements[position];
    }
    
    public int getLength(){
    	return this.size;
    }
    
    private void expand(){
    	Object[] newElements = new Object[this.elements.length*2];
    	for (int i=0; i < this.elements.length; i++){
    	    newElements[i] = this.elements[i];
    	}
    	this.elements = newElements;
    }
    
    public String toString(){
    	String s="|";
    	for (int i=0; i < size; i++){
    	    s = s + this.elements[i] + "|";
    	}
    	return s;
    }
}

```

## Others
`@Deprecated` : indicates a method that is no more used
`@FunctionalInterface` : verifies that only one method requires an implementation
Other can be viewed in `java.lang.annotations`.
## Custom annotations
It's possible to declare our own annotations, with a syntax that is likely an interface :
```Java
@Documented //  javadoc documentation
@Target(ElementType.METHOD) // Used only with method
@Retention(RetentionPolicy.RUNTIME) // only in run-time
public @interface ToString {   //Defined with @interface
    String customName() default "";   

    String associationSymbol() default "->";

    String separator() default ",";
}

public class PrintObjectsUtilities {

	public static String objectToString(Object o) {
		try {
			String out = "";
			for (final Method m : o.getClass().getMethods()) {
				if (m.isAnnotationPresent(ToString.class) && m.getParameterTypes().length==0){
					final ToString annotation = m.getAnnotation(ToString.class);
					out += annotation.customName().equals("") ? m.getName() : annotation.customName();
					out += annotation.associationSymbol();
					out += m.invoke(o) + annotation.separator() + " ";
				}
			}
			return out;
		} catch (Exception e) {
			return null;
		}
	}
}


public class Product {
	
	private final String name;
	private final int id;
	private final double quantity;
	
	public Product(final String name, final int id, final double quantity) {
		this.name = name;
		this.id = id;
		this.quantity = quantity;
	}

	@ToString( separator = ";" )  //We can specify also how it can be as a string
	public String getName() {
		return name;
	}

	@ToString( separator = ";", associationSymbol = ":") 
	public int getId() {
		return id;
	}
	
	@ToString( separator = ";", customName = "qty") 
	public double getQuantity() {
		return quantity;
	}
}

public class Person {
	
	private final String name;
	private final int id;
	
	public Person(final String name, final int id) {
		this.name = name;
		this.id = id;
	}
	
	@ToString
	public String getName() {
		return name;
	}

	@ToString 
	public int getId() {
		return id;
	}
}

public class UsePrintObjectsUtilities {
	
	public static void main(String[] args) {
		final Person p = new Person("Marco", 100);
		System.out.println(PrintObjectsUtilities.objectToString(p));
		// getName->Marco, getId->100, 
		
		final Product pr = new Product("Pr", 200, 100000);
		System.out.println(PrintObjectsUtilities.objectToString(pr));
		// qty->100000.0; getName->Pr; getId:200; 
	}
}
```

# JUnit
JUnit is a popular unit testing framework for Java, allowing developers to write and run automated tests to ensure code reliability. *Unit testing* is a technique for testing individual units of code, like methods or classes, to make sure they function correctly.
JUnit allows developers to write these tests so they can be automatically executed, making it easier to spot bugs and maintain code quality throughout the development process.

JUnit tests can be run through an IDE (like IntelliJ or Eclipse), from the command line with Maven or Gradle, or in continuous integration (CI) environments
### **Common JUnit Annotations**
- `@Test`: Marks a method as a test case.
- `@BeforeEach`: Runs before each test, often used to set up common test data.
- `@AfterEach`: Runs after each test, commonly used for cleanup.
- `@BeforeAll`: Runs once before all tests in the class, often used to set up expensive or shared resources (must be a `static` method).
- `@AfterAll`: Runs once after all tests in the class (must be a `static` method).
- `@Disabled`: Disables a test method or class, skipping it during test execution.

### **Assertions**
JUnit provides a set of `assert` methods to check for expected results. Some common ones include:
- `assertEquals(expected, actual)`: Checks if two values are equal.
- `assertTrue(condition)`: Checks if a condition is `true`.
- `assertFalse(condition)`: Checks if a condition is `false`.
- `assertNotNull(object)`: Checks if an object is not `null`.
- `assertThrows(expectedException, executable)`: Checks if a certain exception is thrown during execution.

### Examples
```Java
public class TestGraphImplImproved {

    private Graph<String> g;

    @Before     // initalisation code executed prior to each test
    public void initialise() {
        //g = new GraphImpl<>();
        g.addNode("a");
        g.addNode("b");
        g.addNode("c"); 
        g.addNode("d");
        g.addNode("e");
        g.addEdge("a", "b");
        g.addEdge("b", "c");
        g.addEdge("c", "d");
        g.addEdge("d", "e");
        g.addEdge("c", "a");
    }

    @Test
    public void testNodeSetSize() {
        assertEquals(5, g.nodeSet().size());
    }

    @Test
    public void testNodeSetElements() {
        assertThat(g.nodeSet(), hasItems("a", "b", "c", "d", "e"));
    }

    @Test
    public void testLinkedNodes() {
        assertEquals(2, g.linkedNodes("c").size());
        assertThat(g.linkedNodes("c"), hasItems("a", "d"));
    }

    @Test
    public void testGetPath() {
        assertEquals(Arrays.asList("b", "c", "a"), g.getPath("b", "a"));
    }
}
```