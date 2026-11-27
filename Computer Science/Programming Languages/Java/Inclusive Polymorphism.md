
This type of polymorphism consist of a class A that uses a class B has a compatibility when we are creating objects. An object A is compatible with operations that expects an object B, for reasons like : 
- sharing the same contract (method signatures)
- compatible behaviour

When we are creating an object, extends the class `Object` and has some characteristic :
* class of that object
* Virtual method table
* `private` fields of `Object`
In other words, objects that are extended has the same information and something more, and this can implement a more well constructed substitutability.

For example : 
![[Pasted image 20241010212428.png]]
This UML graphic can be traduced in : 
```Java
//Person class
public class Person { 

	private final String name; 
	private final int id; 
	
	public Person ( final String name , final int id ) { 
		super() ; 
		this.name = name; 
		this.id = id;
	} 
	
	public String getName() { 
		return this.name; 
	} 
	
	public int getId() {
		return this.id; 
	} 
	
	public String toString() { 
		return "P [ name =" + this.name + ", id=" + this.id + "]"; 
	} 
}

//Student class
public class Student extends Person { 
	final private int matriculationYear; 
	
	public Student (    final String name , final int id , 
						final int matriculationYear ){ 
		super(name,id); 
		this.matriculationYear = matriculationYear; 
	} 
	
	public int getMatriculationYear() { 
		return matriculationYear; 
	} 
	
	public String toString() { 
		return "S [ getName ()=" + getName () + 
		", getId ()=" + getId () + 
		", matriculationYear =" + matriculationYear + "]"; 
	} 

}


//Teacher class
import java.util.Arrays;

public class Teacher extends Person {

    final private String[] courses;
    
    public Teacher(final String name, final int id, final String[] courses) {
        super(name, id);
        this.courses = courses;
    }
    
    public String[] getCourses() {
	    //We are creating a copy for a better encapsulation and security
        return Arrays.copyOf(courses, courses.length);
    }
    
    public String toString() {
        return "T [getName()=" + getName() +
                ", getId()=" + getId() +
                ", courses=" + Arrays.toString(courses) + "]";
    }
}

//UsePerson class
public class UsePerson {
    public static void main(String[] args) {
    
        final var people = new Person[]{
                new Student("Marco Rossi", 334612, 2013),
                new Student("Gino Bianchi", 352001, 2013),
                new Student("Carlo Verdi", 354100, 2012),
                new Teacher("Mirko Viroli", 34121, new String[]{"OOP", "PPS", "PC"})
        };
        for (final var p : people) {
            System.out.println(p.getName() + ": " + p);
        }
    }
}

```

When we are doing polymorphism with interfaces, things change drastically. Class that extends one or more interface has a contract with them and method implementations don't change the class type. 
In the other case, for example with inheritance, class can extend one and only one class. This constrictions comes from the virtual method table and references in memory, in fact will be complex doing late binding.

In conclusion, inclusive polymorphism acts like a wrapper for classes and extends reusability and functionalities that a class can have. With interfaces, it's more flexible and configurable, allowing a contract among more than one interfaces with a class. With classes, it's single use, but allows great functionalities.

# Object class and instanceof keyword

`Object` is the class root of every class in Java. This class can be imagined as an empty class, but it contains some utility methods, like `getClassName`, `toString`, ... , that can have an implementations or has to be implemented by overriding.

`Object` can be even declared as an array, and can be filled with every object that are declared by the project. For example :
```Java
public class AObject {
    public static void main(String[] s) {
        final Object[] os = new Object[5];
        os[0] = new Object();
        os[1] = "string";
        os[2] = Integer.valueOf(10);
        os[3] = new int[] { 10, 20, 30 };
        os[4] = new java.util.Date();
        printAll(os);
        System.out.println(Arrays.toString(os));
        System.out.println(Arrays.deepToString(os));
    }

    public static void printAll(final Object[] array) {
        for (final Object o : array) {
            System.out.println("Object:" + o.toString());
        }
    }
}
```

As we can notice from this code, there is a problem if we want to define some functionalities that are specific to some classes. There is a duality with *compile type* and *run-time type*; type can be accepted during compile time because maybe there is something that has the same types, but it is not guaranteed that will be the same during run-time. The solution of this problem is the keyword `instanceof`, that accept an object variable in the left side and the name of a class in the right side, returning a boolean answer.

For example :
```Java
public class AObject2 {
	public static void main(String[] s) {
		final Object[] os = new Object[5];
		os[0] = new Object();
		os[1] = Integer.valueOf(10);
		os[2] = Integer.valueOf(20);
		os[3] = new int[] { 10, 20, 30 };
		os[4] = Integer.valueOf(30);
		printAllAndSum(os);
	}
	
	public static void printAllAndSum(final Object[] array) {
		int sum = 0;
		for (final Object o : array) {
			System.out.println("Object:" + o.toString());
			if (o instanceof Integer) { // test a runtime
				final Integer i = (Integer) o; // (down)cast
				sum = sum + i.intValue();
			}
		}
		System.out.println("Sum of Integers: " + sum);
	}
}
```

