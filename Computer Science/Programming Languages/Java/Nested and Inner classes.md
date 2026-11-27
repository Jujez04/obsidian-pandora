This page includes some contents about advanced mechanisms in handling classes. Nested static classes paragraph speaks about how to nest classes inside other classes, which can be visible only inside the upper class. Inner classes on the other hand has a particular reference when we want to take access in them. Local classes is a class that is local to a method, defined by the scoping rules. Anonymous  classes can implement a very specific object by a simple declaration, without declaring any signature. Enumeration can define anonymous classes without creating new ones. 

The JVM sees these classes with a particular name inside the `/bin` directory. From the right we see that the most visible class is the first one nominated, then it is followed by the seconds nested inside, and so on. They are divided by `$` from one to another.

For example : 
```
Person$PersonComparator
Map$Entry
```
# Nested static classes
We call "*outer*" the most visible class, the nested class it will called "*nested* *static*" class.
```Java
class Outer{

	static class Inner{
	...
	}
}
```

There are several possibilities in implementing something nested into a class : 
* normal nested classes
* classes that are nested inside other nested ones
* interfaces
```Java
class Outer {

	static class Inner {  ... static class Inner2 {...} }
	interface InterInner{ ... } // Automatically static
}
```

The syntax in accessing nested classes is the normal with the `.` operator : `Outer.Inner , Outer.A, Outer.Nested ,...`
Inside `Outer` we can call directly the nested class, without using the mentioned operator.
`private` fields and methods can be visible by both outer and inner classes, sharing all informations. 

```Java
class Outer {
	...// something private
	static class StaticNested {
		... // everything is visible, also private fields
	}
}
...


Outer . StaticNested obj = new Outer . StaticNested (...) ;
```
## Specialization with inner classes
Also we can specialise outer class (in these cases it will be abstract) by using inner classes, implementing functionalities that has a part of the signature in common. For example :
```Java
class Counter{

	private int value;

	public Counter(int initialValue){
		this.value = initialValue;
	}

	public void increment() {
		this.value++;
	}

	public int getValue() {
		return this.value;
	}

	public static class Multi extends Counter{

		public Multi(int intialValue){
			super(intialValue);
		}

		public void multiInctrement(int n){
			for(int i = 0; i < n; i++){
				this.increment();
			}
		}
	}

	public static class Bidirectional extends Counter{

		public Bidirectional(int intialValue){
			super(intialValue);
		}

		public void decrement() {
			this.value--;
		}
	}
}
```

## Creating sub-functionalities
Another usage of this functionality comes with the implementation of specific sub-functionalities of the object, like creating an *iterable* object. This comes with a closure, the cases that we are going to implementing this idea comes while we are *implementing an interface* or *extending a class.*

For example : 
```Java
public class Range implements Iterable<Integer>{
    
    private final int start;
    private final int stop;
    
    public Range(final int start, final int stop){
    	this.start = start;
    	this.stop = stop;
    }
    
    public java.util.Iterator<Integer> iterator(){
    	return new RangeIterator(this.start,this.stop);
    }
    
    private static class RangeIterator implements java.util.Iterator<Integer>{
        
        private int current;
        private final int stop;
        
        public RangeIterator(final int start, final int stop){
        	this.current = start;
        	this.stop = stop;
        }
        
        public Integer next(){
        	return this.current++;
        }
        
        public boolean hasNext(){
        	return this.current <= this.stop;
        }
        
        public void remove(){}
    }
}

```
## Dividing the object into more parts
When we want to divided the object into more parts, we can implement an inner class that represents the entity to be used. An example of this functionality is with the `Map` class, in fact inside this class is implemented `Entry` class, that represents the couple key-value. It's used for example when we want to iterate all the possible value using the key object.

# Inner class

**Inner classes** are non-static nested classes. They require an instance of the outer class to be created. They can access all members (including private members) of the outer class. A non-static nested class can be referred and called by *outer* methods (for example while we are creating it at compile time). This is called "*enclosing instance*". 

An **enclosing instance** refers to an instance of the outer class that is associated with an instance of an inner (non-static nested) class. When you create an instance of a non-static inner class, it requires a reference to an instance of its outer class, which is what we call the enclosing instance.

The inner class can access the outer class's instance variables and methods, even private ones, because it has a reference to the enclosing instance.

```Java
class Outer {
	private int i;
	...
	class InnerClass { 
		...
		Outer.this.i++;
	}
}

Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
```

The effect of this object comes when we are calling it, with the syntax `Outer.this`. With this signature we are calling the reference of `InnerClass` object.

