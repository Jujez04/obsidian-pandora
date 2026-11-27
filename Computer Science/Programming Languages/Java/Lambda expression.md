Introduced since Java 8, *lambda* *expressions* are an approach that Java takes for facing *functional* *programming*. With this version, there are significant changes in the JVM that allows to write methods without a signature. This feature can be helpful in improving readability in our projects.
In addiction there are some modern libraries that uses it for high level programming, e. g. Stream library.

First of all, a *lambda* is an anonymous function that has a local scope, take advantage of variables declared in the same scope. In Java they are considered *objects* and can be passed when there is a `@FunctionalInterface` (the annotation that this objects are declared).

What the JVM does is creation of an anonymous class that implements the functional interface method. There's a specific *opcode* that allows to avoid the creation of another object for optimization.
# Syntax

## Without typing reference and with single-expression-body

```Java
(x1, x2, ... xN) -> <expb>
() -> <expb>
x -> <expb>
```

## Completed with typing or body

```Java
(T1 x1,T2 x2, ... TN xN) -> { <body> }
```


## Examples 

```Java
public class AllLambdas {

	private static int mycompare(final String a, final String b) { 
		return a.compareTo(b);
	}

	public static void main(String[] args) {
		final List<String> list = List.of("a", "bb", "c", "ddd");

		// completed syntax
		Collections.sort(list, (String a, String b) -> { 
			return a.length() - b.length();
		});

		// with type inference
		Collections.sort(list, (a, b) -> {
			return a.length() - b.length();
		});

		// with type inference and single-expression body
		Collections.sort(list, (a, b) -> a.length() - b.length()); 

		// method reference
		Collections.sort(list, (a, b) -> mycompare(a, b));
		Collections.sort(list, AllLambdas::mycompare); // method reference
	}
}
```

# Method reference
```Java
public MyClass{
	private static void function(...) {...}
	{
	//They are the same
		Collection.sort(list, (a,b) -> function(a, b))
		Collection.sort(list, MyClass::function)
	}
}
```

## Syntax

```Java
//Static method
<class>::<static-method>  == (x1,..,xn)-> <class>.<static-method>(x1,..,xn)
//Non-static method
<obj>::<method> == (x1,..,xn)-> <obj>.<method>(x1,..,xn) 
//Instance method
<class>::<instance-method> == (x1,x2,..,xn)-> x1.<instance-method>(x2,..,xn)
//Constructor
<class>::new == (x1,..,xn)-> new <class>(x1,..,xn)
```

## Examples
```Java
public class MethodReferences {

	private static int staticMyCompare ( final String a , final String b) {
		return a. compareTo ( b);
	}
	
	private int instanceMyCompare ( final String a , final String b) {
		return b. compareTo ( a);
	}

	public static void main ( String [] args ) {
		final List < String > list = List . of ("a", "bb", "c", "ddd ");
		final MethodReferences objAL = new MethodReferences () ;
		
		Collections . sort ( list , (x ,y) -> staticMyCompare (x , y));
		// <class >:: < static - method >
		Collections . sort ( list , MethodReferences :: staticMyCompare ) ;
		
		Collections . sort ( list , (x ,y) -> objAL . instanceMyCompare (x , y)) ;
		// <object >:: < instance - method >
		Collections . sort ( list , objAL :: instanceMyCompare ) ;
		
		Collections . sort ( list , (x ,y) -> x. compareTo (y ));
		// <class >:: < instance - method >
		Collections . sort ( list , String :: compareTo ); // same as above
	}
}
```
### Lambda that has access to their scope
```Java
public class ChangeButton extends JFrame{

	private int counter = 0;
	
	public ChangeButton(){
		super("Changing button example");
		final JButton button = new JButton("Val: ");
		button.addActionListener(
			e -> button.setText("Val: " + ChangeButton.this.counter++));
	
		this.setSize(320, 200);
		this.getContentPane().setLayout(new FlowLayout());
		this.getContentPane().add(button);
		this.setVisible(true);
	}
}
```
### Filter example
```Java
public interface Filter<X> {
	// Does element x pass the filter?
	boolean applyFilter(X x);
}

public class FilterUtility {
	
	public static <X> Set<X> filterAll(Collection<X> set, Filter<X> filter){
		final Set<X> newSet = new HashSet<>();
		for (final X x: set){
			if (filter.applyFilter(x)){
				newSet.add(x);
			}
		}
		return newSet;
	}

	public static void main(String[] args) {
		final List<Integer> ls = List.of(10,20,30,40,50,60);
		
		System.out.println(filterAll(ls,x -> x>20)); // [30,40,50,60]
		System.out.println(filterAll(ls,x -> x>20 && x<60)); // [30,40,50]
		System.out.println(filterAll(ls,x -> x%20==0)); // [20,40,60]
	}
}
```

