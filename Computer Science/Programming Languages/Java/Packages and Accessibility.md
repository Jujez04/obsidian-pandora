Classes can be divided in groups, also there is an implementation for defining a hierarchy of which path a class belongs. These groups are called *package* and there is a keyword for them called `package`. Package name has a hierarchy structure, such as `java.lang` (`lang` directory it's inside `java` directory).

Package are defined starting from classes that belongs to the same directory, which is saved inside a directory that belongs to an environment variable called `CLASSPATH`.

Printing `CLASSPATH`:
```Java
public class PrintClassPath {
    public static void main(String[] args) {
        String classpath = System.getProperty("java.class.path");
        System.out.println("CLASSPATH: " + classpath);
    }
}
```

Note that the default value is `.` , that stands for the actual directory.

If a class belongs to a package, it must explicit at the start of the code with the keyword `package` , following which directory the program belongs.

One or more classes are going to used inside a Java program we have to use the `import` keyword, and this can be performed in two ways : saying all package classes one by one or import all the class inside the package with `*`.

Example : 
```Java
import java.util.ArrayList;  //Import ArrayList class
import java.util.*;          //Import all classes inside java.util
```

Each package as an extension `.java` and can be compiled by `javac`. The file that contains all classes is called *compilation* *unit* and if it's not declared, all classes has a level of accessibility called *package access*. They can be used only if the package is imported or from classes that are inside the package, outside they are considered invisible.

# public and private
If we want to define which class can be visible or not, we can use `private` and `public` keywords. These must be written before class/method/field/constructor.

If it's `public`, then it will be visible from other classes, without limitations. With `private` it will be visible only inside the class which it's defined. 

In a compilation unit there is only one `public` class and it must have the same name of `.java`file.

These keywords can implement more effectively *information* *hiding*.

# final keyword

The `final` keyword can be used for declaring constants, or more specifically variables that cannot change their values. It's helpful for resolving the "magic number" problem.

Example :
```Java
public class MagicExample { 
	private static final int SIZE = 100; 
	public static void main(String[] s){ 
		double[] array = new double [ SIZE ]; 
		double sum = 0; 
		for (int i = 0; i < SIZE ; i ++) { 
			array [ i ] = Math . random () ;
			sum = sum + array [ i ]; 
		} 
		System.out.println("Somma" + sum); 
}
```

Its usefulness comes with creating fields of an objects or when we want to create something that is "immutable".

For example :
```Java
public class ImmutablePoint3D { 

		private final double x;
		private final double y; 
		private final double z; 
	
	public ImmutablePoint3D ( final double x , final double y , final double z ) { 
		this .x = x; 
		this .y = y; 
		this .z = z; 
	} 

	public double getSquareModule () { 
		return this .x * this .x + this .y * this . y + this .z * this . z; 
	} 
	
	public double getX () { 
		return this .x; 
	} 
	
	public double getY () { 
		return this .y; 
	} 
	
	public double getZ () { 
		return this .z; 
	}  
	
	public ImmutablePoint3D translate ( final double x , final double y , final double z) {  
		return new ImmutablePoint3D ( this .x + x , this .y + y , this . z + z) ;
	}

```

When we create `final` fields, this implies that these field can be initialized only inside the constructors.

Additionally, when `final` is used with an object, it implies that the reference to that object cannot be changed.