For example :
```Java
public class Outer {
	
	private int i;
	
	public Outer(int i){
		this.i=i;
	}
	
	public Inner createInner(){
		return new Inner();
		// alternitevely: return this.new Inner();
	}
	
	public class Inner {
		
		private int j = 0;
		
		public void update(){
			this.j = this.j + Outer.this.i; //enclosing instance
		}
		
		public int getValue(){
			return this.j;
		}
	}
}
```

The correct syntax for declaring an inner class is `<obj-outer>.new <inner-class>(<args>)` , where the first part can be replaced with `this`. It is accessible by using `<outer-class>.this`.
We can use inner class when we want that the outer class can use inside the inner class.

For example : 
```Java
public class Range2 implements Iterable<Integer> {

	private final int start;
	private final int stop;

	public Range2(final int start, final int stop) {
		this.start = start;
		this.stop = stop;
	}

	public java.util.Iterator<Integer> iterator() {
		return this.new Iterator();
	}

	private class Iterator implements java.util.Iterator<Integer> {
		
		private int current;

		public Iterator() {
			this.current = Range2.this.start; // this.current = start
		}

		public Integer next() {
			return this.current++;
		}

		public boolean hasNext() {
			return this.current <= Range2.this.stop;
		}

		public void remove() {}
	}
}
```
# Local class
There are some objects that we want to create inside methods and this is possible with *local* *classes.* Inside `Outer` is possible to nest the definition of another class called `LocalClass` (without saying that is `static`). 
```Java
class Outer {
...
	void m ( final int x ){
		final String s =..;
		class LocalClass { // non-static
		... // Outer.this, s and x can be used
		}
		LocalClass c = new LocalClass (...) ;
	}
}

```

The effect is the same of Inner class, in other words it will have an *enclosing* *instance*, but with the effect that is local to a method and it's not visible outside.
Additionally, the local class will see all variable that are "effectively" `final`.

For example :
```Java
public class Range3 implements Iterable<Integer>{
    
    private final int start;
    private final int stop;
    
    public Range3(final int start, final int stop){
    	this.start = start;
    	this.stop = stop;
    }
    
    public java.util.Iterator<Integer> iterator(){
    	class Iterator implements java.util.Iterator<Integer>{
            
            private int current;
            
            public Iterator(){  // Local class
            	this.current = Range3.this.start;
            }
            
            public Integer next(){
            	return this.current++;
            }
            
            public boolean hasNext(){
            	return this.current <= Range3.this.stop;
            }
            
            public void remove(){}
        }
    	return new Iterator();
    }
}

```

They can be helpful when we want to bound a class only inside a method, but in practice it's more useful defining anonymous classes.
# Anonymous class

Anonymous classes allow you to create an instance of a class with a specific behavior inline, without a separate class definition. Then there aren't any constructors, but there is an *enclosing* *instance* of the object created. About variables is the same as for local classes, in fact they will see also variables that are effectively `final`.

```Java
class C {
	...
	Object m( final int x ) {
		return new Object () {
			public String toString () { 
				return " value "+ x ; 
			}
		}
	}
}
```

Anonymous classes are useful for creating objects that are referred for only few methods and when we want to create something very specific.

An example is with the `Iterator` interface :
```Java
public class Range4 implements Iterable<Integer>{
    
    private final int start;
    private final int stop;
    
    public Range4(final int start, final int stop){
    	this.start = start;
    	this.stop = stop;
    }
    
    public java.util.Iterator<Integer> iterator(){
    	return new java.util.Iterator<Integer>(){
            // No constructors
            private int current = start; // Range4.this.start
            
            public Integer next(){
            	return this.current++;
            }
            public boolean hasNext(){
            	return this.current <= stop; // Range4.this.stop
            }
            public void remove(){}
        }; 
    }
}
```

Another one is with `Comparator` interface:
```Java
import java.util.*;

public class UseSort {

	public static void main(String[] args) {
		final List<Integer> list = Arrays.asList(10,40,7,57,13,19,21,35);
		System.out.println(list);
		Collections.sort(list,new Comparator<Integer>(){  //Starting from here
			public int compare(Integer a,Integer b){
				return a-b;  //increasing order
			}
		});  //End
		System.out.println(list);

		Collections.sort(list,new Comparator<Integer>(){  
			public int compare(Integer a,Integer b){
				return b-a;  //decreasing order
			}
		});
		System.out.println(list);
	}
}

```
## Enumeration