# Default methods inside interfaces
There are some methods that have the signature `default` :
```Java
interface I {
	... 
	default int m(){...}
}
```
With this keyword defines the method as the default method that is going to be implemented without creating a subclass, every time we are using the lambdas.
For example:
```Java
public interface SimpleIterator<X> {
	X getNext();
	
	// a template method: i.e., it calls abstract getNext()
	default List<X> getListOfNext(int n){
		final List<X> l = new LinkedList<>();
		for (int i = 0; i < n; i++){
			l.add(getNext());
		}
		return l;
	}
}

public class UseSimpleIterator {
	public static void main(String[] args){
		final Counter c = new Counter();
		final SimpleIterator<Integer> si = 
				() -> {c.inc(); return c.getValue();};
		
		System.out.println(si.getNext());
		System.out.println(si.getListOfNext(20));
	}
}
```
As we can see, only one method can be defined as `default` method and can be implemented inside an interface. This means methods can be implemented inside interface, even if they are `static`. Old classes that have implemented this interface cannot be corrupted for the fact that they override the method `getListOfNext`. Then classes that haven't implemented the method receives a new functionality.
In another view, the keyword `default` supports in a more efficient wat the [[Template method pattern]], for the fact that we are implementing methods inside interfaces, allowing multiple inheritance.

## Functional Interface annotation
The annotation `FunctionalInterface` ensure that there is only one "abstract" method declared and whether there are more abstract assert they are defined as `default`
```Java
@FunctionalInterface
public interface SimpleIterator2<X> {
	
	X getNext();
	
	default List<X> getListOfNext(int n){ // template method
		final List<X> l = new LinkedList<>();
		for (int i = 0; i < n; i++){
			l.add(getNext());
		}
		return l;
	}
	
}
```

# Lambda expressions in Java API
There are some cases that we want to create interfaces that are specific to certain operations, adding some default methods and representing functional concepts, like the `SimpleIterator` created before. 
There is a library in `java.util` that is called `function`, in which there are some functional interface that are highly generic. Here we have a list of them:
```Java
Consumer<T>: accept: (T) -> void
Function<T,R>: apply:(T)->R
Predicate<T>: test:(T)->boolean
Supplier<T>: get:()->T
UnaryOperator<T>: apply:(T)->T
BiConsumer<T,U>: accept:(T,U)->void
BiFunction<T,U,R>: apply:(T,U)->R
BinaryOperator<T>: apply:(T,T)->T
BiPredicate<T,U>: test:(T,U)->boolean
java.lang.Runnable: run:()->void  (Related to threads)
```

For example, here we have an example of a filter using the `Predicate` interface:
```Java
public class FilterUtility {
	
	public static <X> Set<X> filterAll(Collection<X> set, Filter<X> filter){
		final Set<X> newSet = new HashSet<>();
		for (final X x: set){
			if (filter.applyFilter(x)){
				newSet.add(x);
			}
		}
		return newSet;
	}

	public static void main(String[] args) {
		final List<Integer> ls = List.of(10,20,30,40,50,60);
		// Note that the name of the method in Filter is never mentioned here
		System.out.println(filterAll(ls,x -> x>20)); // [30,40,50,60]
		System.out.println(filterAll(ls,x -> x>20 && x<60)); // [30,40,50]
		System.out.println(filterAll(ls,x -> x%20==0)); // [20,40,60]
	}
}
```

