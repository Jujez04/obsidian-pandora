
This concept comes from creating a new way of reusability. First of all, other ways to produce an object that is composed of another object there are, and one of them is *composition*. 

```Java
public class Counter { 

	private int value; 
	
	public Counter(final int initialValue){ 
		this.value = initialValue; 
	} 
	
	public void increment() { 
		this.value++; 
	} 
	
	public int getValue() { 
		return this.value; 
	} 
}
```

For example : if we have a class `Counter` and we want to create a class called `MultiCounter` we have to face some problems that can happen :
* Overloading methods : if we want to change method implementations, we have to pay attention at DRY concept. One way to resolve this problem is calling up the other methods that have the same name and implementation with the keyword `this`. This solution is called *delegation*. For example : 
```Java
public class MultiCounter2 { 

	private final Counter counter; 
	
	public MultiCounter2( final int initialValue ) { 
		this.counter = new Counter( initialValue ); 
	} 
	
	public void increment() {
		this.counter.increment(); 
	} 
	
	public int getValue() { 
		return this.counter.getValue() ; 
	} 
	
	public void multiIncrement( final int n) { 
		for(int i = 0; i < n; i ++) { 
			this.counter.increment(); 
		} 
	} 
}
```
* Fields : another problem is handling with fields of the base method, for the fact in most cases they will be `private`. The solution could be with setters and getters but it will be tough to be managed without modifying field visibility.

# Inheritance as specialization
*Inheritance* is a concept linked with *specialization*, defined in [[Encapsulation and Decomposition]], where we use an existing class to define another one, "inheriting" all fields and methods of that class. In the new class we can modify methods, add new fields/methods without breaking out the DRY law.

We can define an inherited class with the `extends` keyword, and new class, called *derived class*/*child class*/*subclass*, will have all non-`private` methods and fields of the old class, called *base class*/*parent class*/*superclass*.

In UML we build a inherited class with this notation : 
![[Pasted image 20241009221500.png]]

# protected keyword
In addition with the description of [[Interfaces and Dependence]], where are explained all level of accessibility, now we are specifying a new level of information hiding, that is called with the keyword `protected`. A hierarchy of accessibility can be (top --> most accessible, down --> least accessible): 
* `public`
* `protected`
* package access
* `private`

`protected` keyword can be used for specifying fields and methods that are going to be handled in the derived class. It stands that the properties are accessible to the class where it is declared, child class, and all classes inside the same package.

 It's not recommended to use `protected` keyword with fields, instead it's better to use it with getters and setters
 
 ![[Pasted image 20241009223933.png]]
 `protected` fields and methods in UML are specified with  "#".
 
# Overriding methods

If we want to define another implementation of a method that exists in the base class, we are going to create an *overridden* method. *Overriding* can improve readability.

Example : 
```Java
public class LimitCounter extends ExtendibleCounter { 

protected final int limit; 

public LimitCounter(final int limit) { 
	super(0);
	thislimit = limit; 
} 

public boolean isOver() { 
	return this.getValue() == this.limit;
} 

@Override
public void increment() { 
	if (!this.isOver()) { 
		super.increment(); 
	} 
}
```

Notes that every overridden method should have "@Override" comment.

## Upgrade visibility in overriding

When a method is overridden, we can modify visibility in a more "visible" state, such as "from `protected` to `public`". Therefore, we cannot downgrade the visibility of a method/field.
# super keyword
If the constructor of the base class needs a parameter to be passed, we can `super` keyword to call up this constructor in the derived constructor.
In first place, the constructor needs to call up parent constructor with `super`, then `super` will be a reference of the base class in the child class.

Overridden methods can call up the base version with the `super` keyword.

For example : 
```Java
public class LimitCounter extends ExtendibleCounter {

	private final int limit; 
	
	public LimitCounter(final int initialValue,final int limit) { 
		super(initialValue); 
		this.limit = limit; 
	} 
	
	public boolean isOver() { 
		return this.getDistanceToLimit() == 0; 
	} 
	
	public int getDistanceToLimit() { 
		return this.limit - this.value; 
	} 
	
	public void increment() { 
		if(!this.isOver()) { 
			super.increment() ; 
		} 
	} 
}
```

# Virtual method table ( or Vtable )

Each class has one *virtual method table* in memory and each object of that class can access to them. In these tables methods can do pattern matching with parameters, even with *late binding*. This structure is a good implementation for polymorphism and makes understandable how keywords `this` and `super` reference in memory.

For example :
![[Pasted image 20241010201650.png]]
In this example each reference is a way to call objects stored in the heap, and consequentially each object has a Vtable, where matches parameters passes.
# Another use of final keyword

In this section we can move an objection for the use of `super` keyword, and that makes accessibility problem with methods. A solution of this problem is declaring `final` methods that cannot be modified when inherited. A way to think this use is like specifying that is the *ultimate* implementation of that method and cannot have another implementation, in other words, cannot be overridden.


