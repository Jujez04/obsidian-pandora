# Set
`Set` is an interface created for representing collections of objects without duplicates. There are some characteristics that can be discovered while reading this interface, in fact its code implementation looks like this :
```Java
public interface Set extends Collection {}
```
As we can see it's quite straightforward, it doesn't implement any other methods and uses all the methods declared in `Collection`. Although this code, there are some rules that has to be followed and understood, like for example creating an `equal()` that returns `false` with two different elements, or `null` can't appear more than one time. All of these rules have a more detailed description in https://docs.oracle.com/javase/7/docs/api/java/util/Set.html
 
# Implementations of set
Let's take a look on how set classes are collocated in the UML graphic :
![[Pasted image 20241018174419.png]]

First of all there are two usable classes without any modifications and they are  `HashSet` and `TreeSet`. They inherit from `AbstractSet` that is an abstract class with implementation of shared methods, and it inherits from `AbstractCollection` that is also an abstract class.

`AbstractSet` is used for some particular cases :
- For immutable set, only restriction is to implement `size()` and `iterator()`
- For mutable set, redefine the `add()`

## HashSet

This implementation uses the Hash table for implementing a set, but there are restriction in override `equals` and `Object.hashCode()` for positioning elements inside the table.

After override these two methods, we can see through the hashing technique used :
- Creating an array that is 25% more huge than the normal one
- Add an element by hashing function : it can be for example `e.hashCode % size`
- If it is occupied by another element, goes into the next one
- If it is almost full, there is a rehashing method that expand our table
- Researching/Writing in $O(1)$

Also it can be useful watching how it is implemented, then there is the `HashSet` code :
```Java
public class HashSet <E >
	extends AbstractSet <E >
	implements Set <E > , Cloneable , java . io . Serializable {
	
	// Empty set of fixed 16 as size
	public HashSet () {...}
	
	// Set with elements of a collection c
	public HashSet ( Collection <? extends E > c) {...}
	
	// Empty set
	public HashSet ( int initialCapacity , float loadFactor ) {...}
	
	// EmptySet , loadFactor = 0.75
	public HashSet ( int initialCapacity ) {...}
	

}

```
The **`loadFactor`** is a measure of how full the hash table is allowed to get before it needs to grow in size. It affects the performance and memory usage of the hash-based collection.
### equals and hashCode

`equals()` belongs to the `Object` methods set and represents an important way of  comparing two object. Inside this class the implementation is straightforward : two objects are the same if they represents the same object in the heap memory. But if I would like to compare fields instead the pure object because it makes more sense than compare the same object, what should I do?

We can override this method and make an implementation that compares the fields of two objects or more naturally parameters of objects.

For this reason, `equals()` is important to be overridden for checking elements inside a set, and then for chain effect, for hashing in general. Also `hashCode()` as to be redefined for making a good hash function, because with this method we are generating the position of our objects.

For example :
```Java
import java.util.Objects;

public class Person {

	final private String name;
	final private int year;
	final private boolean married;

	public Person(String name, int year, boolean married) {
		this.name = name;
		this.year = year;
		this.married = married;
	}

	public boolean isMarried() {
		return this.married;
	}

	public String getName() {
		return this.name;
	}

	public int getYear() {
		return this.year;
	}

	public String toString() {
		return this.name + ":" + this.year + ": marr-" + this.married;
	}

	public int hashCode() {
		return Objects.hash(name, year);
	}

	public boolean equals(Object obj) {
		if (this == obj) {
			return true;
		}
		if (!(obj instanceof Person)) {
			return false;
		}
		Person other = (Person) obj;
		return Objects.equals(name, other.name) && year == other.year;
	}
}

//Test class

import java.util.*;

public class UseHashSetPerson {
	public static void main(String[] s) {
		// HashSet è un dettaglio, lavorare sempre su Set!
		final Set<Person> set = new HashSet<>();

		// Agggiungo 4 elementi
		set.add(new Person("Rossi", 1960, false));
		set.add(new Person("Bianchi", 1980, true));
		set.add(new Person("Verdi", 1972, false));
		set.add(new Person("Neri", 1968, false));
		System.out.println(set);

		// Testo presenza/assenza di 2 elementi
		final Person p1 = new Person("Rossi", 1960, false);
		final Person p2 = new Person("Rossi", 1961, false);
		System.out.println("Check "+p1+" found "+set.contains(p1));
		System.out.println("Check "+p2+" found "+set.contains(p2));

		// Iterazione: nota, fuori ordine rispetto all'inserimento
		for (final Person p : set) {
			System.out.println("Iterating: "+p+" hash = "+p.hashCode());
		}
	}
}
```
## TreeSet
`TreeSet` is a *specialization* of `NavigableSet` and consequentially `SortedSet` that are both interface extensions of `Set` interface and it's based on a red-black tree implementation. It assumes an order among elements and each of them has position, leading to a $O(log(n))$ in terms of research, insert and erasure.

