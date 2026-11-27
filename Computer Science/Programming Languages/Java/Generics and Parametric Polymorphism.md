From Java 5, the Java developers decided to implement something new : containers that works indistinctly from the object type. From this standpoint, the type becomes like a parameter that can be added when we want to specify on what kind of container we want.

The decision on making this type of objects comes from a repeated problem that lead to a common solution. These solutions are implemented with a process of abstraction, building data structures that are independent on what type we have to handle.

There are several data structures that are implemented in Java libraries : collection, stream, functions, products, consumers and predicates.

# Collection and Java Generics
Before Java 5, there was a library called *Java* *Collection* *Framework* that was fundamental for implementing storages and data structures that works with every Java type, but this library comes with a problem : there were several issues in making downcast (for example in getting elements) and then programs didn't work, producing a `ClassCastException`.

These collections can be referred as objects, that are implemented with something more special than other storages created before, and this thing is *type-variable* : in other words interpreting the type of our structures as a variable.

The type-variable can be specified using `<>` and inside these brackets we are going to define what type of collection we are creating. With these special characters, we are creating something that in Java is called *Java* *Generics*, because these objects works with generic objects. Generics can be classes, interfaces and methods and it won't produce any exception for the "erasure" propriety of the compiler `javac`, in fact in Java 5 there weren't any changes inside the JVM.

## Generic classes

Assuming that X and Y are type-variables. These can be used inside any classes that will work with generics.
For the definition of these classes we have to write : `C<X,Y>`
From now on we can create `C<>` objects that contains the desired type. If we want to specify what kind of classes that our class works with, these types are defined inside the `<>`.
For example : 
```Java
//Pair.java
public class Pair<X,Y>{...}

//UsePair.java
Pair<Integer,String> pair = new Pair<Integer,String>();
//Or in alternative
Pair<Integer,String> pair = new Pair<>();
//With this definition javac does a type matching
 ```

Although this can work with general types, there are some restrictions that we have to pay attention on that parameters can be:
- classes : `Object`, `Integer`, ... (or declared by the programmer) 
- defined type-variable : `X` and `Y` (inside classes like `C<X,Y>`)
- generic type already defined : `C<Object,Object>`
- partially defined generic type : `C<Object,X>`

For example we want to create a class of vectors that will work with generic types :
```Java
//Vector.java
public class Vector<X>{

    private final static int INITIAL_SIZE = 10;

    private Object[] elements;  
    private int size;       

    public Vector(){        
        this.elements = new Object[INITIAL_SIZE];
        this.size = 0;
    }

    public void addElement(final X e){
        if (this.size == elements.length){
            this.expand();  
        }
        this.elements[this.size] = e;
        this.size++;
    }

    public X getElementAt(final int position){
        return (X)this.elements[position];
    }

    public int getLength(){
        return this.size;
    }

    private void expand(){  
        final Object[] newElements = new Object[this.elements.length*2];
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

//UseVector.java

public class UseVector{
    public static void main(String[] s){

        final Vector<String> vs = new Vector<String>();
        vs.addElement("Prova");    
        vs.addElement("di");        
        vs.addElement("Vettore");
        final String str = vs.getElementAt(0) + " " +
                 vs.getElementAt(1) + " " +
                 vs.getElementAt(2); 
        System.out.println(str);
        final Vector<Integer> vi=new Vector<Integer>();
        vi.addElement(10); // Autoboxing
        vi.addElement(20);
        vi.addElement(30);
        final int i = vi.getElementAt(0) + // Unboxing
            vi.getElementAt(1) +
            vi.getElementAt(2);
        System.out.println(i);
    }
}
```

Another example is classes that evaluates pairs of objects inside another class : 
```Java
public class Pair<X, Y> {

    private final X first;
    private final Y second;

    public Pair(final X first, final Y second) {
        this.first = first;
        this.second = second;
    }

    public X getFirst() {
        return this.first;
    }

    public Y getSecond() {
        return this.second;
    }

    public String toString() {
        return "<" + this.first + "," + this.second + ">";
    }
}
```

Additionally, when we are creating an object that we already know what type of object is, we can leave `<>` (diamond symbol) completely blank : `C<Integer,String> c = new C<>();`
If we leave the name of the class without the diamond symbol, there will be a warning or an error that identify the name of the class as a *raw type*.

