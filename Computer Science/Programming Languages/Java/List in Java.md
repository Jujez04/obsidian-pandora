`List` is an interface that represents a collection storing objects like a list. As we can see in the UML map, lists are implemented in different ways, one that uses an array as structure and the other that uses linked list. One aspect in common of these two objects is that are sequences, where each elements has a position and can be accessed in different ways, therefore we are involving arrays and linked list. Arrays can be accessed in $O(1)$ but aren't quite flexible. On the other hand, linked list are a flexible structure but quite tough in access elements.
![[Pasted image 20241026204348.png]]

## ArrayList
This approach is based on an array that is more larger than the one is declared within the constructor. Reading and writing comes with a cost of $O(1)$ due to the array concept. Adding elements can be quite simple, but in the worst case the cost is $O(n)$ for copying the same array in a larger one. There are also methods that modify the size of arrays : 
```Java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess , Cloneable , java.io.Serializable {
	public ArrayList ( int initialCapacity ) {...}
	
	public ArrayList () {...}
	
	//Uses c as elements for the array
	public ArrayList ( Collection <? extends E > c) {...}
	
	// Reduce the size of the array
	public void trimToSize () {...}
	
	// Increase the size of the array
	public void ensureCapacity ( int minCapacity ) {...}

}
```

## LinkedList
This class implements a linked list, but has some functionalities that includes a queue and deque implementations. Reading and writing elements has linear cost. 
```Java
public interface Queue <E > extends Collection <E > {
	boolean offer(E e);  //Insert if there is space
	E poll() ; // withdraw the first element
	E element() ; // read the first element, exception if there isn't one
	E peek() ; // read the first element, null if there isn't one
}

public interface Deque<E> extends Queue<E> {
	void addFirst(E e) ;
	void addLast(E e);
	boolean offerFirst( E e );
	boolean offerLast(E e) ;
	E removeFirst() ;
	E removeLast() ;
	E pollFirst() ;
	E pollLast() ;
	E getFirst() ;
	E getLast() ;
	E peekFirst() ;
	E peekLast() ;
	boolean removeFirstOccurrence( Object o);
	boolean removeLastOccurrence( Object o);
	void push( E e ); // identical to addFirst ()
	E pop() ; // identical to removeFirst ()
}
```