`Runnable` can be helpful if we want to create programmable commands:
```Java
public class RunnableUtility {
	
	private static void iterate(final int howMany, final Runnable r){
		for (int i = 0; i < howMany; i++){
			r.run();
		}
	}
	
	private static void batchExecution(final List<Runnable> list){
		for (final Runnable r: list){
			r.run();
		}
	}

	public static void main(String[] args) {
		iterate(10, ()-> System.out.println("ok"));
		final List<Runnable> list = List.of( 
				()->System.out.println("a"),
				()->System.out.println("b"),
				()->System.out.println("c"),
				()->System.exit(0)
		); // Inferenza su List.of automatica!
		batchExecution(list);
	}
}
```

We can use `Supplier` and its method `get` to check some conditions:
```Java
public class SupplierExample {

	private void check(boolean condition, Supplier<RuntimeException> exception){
		if (!condition){
			throw exception.get();
		}    
	}

	public int access(int[] array, int index){
		check(array != null, NullPointerException::new); //method/constructor reference
		check(index >=0 && index < array.length, 
				() -> new IndexOutOfBoundsException("Error at position "+index));
		return array[index];
	}

}
```

In conclusion, lambdas can be helpful when we're handling functionalities that has high usage. Passing functions also improves readability an gives more abstraction.
Also there are some API that take advantage of lambdas, like `Threads` and parallel programming classes, and even patterns, like `Command`, `Strategy` and `Observer`.

## Examples where classes uses lambdas

`Iterable` and `Iterator` interfaces are functional:
```Java
public class UseIterators {

	// Uso ancor più compatto del for-each, e sue varianti..
	public static void main(String[] args) {
		final List<Integer> list = List.of(10,20,30,40,50,60,70);
		list.forEach(x -> System.out.print(x+" "));
		System.out.println();
		
		final Collection<Integer> coll = new HashSet<>();
		list.forEach(x -> coll.add(x)); // list.forEach(coll::add);
		System.out.println(coll);
		
		final Iterator<Integer> it = list.iterator();
		System.out.println(it.next()+" "+it.next());
		it.forEachRemaining( x -> System.out.print(x+" "));
		System.out.println();
		
		final Iterable<Integer> iterable = () -> list.iterator();
		for (final int i: iterable){ 
			System.out.print(i+" ");
		}
		for (final int i: (Iterable<Integer>)()->list.iterator()){
			System.out.print(i+" ");
		}
		System.out.println();
	
	}
}
```

`java.util.Map` has `default` methods: 
```Java
public class UseMap {

	public static void main(String[] args) {
		final Map<Integer,String> map = new HashMap<>();
		map.put(10, "a");
		map.put(20, "bb");
		map.put(30, "ccc");
		
		map.forEach((k, v) -> System.out.println(k+" "+v));
		
		map.replaceAll((k, v) -> v + k); // nuovi valori
		System.out.println(map);
		// {20=bb20, 10=a10, 30=ccc30}
		
		map.merge(5, ".", String::concat);
		map.merge(10, ".", String::concat);
		System.out.println(map);
		// {20=bb20, 5=., 10=a10., 30=ccc30}
		
		System.out.println(map.getOrDefault(5, "no")); // "."
		System.out.println(map.getOrDefault(6, "no")); // "no"
	}
}
```

# Optional class
`Optional` class tries to resolve the `NullPointerException`: there are some classes that initialize their fields with `null` value. Every time that we are going to check these fields we can encounter a `NullPointerException`.

