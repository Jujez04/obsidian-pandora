
# type-erasure
The *type*-*erasure* is a modus operandi of the compiler when it encounters *Generics*. Every time that the JVM sees something inside `<>` it will replace it with an `Object` generic reference.
For example :
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
    	// unchecked warning
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
}


//Replaced with a generic reference
public class Vector{
    
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
    
    public Object getElementAt(int position){
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
}
```

For this fact, a type-variable *cannot* be used with this operation :
```Java
new X(), new X[]{..}, new X[10], instanceof X
```

This problem can be compared with a *forced* *downcast*, where we are casting from a more generic object to a more specific one.


# Bounded Polymorphism
Each class `C<X>`, `X` can be declared as a subtype of `Object`, in fact `C<X extends Object>`.
In general, every `X` can be a subtype of every object, declaring a more specific structure that handles only child of that classes. The definition of this type of object is `C<X extends D>` and each the implementation includes methods that uses `D` object and also child classes (`X`).

For example :
```Java
public class LampsRow<L extends SimpleLamp> {
    
    private Vector<L> lamps;
    
    public LampsRow(){
    	this.lamps = new Vector<>(); 
    }
    
    public L getLamp(int position){
    	return this.lamps.getElementAt(position);
    }
    
    public void addLamp(L lamp){
    	this.lamps.addElement(lamp);
    }
    
    public void switchOffAll(){
    	for (int i = 0; i < lamps.getLength(); i++){
    	    this.getLamp(i).switchOff();  //switchOff belongs to SimpleLamp class
    	}
    }
    
    public String toString(){
    	return this.lamps.toString();
    }
}
```
# Java Wildcards (Igarashi & Viroli, 2002)

The **question mark (?)** is known as the **wildcard** in generic programming. It represents an unknown type. The wildcard can be used in a variety of situations such as the type of a parameter, field, or local variable; sometimes as a return type. Unlike arrays, different instantiations of a generic type are not compatible with each other, not even explicitly. This incompatibility may be softened by the wildcard if ? is used as an actual type parameter.

There are three of them actually :
- Lower bounded :  `C<? super T>`
- Upper bounded : `C<? extend T>`
- Unbounded : `C<?>`

## Subtyping and safety
From this perspective, every structure that uses a more specific object can be a subtype of the same structure but uses a more general object, but this is not true for type erasure. For example :
`Vector<Integer>` isn't a subtype of `Vector<Object>` 

```Java
Vector<Integer> vec =new Vector <>() ;
vec.addElement(new Integer (0) ) ;
vec.addElement(new Integer (1) ) ;
vec.addElement(new Integer (2) ) ;
addAString(vec) ;    //Runtime error
```

*Safety* is an important concept that gives us an advice on what to do about subtyping, in fact we mustn't use forced downcast or unchecked. In wildcards case, there isn't:
- *covariance* : where `C<T> <: C<S>` , where `T <: S` 
- *contravariance* : where `C<S> <: C<T>` , where `S <: T` 

Starting from Java 1, arrays had been dealing this problem and this is resolved launching an `ArrayStoreException` :
```Java
Object[] o = new Integer []{1 ,2 ,3}; //Covariance
o[0] = "a";  //ArrayStoreException
```

Covariance allows a type to be **read** from a generic structure but **not modified**. You use `? extends T` to indicate that the type can be `T` or any subtype of `T`. This is useful when you want to ensure that you can only retrieve items from a collection, without the need to add elements.

Contravariance allows a type to be **modified** but not **read safely** from a generic structure. You use `? super T` to indicate that the type can be `T` or any supertype of `T`. This is useful when you need to modify a structure with elements of a certain type or its subtypes.