For example with `Pair` class:
```Java
public class List<X> {

    final private X head;
    final private List<X> tail;

    public List(X head, List<X> tail) {
        this.head = head;
        this.tail = tail;
    }
  
    public X getHead() { 
        return this.head;
    }

    public List<X> getTail() { 
        return this.tail;
    }

    public int getLength() { 
        return (this.tail == null) ? 1 : 1 + this.tail.getLength();
    }

    public String toString() {
        return "|" + this.head
               + ((this.tail == null) ? "|" : this.tail.toString());
    }
}

public class UsePair2{
    public static void main(String[] s){

        final Vector<Pair<String,Integer>> v = new Vector<>();
        v.addElement(new Pair<>("Prova",1));        
        v.addElement(new Pair<>("di",2));      
        v.addElement(new Pair<>("Vettore",3));
        final String str = v.getElementAt(0).getFirst() + " " +
                 v.getElementAt(1).getFirst() + " " +
                 v.getElementAt(2).getFirst();
        System.out.println(str);
        System.out.println(v);
        final List<Pair<Integer,Integer>> l =
          new List<>(new Pair<>(1,1),
          new List<>(new Pair<>(2,2),
          new List<>(new Pair<>(3,3),
          null)));
        System.out.println(l);
        // Local variable type inference
        final var v2 = new Vector<Integer>();
        v2.addElement(1);      
        System.out.println(v2);
    }
}
```

There are several advantages in doing generic classes, like doing more safe code, or more comprehensive code; but there are also disadvantages that suggest to not abuse it, like creating a more sophisticated code.
# Generic interface

It's interfaces declared with type-variables : `interface I<X,Y>{...}` 
This type-variables that are declared in the signature of the interface appears also in its methods. When a class implements this type of variables, also it must implement the type-variable. A class that uses this type of implementation is the interface `Iterator` :
```Java
public interface Iterator<E> {

    E next();
    
    boolean hasNext();

}

//Implementation
public class ListIterator<E> implements Iterator<E> {

    private List<E> list; // Lista corrente

    public ListIterator(final List<E> list) {
        this.list = list;
    }

    public E next() {
        final E element = this.list.getHead();// Elemento da tornare
        this.list = this.list.getTail();// Aggiorno la lista
        return element;
    }

    public boolean hasNext() {
        return (this.list != null);
    }

}

//Usage
public class UseIterators {
	public static void main(String[] s) {
		final List<Integer> list = new List<>(1, 
				                	new List<>(2, 
				                	  new List<>(3, null)));

		final Vector<Integer> vector = new Vector<>();
		vector.addElement(10);
		vector.addElement(20);

		// creo 3 iteratori..
		final Iterator<Integer> iterator = new IntRangeIterator(5, 10);
		final Iterator<Integer> iterator2 = new ListIterator<>(list);
		final Iterator<Integer> iterator3 = new VectorIterator<>(vector);

		// ne stampo il contenuto..
		printAll(iterator);
		printAll(iterator2);
		printAll(iterator3);
	}

	// e se volessi una printAll che prende anche Iterator<String>??
	static void printAll(Iterator<Integer> iterator) {
		while (iterator.hasNext()) {
			System.out.println("Elemento : " + iterator.next());
		}
	}
}
```

As we can notice from this implementation, `E` can appear as a return type of the methods and can be written in the declaration of our class.

# Generic methods
It's a method that works independently on the parameter types or return type, with an abstraction process.

The syntax of these methods can be :
```Java
//Definition
<X1,...,Xn> return-type method-name(formal-args){...}
//Call
receiver.<X1...Xn>method-name(args){...}
//Inference call
receiver.method-name(args){...}
```

There are two actual uses of generic methods : 
- Static or non-static inside a non-generic class
- non-static with a generic class

Example with first case : 
```Java
public class UseIterators2{
    public static void main(String[] s){
    	final Iterator<Integer> iterator = new IntRangeIterator(5,10);
    	
    	final List<String> list = new List<String>("a", 
    	    		    new List<String>("b", 
    	    		    new List<String>("c",null)));
    	final Iterator<String> iterator2 = new ListIterator<>(list);
    	
    	final Vector<java.util.Date> vector=new Vector<>(); 
    	vector.addElement(new java.util.Date());
    	vector.addElement(new java.util.Date());
        
        final Iterator<java.util.Date> iterator3 = new VectorIterator<>(vector);

        UseIterators2.<Integer>printAll(iterator);	      
        UseIterators2.<String>printAll(iterator2);	     
        UseIterators2.<java.util.Date>printAll(iterator3);
        
        UseIterators2.printAll(iterator3); 
    }
    // Generic static method with non generic class
    public static <E> void printAll(final Iterator<E> iterator){
    	while (iterator.hasNext()){
    	    System.out.println("Elemento : "+iterator.next());
    	}
    }
}
```

Example with second case : 
```Java
public class Vector<X>{
    
    private final static int INITIAL_SIZE = 10;
    
    private Object[] elements; 	
    private int size;		
    
    public Vector(){		// Inizialmente vuoto
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

	// Generic method with a generic class
    <E> Vector<Pair<X,E>> genVectorPair(E e){
    	Vector<Pair<X,E>> vp = new Vector<>(); 
    	for (int i=0;i<this.size;i++){
    	    vp.addElement(new Pair<>(this.getElementAt(i),e));
    	}
    	return vp;
    }
}
```