Each field that is declared with `Optional<T>` waits optionally a `T` object, where `Optional` class is a wrapper of `T` class. `Optional` can interpreted as a collection of 0 or 1 values representing two status of an object.
There are some methods that can help our code, but there are some disadvantages, like:
* `ifPresent` and `orElse` bypass `null` problem
* `get` throws `NoSuchElementException` if the object isn't set
* `Optional` isn't serializable
* There are mapping function like `map` and `flatMap` that modify values in a way that returns the `Optional` of the result of the mapper function. Furthermore, `map` wrapper the result of the mapper function in a `Optional` object, `flatMap` checks if it's already an `Optional` and then return the result.

Here there are some examples of `Optional` methods:
```Java
public class UseOptional {

	public static void main(String[] args) {
		
		final String s = Math.random() > 0.5 ? "high value" : null;
		//Optional<String> opt = Optional.ofNullable(s);
		Optional<String> opt = s == null ? Optional.empty() : Optional.of(s);
		
		System.out.println("present: "+opt.isPresent());
		System.out.println("orElse: "+opt.orElse("it is actually null"));
		try{
			System.out.println("get: "+opt.get());
		} catch (Exception e){
			System.out.println("get failed.."+e.getClass());
		}
		
		System.out.print("ifPresent..");
		opt.ifPresent(System.out::println);
	}
}

public class UseOptional2 {

	public static void main(String[] args) throws Exception {

		final Optional<Integer> opt = Optional.of(10); 
		final Optional<Integer> opt2 = Optional.empty();
		System.out.println(opt.isPresent());
		System.out.println(opt2.isPresent());
		
		if (opt.isPresent()){
			System.out.println(opt.get()); // 10
		} else {
			System.exit(0);
		}
		opt.ifPresent(System.out::println); // 10
		opt2.ifPresent(System.out::println); // nothing
		
		opt.map(x -> x+1).ifPresent(System.out::println); // 11
		opt.map(x -> x+"1").ifPresent(System.out::println); // "101"
		
		// Your first monad example!
		Optional<Integer> op3 = opt.flatMap(x -> 
								opt2.flatMap(y ->
								opt2.flatMap(z -> Optional.of(x+y+z))));
		System.out.println(op3);
		
		System.out.println(opt.orElseThrow(()->new Exception())); // 10
		System.out.println(opt2.orElseThrow(()->new Exception())); // exc
	}
}

public class UseOptional3 {

	public static void main(String[] args) {
		
		final List<Integer> list = Arrays.asList(10,20,30,null,null,50,60,null);
		System.out.println(list);
		
		final List<Optional<Integer>> olist = new ArrayList<>();
		list.forEach(i -> olist.add(Optional.ofNullable(i)));
		System.out.println(olist);
		
		final List<Integer> l2 = new ArrayList<>();
		olist.forEach(o -> o.ifPresent(l2::add));
		System.out.println(l2);
		
		
		final List<Optional<Integer>> olist2 = new ArrayList<>();
		olist.forEach(o -> olist2.add(o.filter(i -> i<60)));
		System.out.println(olist2);
		
		final List<Optional<String>> olist3 = new ArrayList<>();
		olist.forEach(o -> olist3.add(o.map(i -> i<40 ? "small" : "big")));
		System.out.println(olist3);
	}

}
```

# Records

Introduced in Java 14, the keyword `record` declares a class with immutable fields.
Syntax:
```Java
record ClassSignature(T1 x1, T2 x2, ...) implements I1, ... Ik{...}
```
- The constructor can be redefined
- Implements `public` getters and setters
- Implements `equals`, `hashCode` and `toString` methods by default
- Doesn't allows inheritance and instance fields
- Allows the addition of methods, static members, ...