There are some implementations of this collection, but most of them are based on this characteristics :
- Internal classes that implements `Comparable` 
- Or with an extern implementation of the interface `Comparator`

This is a closer look to the code structure :
```Java
//Interfaces
public interface SortedSet <E> extends Set<E> {
	Comparator <? super E> comparator () ;
	SortedSet <E> subSet (E fromElement , E toElement ) ;
	SortedSet <E> headSet (E toElement ); 
	SortedSet <E> tailSet (E fromElement ); 
	E first () ;
	E last () ;
}

public interface NavigableSet <E > extends SortedSet <E > {
	E lower (E e);
	E floor (E e); 
	E ceiling (E e) ; 
	E higher (E e); 
	E pollFirst () ; 
	E pollLast () ;
	NavigableSet <E> descendingSet() ;
	Iterator <E> descendingIterator() ; 
	NavigableSet <E> subSet (E fromElement , boolean fromInclusive ,
							 E toElement , boolean toInclusive );
	NavigableSet <E> headSet ( E toElement , boolean inclusive );
	NavigableSet <E> tailSet ( E fromElement , boolean inclusive ) ;
}

//Class
public class TreeSet <E > extends AbstractSet <E >
			implements NavigableSet <E >, Cloneable , java.io.Serializable {

	// Empty Set
	public TreeSet () {...}
	
	// Empty Set with an external comparator
	public TreeSet ( Comparator<? super E> comparator ) {...}
	
	// Set defined by another collection
	public TreeSet ( Collection<? extends E> c) {...}
	
	// Set given by another set that has an order implementation
	public TreeSet ( SortedSet<E> s) {...}
}

```




### Compare inside a TreeSet
Thus the implementation involves a red-black tree, we can modify some methods that allow us how to storage properly objects. This idea can be done in two different ways, producing an *internal* adjustment or an *external* object that implements `Comparator` interface.

The first case can be done with a class that implements `Comparable` interface and its `compareTo` method. This idea is already implemented with some Java objects by default. For example :
```Java
public interface Comparable<T> {
	/*@return : 
		0 if this == o
		positive if this > o
		negative if this < o
	*/
	public int comparaTo (T o);
}

class Integer implements Comparable<Integer>

//If we want to create a comparable class
public class Person implements Comparable<Person>{
	//...
	@Override
	public int comparaTo(T o) {
		return (this.year != p.year) ?
			this.year - p.year
			: this.name.compareTo(p.name);
	}
}
```

On the other side we can use an external one created by us with the `Comparator` interface :
```Java
public interface Comparator<T> {
	/*
	0 if o1 == o2
	positive if o1 > o2
	negative if o1 < o2
	*/
	int compare(T o1, T o2);
}
```
Therefore we have to implement the `compare` method, then we can use as parameter to land to our data structure, in fact `TreeSet` can be inizialised with a class that implements `Comparator` interface :
```Java
public TreeSet ( Comparator<? super E> comparator ) {...}
```
But why we are using `Comparator<? super E>` ? The wildcard used is for every type that has `E` as a super type, for the fact that we want to compare any other subclasses. This is a generalisation for every child class, without making any other specific code for them. 

Now we have to create our a class that implements Java interface :
```Java
public class PersonComparator implements Comparator < Person > {
	public int compare ( Person o1 , Person o2 ) {
		return o1 . getName () . equals ( o2 . getName () )
		? o1 . getYear () - o2 . getYear ()
		: o1 . getName () . compareTo ( o2 . getName () );
	}
}
```

## Mutable and Immutable set

There are some methods that return what are called *immutable* set. These set cannot be modified, but they can be used for read inside them or as a parameter of a method. Therefore, if we want to create a *mutable* set, we can insert one of them inside the constructor of our data structure that is going to be used.

For example :
```Java
Set<Product> set = new LinkedHashSet<>(Set.copyOf(hashSet));
return set;
```
`copyOf` returns a collection and this one is used as a parameter in a constructor. Then `set` is a variable of a mutable set.
