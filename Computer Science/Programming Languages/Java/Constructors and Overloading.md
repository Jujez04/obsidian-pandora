Constructors are useful methods that have the possibility to initialise objects with `new` keyword.
They have the same name as the class and there isn't any return value, but they can have parameters. There is a default constructor that has zero argument.

They can be more than one. As methods they can be overloaded and have many different implementations or arguments.

Example :
```Java
import java.time.LocalDate;

class Person {
    static int currentYear = LocalDate.now().getYear(); // Get current year
    String name;
    int birth;
    boolean married;

    // Default constructor
    Person() {
        this.name = "Unknown";
        this.birth = currentYear;
        this.married = false;
    }

    // Constructor with name
    Person(String name) {
        this.name = name;
        this.birth = currentYear;
        this.married = false;
    }

    // Constructor with name and birth year
    Person(String name, int birth) {
        this.name = name;
        this.birth = birth;
        this.married = false;
    }

    // Constructor with all attributes
    Person(String name, int birth, boolean married) {
        this.name = name;
        this.birth = birth;
        this.married = married;
    }
}

class UsePerson { 
	public static void main ( String [] s ) { 
		Person p2 = new Person (" Mario Rossi "); 
		Person p3 = new Person (" Gino Bianchi ", 1979); 
		Person p4 = new Person (" Carlo Verdi ", 1971 , true);
	} 
}
```

Since we are repeating our code (DRY = don't repeat yourself), we can use `this` instead :
```Java
class Person { 
    static int currentYear = LocalDate.now().getYear();
    String name;
    int birth;
    boolean married;

    Person(String name, int birth, boolean married) {
        this.name = name;
        this.birth = birth;
        this.married = married;
    }
    
    Person() {
        this("Unkown", currentYear, false);
    }

    // Constructor with name
    Person(String name) {
        this(name, currentYear, false);
    }

    // Constructor with name and birth year
    Person(String name, int birth) {
        this(name, birth, false);
    }
}
```


# Overloading methods
Methods can have multiple implementations, both static and non-static, but we can declare different ones only doing different parameters.

Example:
```Java
class ExampleOverloading { 
	static int m( double a , int b ) { 
		return 1; 
	} 
	
	static int m( int a , double b ) { 
		return 2; 
	} 
	
	static int m2 ( double a , double b) { 
		return 1; 
	} 
	
	static int m2 ( int a , int b) {
		return 2;
	}
	
	public static void main ( String [] s ) { 
		// System . out. println (""+ m(1 ,1)); AMBIGUOUS ! 
		// System . out. println (""+ m (1.5 ,1.5) ); NO COMPATIBLE ! 
		System . out . println ("" + m2 (1.5 , 1.5) ); // 1 
		System . out . println ("" + m2 (1 , 1) ); // 2 
	}
}

```