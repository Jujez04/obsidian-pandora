An **object** is the fundamental entity for resolving problems in Java.  Despite of treating everything as an object, the programmer manipulates references of objects. When we are doing an initialization for a reference and then we call up the reference, we are not manipulating the object, but its reference. Therefore we have to use a keyword for the initialization of this object : #new. 
```Java
String s = new String();
```
With this declaration, we are defining an object that belongs to the *String* class.

# Primitive types
These type of variables don't need the use of #new keyword, because they aren't references. An important difference is that primitive variables holds values directly, and it's placed on the stack.

The *wrapper* classes for the primitive types allow us the creation a non-primitive object on the heap to represent that primitive type.
```Java
char c = 'x';
Character ch = new Character(c);
//or
Character ch = new Character('x');
```

In addition, Java SE5 *autoboxing* will automatically convert from a primitive to a wrapper class:
```Java
Character ch = 'x';
```

## High precision numbers
Java includes two classes for performing high-precision arithmetic:
* **BigInteger** supports arbitrary-precision integers.
* **BigDecimal** regards arbitrary-precision fixed-point numbers

## var construct
The `var` keyword is a construct to define local variable. The compiler does a process called **Local variable type difference**, where it matches up variable with its assignment. For example :
```Java
//Primitive types
var i = 5;   // i is an integer
var d = 145e-20;    // d is a double
var b = true;     //b is a boolean

//Class
var o = new Object();
var s = "string";
var q = s;       //q belongs to the String class
```

## Default values for primitive types
![[Pasted image 20240912174615.png]]
# Scope of objects
In Java the *scope* of an object is defined by the placement of curly braces **{ }**
```Java
{
	int x = 12;
	//x is available
	{
		int q = 15;
		//Both x and q are available
		int x = 96; //!Error!, illegal operation 
		//x is already defined
	}
	//Only x is available
	//q is no more available
}
```
Same goes for objects:
```Java
{
	String s = new String("");
}   //End of scope
```
All the objects created with the #new keyword stay in a *garbage* *collector*, which are not being referenced anymore. This concept of destroying objects resolves the problem of "memory leak"

# Class
A way for defining behaviours and attributes of our object is declaring a #class. With the #class keyword, followed by the name of the new type, it's possible to create new types of objects not implemented in Java. For example:
```Java
class NewTypeName{ 
//Class body
}
//...
NewTypeName a = new NewTypeName();
```
## Fields and methods
In a class, there are two types of elements that can put in : 
* **fields** (or *data* *members*) : references or primitive types 
* **methods** (or *member* *functions*) : functions that implement what kind of operation will do an object that belong to a class. It is composed with a *signature* (name of the method) and a *body* (instructions that belongs to the method)

The construct `var` cannot be defined inside a class.

For example : 
```Java
class NewClass{
	int x;
	double y;
	void method(){
		//...
	}
	Object o;
}

//Access to a field of a object
NewClass n = new NewClass();
n.x = 12;
```

In a class, if a member is used, it will initialized with the default value corresponding on the type is used. If the type is a class it will be set as `null`.

If we want to get an access on a certain field/method we can use the *point* operator. For example :
```Java
class A{
	int i;
	void add(int a){
		i = i + a;
	}
	int getValue(){
		return i;
	}
}

//in Main class
A obj = new A();
int v = obj.i;            // v = 0
obj.add(10);
obj.add(20);
int v2 = obj.i;           // v2 = 30
int v3 = obj.getValue();  // v3 = 30
```

Methods in Java determine the messages an object can receive and it depends on the name, the arguments, the return type and the body :
```Java
ReturnType methodName(/*argument list*/){
	//Method body
	ReturnType x;
	return x;  //Not necessary if it is void 
}

//method call
methodName(/*Argument list divided by commas*/)
```
### this keyword
The `this` keyword is a special variable that can be called up when we want to define different object, in fact it contains the reference of the object that is called. For example : 
```Java
class Point3D { 
	double x;
	double y;
	double z;
	void build ( double a , double b , double c){
		 this .x = a; this . y = b; this .z = c;
	}
	double getSquaredModulus () { 
		return this .x * this .x + this .y * this . y + this .z * this . z;
	}
	boolean isEqual ( Point3D q){
		return this .x == q.x && this .y == q .y && this . z == q .z;
	} 
} 

//Main class
Point3D p1 = new Point3D () ;
p1 . build (10.0 ,20.0 ,30.0) ; 
Point3D p2 = new Point3D () ; 
p2 . build (10.0 ,20.0 ,31.0) ; 
double m2 = p1 . getSquaredModulus () ;
boolean b = p1 . isEqual ( p2 ) ;
```

# Package
In Java we can import components from other files using the #import keyword, also if files that we are importing share the same name. This keyword tells the compiler to bring in a *package*, a library of classes. These libraries belongs to the JDK (Java Development Kit), but they can be written by us as external package.

An example using the Java libraries:
```Java
import java.util.ArrayList;
```
If we want to import all libraries that belong to a package, we can use \*
```Java
import java.util.*;
```

Java libraries can be explored on this site :  https://docs.oracle.com/en/java/javase/21/docs/api/ 

Note that a name of a class depends on the package in which it belongs.
# Module (from Java 9)

A `module` in Java is a fundamental part of code organization, that allows to regroup related packages and resources together, promoting encapsulation and better management of dependencies. Also a module defines self sufficient code.

For example (Java modules) : `java.base`, `java.desktop`, `java.sql`, `java.xml`
`java.base` is the foundational module that provides essential classes and interfaces required for Java programming. 

A **module** in Java is defined using the `module` keyword in a `module-info.java` file.
```Java
//module-info.java
module Class{
	exports com.example.myapp.api;    //Expose this package to other modules, accessible
	requires com.example.utilities;    //This module requires another module, dependent
}
```

The introduction of the Java Platform Module System (JPMS) in Java 9 marked a significant change in how Java applications could be structured. It allows developers to create modular Java applications that enhance maintainability and scalability.
# The static keyword

If you need to use a field or method that does not belong to any specific instance of a class, you can use the `static` keyword. So, if an object has never been declared, we can call a #static method/field.
To make a field or method #static, you have to put the keyword before the definition:
```Java
class Test{
	static i = 47;
	static void increment(){
		i++;
	}
}

Test.i++; //i = 48
Test t1 = new Test();
Test t2 = new Test();
//t1.i = t2.i ; they share the same field

class Decrementable{
	static void decrement(){
		Test.i--;
	}
}
```

This keyword is essential for defining the *main* method.

# main method

```Java
//Main.java
public class Main{
	public static void main(String args[]){
		System.out.println("Hello World!");
	}
}
```

There are some packages that are already imported into our program, like **java.lang**. For more documentation, visit http://java.sun.com.

Key features of this program:
* `System` is a class that belongs to the package `java.lang`.
* `out` is a static field (standard output).
* `println` is a method that accepts string.