Example:
```Java
public class TryRecords {

    // basic record
    record Person(String name, String surname, int year){}

    interface Product{
        int id();
        String name();
    }

    // a record implementing an interface
    record ProductImpl(String name, int id) implements Product {
        public ProductImpl{ // changing primary constructor
            Objects.requireNonNull(name);
        }
        public ProductImpl(int id){ // adding another constructor
            this("", id);
        }
    }

    public static void main(String[] args){
        Person person = new Person("mario", "rossi", 1980);
        System.out.println(person + " " + person.name() + " " + person.surname());
        Product product = new ProductImpl("fruit", 100);
        System.out.println(product);
        System.out.println(new ProductImpl(0));
        // System.out.println(new ProductImpl(null, 101)); // exception
    }
}
```


# Switch expression (Not confusing it with instruction - Java 21)
The `switch` expression is likely the `switch` instruction but has some interesting feature using *lambda syntax*. It can be implemented with `return` keyword:
```Java
public class SwitchExpression {

    public static boolean isAGoodDay(WorkDay workday) {
        return switch (workday) {
            case MONDAY, TUESDAY, WEDNESDAY -> false;
            case THURSDAY, FRIDAY -> true;
        };
    }

    public static void printWelcomeMessage(WorkDay workday){
        System.out.println(switch (workday){ 
            case MONDAY -> "Welcome from weekend!";
            case FRIDAY -> "Have a nice weekend!"; 
            default -> "Nothing to say";
        });
    }

    public static int fibonacci(int i) {
        return switch (i) {
            case 0, 1 -> 1;
            default -> fibonacci(i - 1) + fibonacci(i - 2);
        };
    }

    public static void main(String[] args){
        Function<Integer, Optional<Integer>> sqrt = i -> switch(i){
            case 1 -> Optional.of(1);
            case 4 -> Optional.of(2);
            case 9 -> Optional.of(3);
            default -> Optional.empty();
        };
        System.out.println(sqrt.apply(5));
    }
}
```

## switch pattern and sealed inteface
We can use this syntax for the constructor of our classes:
```Java
public class SwitchPatterns {

    record Person(String name, String surname, int year){}
    
    public static String asString(Object obj) {
        return switch (obj) {
            case null -> "null";
            case String str -> str;
            case Integer i when i>=0 -> "Positive: "+i;
            case Integer i -> "Negative: "+i;
            case Person(var n, var s, var y) -> "(" + n + "," + s + "," + y + ")";
            default -> obj.toString();
        };
    }

    public static void main(String[] args){
        System.out.println(asString(null));
        System.out.println(asString("ciao"));
        System.out.println(asString(1));
        System.out.println(asString(-1));
        System.out.println(asString(new Person("mario", "rossi", 1900)));
        System.out.println(true);
    }
}
``` 
This syntax can provide another implementation of identifying multiple instance without using `instanceof`, allowing [[Factory pattern]] to be more readable : 
```Java
sealed interface Shape permits Circle, Rectangle, Triangle {}

record Circle(double radius) implements Shape {}
record Rectangle(double length, double width) implements Shape {}
record Triangle(double base, double height) implements Shape {}

public class SwitchPatternExample {
    public static double calculateArea(Shape shape) {
        return switch (shape) {
            case Circle c -> Math.PI * c.radius() * c.radius();
            case Rectangle r -> r.length() * r.width();
            case Triangle t -> 0.5 * t.base() * t.height();
            default -> throw new IllegalArgumentException("Unknown shape: " + shape);
        };
    }

    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);
        Shape triangle = new Triangle(3, 7);

        System.out.println("Area of circle: " + calculateArea(circle));
        System.out.println("Area of rectangle: " + calculateArea(rectangle));
        System.out.println("Area of triangle: " + calculateArea(triangle));
    }
}
```

Also we can a feature that is used rarely, called *sealed* *interface*. A **sealed interface** in Java is a feature introduced in **Java 15** (as a preview feature) and officially released in **Java 17**. It allows you to explicitly control which classes or interfaces can implement or extend the sealed interface. This provides a way to restrict the hierarchy of a type, enhancing type safety and ensuring exhaustive handling in constructs like `switch`.