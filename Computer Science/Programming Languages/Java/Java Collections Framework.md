The **Java Collection Framework** is a Java library defined inside the package `java.util` and its purpose handle with data structures and related algorithms.
 
Let's take a look of the UML of classes declared inside this library : 
![[Pasted image 20241017204611.png]]

It seems quite confusing, but gives us an idea of what we can talk about and what kind of strategies were adopted for creating this library.
The most important ones are `Collection` that has three subtypes, `List, Set, Queue`, and `Map`.
For a closer look, we are defining interfaces and classes that can be helpful for understanding this diagram : 
- Interfaces : `Collection` , `List`, `Set`, `Iterator`, `Comparable`
- Classes : `ArrayList`, `LinkedList`, `HashSet`, `HashMap`
- Utilities : `Collections`, `Arrays`

Also these classes throw exception when we are working with types that normally they wouldn't work with them, but we can implement some methods that can help us on avoiding these restrictions.

# Iterable and Iterator
`Iterable` and `Iterator` are interfaces that belong respectively on `java.lang` and `java.util` 
 packages and `Iterable` uses `Iterator`. In a simple way, this interfaces implements functionalities that regard on the `foreach` construct, in fact every classes that are implementing this interfaces can be iterated.

For a better description, this is the UML graphic : 
![[Pasted image 20241017210037.png]]

And also a code example : 
```Java
public class Range implements Iterable<Integer> {

	private final int start;
	private final int stop;

	public Range(final int start, final int stop) {
		this.start = start;
		this.stop = stop;
	}

	public java.util.Iterator<Integer> iterator() {
		return new RangeIterator(this.start, this.stop);
	}
}

//Iterator
class RangeIterator implements java.util.Iterator<Integer> {

	private int current;
	private final int stop;

	public RangeIterator(final int start, final int stop) {
		this.current = start;
		this.stop = stop;
	}

	public Integer next() {
		return this.current++;
	}

	public boolean hasNext() {
		return this.current <= this.stop;
	}

	public void remove() {
	}
}

//Test
public class UseRange {
	public static void main(String[] s) {
		for (final int i : new Range(5, 12)) {
			System.out.println(i);
			// 5 6 7 8 9 10 11 12
		}
	}
}
```

As we can see, an iterator implements `hasNext()` and `next()` methods, and this belong to the *Iterator pattern*. The `foreach` construct can be used with `Range` class for implementing any sort of class iterator, independent on what type it's working on.


# Collection , Set and List

## Interface Collection
In the hierarchy of collections, the interface `Collection` is the primal one and represents groups of objects. Each class that implements this interface has to implement two constructors, one with arguments and another without them, the methods of the interface and modify a method that belongs to `Object` class and it's `Object.equals()`, that works on comparison of two objects.

The code of `Collection` is : 
```Java
public interface Collection <E > extends Iterable <E > {

	 // Query Operations
	int size () ; // number of elements
	boolean isEmpty () ; // is the size zero ?
	boolean contains ( Object o); // does it contain an element equal to o?
	Iterator <E> iterator () ; // yields an iterator
	Object [] toArray () ; // convert to array of objects
	<T> T [] toArray (T [] a); // puts in ‘a‘, or create new if too small
	
	// Modification Operations
	boolean add (E e); // adds e
	boolean remove ( Object o); // remove one element that is equal to o
	
	// Bulk Operations
	boolean containsAll ( Collection <?> c) ; //contain all elements in c?
	boolean addAll ( Collection <? extends E > c); // add all elements in c
	boolean removeAll ( Collection <?> c); // remove all elements in c
	boolean retainAll ( Collection <?> c); // keep only elements in c
	void clear () ; // remove all element
	
	// ... and other methods introduced in Java 8
}
```

The fact that `contains()` and `remove()` return an object regards the way that are implemented, in fact when we want to implement them we have to modify `equals()`.

From Java ) we can create *immutable* collections, in other words collection that cannot be modified:
```Java
public class UseFactories {
	public static void main(String[] s) {

		final Set<Integer> set = Set.of(1, 2, 3, 4, 5, 6);
		System.out.println(set);

		final List<String> list = List.of("a", "b", "c", "a");
		System.out.println(list);

		final Set<String> set2 = Set.copyOf(list);
		System.out.println(set2);
	}
}
```
As we can see, it's done with the implementation of `of()` method, calling up the class where it's defined.

![[Set in Java]]

![[List in Java]]

![[Map in Java]]


## Collections and Arrays
`java.util.Arrays` and `java.util.Collections` are utility classes that belong to the `java.util` package. They improves functionalities that involves collections. `Arrays` contains binary search, sort algorithms and copy methods. `Collections` contains more general methods, like sorting, min, max, sublist, replace, reverse, rotate, shuffle...