As a concept in programming, *enumeration* is used when we want to define a set of constants or things that aren't going to change in the future. In OOP languages we want to define not only constant values, but also objects. With the concept of anonymous classes, in Java is possible to declare an enumerated type even with methods and fields.

Declaring `final` variables might be a solution, but they are considered "*low level practice*". Then we could thing creating some classes with *polymorphism*, but it's verbose if we have to create 10000 objects. Then a solution can be with anonymous classes and in fact underneath there is a similar implementation.

Without enumerations the solution could start from this :
```Java
public class Region {
	
	public static final Region MARCHE = new Region(0,"Marche");
	public static final Region VENETO = new Region(1,"Veneto");
	public static final Region LOMBARDIA = new Region(2,"Lombardia");
	
	public static final Region[] VALUES = 
			new Region[] {MARCHE, VENETO, LOMBARDIA};
	
	private final int id;
	private final String name;
	
	private Region(int id, String name) {
		this.id = id;
		this.name = name;
	}

	public int getId() {
		return this.id;
	}

	public String getName() {
		return this.name;
	}

	public String toString() {
		return "Region [id=" + id + ", name=" + name + "]";
	}
}
```

With implicit inheritance, every `enum` extends `java.lang.Enum`, so they cannot extend any other class. Although they can be considered as anonymous classes, they can have constructors, fields and methods by their own. 

There are some methods that inherited from `java.lang.Enum`, like :
- `values()` : returns an array of all `enum` constants.
- `valueOf(String enumName)` : converts a string to the corresponding `enum` constant 
- `ordinal()`: returns the index (zero-based) of the `enum` constant.


```Java
public enum Region {
	ABRUZZO, BASILICATA, CALABRIA, CAMPANIA, EMILIA_ROMAGNA, 
	FRIULI_VENEZIA_GIULIA, LAZIO, LIGURIA, LOMBARDIA, MARCHE, 
	MOLISE, PIEMONTE, PUGLIA, SARDEGNA, SICILIA, TOSCANA,
	TRENTINO_ALTO_ADIGE, UMBRIA, VALLE_D_AOSTA,	VENETO;
}

public class UseRegion {
	public static void main(String[] args) {
		final ArrayList<Region> list = new ArrayList<>();
		list.add(Region.LOMBARDIA);
		list.add(SARDEGNA);
		list.add(Region.valueOf("SICILIA"));
		list.add(Region.values()[10]);
		
		for (final Region r: list){
			System.out.println("toString "+r); 
			System.out.println("ordinale "+r.ordinal()); // 8, 13, 14, 10
			System.out.println("nome "+r.name()); // LOMBARDIA,...,MOLISE
			System.out.println("---");
		}
		
		for (final Region r: Region.values()){
			System.out.print(r+" "); // Prints region
		}

	}
}

```


Whether we want to create our own methods, we can create an entire class to say what it's useful for our purpose. 

For example :
```Java
public enum Region {
	ABRUZZO(CENTRO,"Abruzzo"), 
	BASILICATA(SUD,"Basilicata"), 
	CALABRIA(SUD,"Calabria"), 
	CAMPANIA(SUD,"Campania"), 
	EMILIA_ROMAGNA(NORD, "Emilia Romagna"), 
	FRIULI_VENEZIA_GIULIA(NORD, "Friuli Venezia Giula"), 
	LAZIO(CENTRO, "Lazio"), 
	LIGURIA(NORD, "Liguria"), 
	LOMBARDIA(NORD, "Lombardia"), 
	MARCHE(CENTRO, "Marche"),
	MOLISE(SUD, "Molise"), 
	PIEMONTE(NORD, "Piemonte"), 
	PUGLIA(SUD, "Puglia"), 
	SARDEGNA(SUD, "Sardegna"), 
	SICILIA(SUD, "Sicilia"), 
	TOSCANA(CENTRO, "Toscana"),
	TRENTINO_ALTO_ADIGE(NORD, "Trentino Alto Adige"), 
	UMBRIA(CENTRO, "Umbria"), 
	VALLE_D_AOSTA(NORD,"Valle D'Aosta"),
	VENETO(NORD,"Veneto");
	
	private final Zona z;
	private final String actualName;
	
	private Region(final Zona z, final String actualName){
		this.z = z;
		this.actualName = actualName;
	}
	
	public Zone getZone(){
		return this.z;
	}
	
	public String toString(){
		return this.actualName;
	}
}
```

In addition, there is a way when we want some constants that are local to something. Actually there are *nested* `enum` that can do this type of things.