# Abstract classes

Java implements an alternative way to define a class that has implemented/unimplemented methods and this is the *abstract classes*. They use the keyword `abstract` to define this kind of class and they can be considered an intermediate between interfaces and complete classes. 

An abstract class :
- has to be declared in this way : `abstract class C {...}`
- cannot declare other objects
- can optionally implement some methods, but if we want to specify only the signature, we have to say that is `abstact` : `abstract class sum(int m, int n);`
- can extends interfaces or other classes, both abstract and completed ones.
- can implement interfaces
- can be extended by complete classes if and only if all methods are implemented.

A complete example : 

```Java
//SimpleLamp class
public class SimpleLamp {

	private boolean switchedOn;

	public SimpleLamp() {
		this.switchedOn = false;
	}

	public void switchOn() {
		this.switchedOn = true;
	}

	public void switchOff() {
		this.switchedOn = false;
	}

	public boolean isSwitchedOn() {
		return this.switchedOn;
	}
}

//LimitedLamp class
public abstract class LimitedLamp extends SimpleLamp {

	public LimitedLamp() {
		super();
	}
	
	public final void switchOn() {
		if (!this.isSwitchedOn() && !this.isOver()) {
			super.switchOn();
			this.okSwitch();
		}
	}
	
	protected abstract void okSwitch();
	
	public abstract boolean isOver();

	public String toString() {
		return "Over: " + this.isOver() + 
		       ", switchedOn: "	+ this.isSwitchedOn();
	}
}

//UnlimitedLamp class
public class UnlimitedLamp extends LimitedLamp {

	public UnlimitedLamp() {
		super();
	}
	
	protected void okSwitch() {
	}
	
	public boolean isOver() {
		return false;
	}
}

//ExpirationalTimeLamp class
import java.util.Date;

public class ExpirationTimeLamp extends LimitedLamp {

	private Date firstSwitchDate;
	private long duration;

	public ExpirationTimeLamp(final long duration) {
		super();
		this.duration = duration;
		this.firstSwitchDate = null;
	}

	protected void okSwitch() {
		if (this.firstSwitchDate == null) {
			this.firstSwitchDate = new java.util.Date();
		}
	}

	public boolean isOver() {
		return this.firstSwitchDate != null &&
		   (new Date().getTime() - this.firstSwitchDate.getTime() 
				   >= this.duration);
	}
}

//UseLamp class
public class UseLamps {
	public static void main(String[] s) throws Exception {
		LimitedLamp lamp = new UnlimitedLamp();
		lamp.switchOn();
		System.out.println("ul| " + lamp);
		for (int i = 0; i < 1000; i++) {
			lamp.switchOff();
			lamp.switchOn();
		}
		System.out.println("ul| " + lamp); 

		lamp = new CountdownLamp(5);
		for (int i = 0; i < 4; i++) {
			lamp.switchOn();
			lamp.switchOff();
		}
		System.out.println("cl| " + lamp);
		lamp.switchOn();
		System.out.println("cl| " + lamp);

		lamp = new ExpirationTimeLamp(1000); 
		lamp.switchOn();
		System.out.println("el| " + lamp);
		Thread.sleep(10000);
		System.out.println("el| " + lamp);
	}
}
```

# Wrapper classes
Wrapper classes are classes implemented for primitive type in Java, in fact an interpretation can be like a shell that "wraps" fields that are primitive types.

Principale wrapper classes : 
-  **`byte`** → **`Byte`**
- **`short`** → **`Short`**
- **`int`** → **`Integer`**
- **`long`** → **`Long`**
- **`float`** → **`Float`**
- **`double`** → **`Double`**
- **`char`** → **`Character`**
- **`boolean`** → **`Boolean`**

There are methods like `valueOf` that assigned the argument inserted to the field of corresponding wrapper class.

## autoboxing

This is an automatic process in which the compiler converts primitive type into its corresponding wrapper class. For example : 
```Java
int num = 10;
Integer obj = num;
```
## unboxing

This operation is the opposite of autoboxing : when a wrapper class is assigned with a primitive types, it will be converted into its corresponding primitive type.
For example :
```Java
Integer obj = 10;
int num = obj;
```
# variable arguments

There are methods that can accept a variable number of arguments (`varargs`) of the same type. Before Java 5 the argument are wrapped in one array. There is a symbol that can implement this type of process and it's `...`

For example : 
```Java
public void metodoConVarArgs(int... numeri) {
    for (int num : numeri) {
        System.out.println(num);
    }
}
```

Variable arguments have some restrictions :
- The number of arguments can be from 0 to any number that not exceed in memory storage, in fact this restriction can extend to array restrictions.
- There is only one variable argument parameter, and it is the last one on the argument list.