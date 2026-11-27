First of all, there is a description of types of dependence in OOP languages :
* *Association* : when an object "uses" another one, like a `Teacher` class uses an `Student` class and vice versa.
* *Composition* : when an object "has-a" some objects, such as `Car` class has an `Engine` class, a `Chassis` class and a `Driver` class
* *Specialisation* : when an object "is-a" another one, for example a `Dog` class is an `Animal` class

# Association
It represents a general relationship between two objects, where one object uses or interacts with another object. It typically indicates that objects are aware of each other and can communicate. However, each object can exist independently.

There are two types of association :
- **Unidirectional Association**: One object knows about the other, but the reverse is not true.
    - Example : A `Car` knows about its `Driver`, but the `Driver` object does not necessarily know about the `Car`.
- **Bidirectional Association**: Both objects know about each other and interact with each other.
	- Example: A `Customer` can have multiple `Orders`, and each `Order` knows which `Customer` placed it.
# Composition
Composition is a stronger form of aggregation, where the "whole" owns its "parts," and the **parts cannot exist independently** of the "whole." If the "whole" is destroyed, the "parts" are also destroyed.


Composition implies a **strong ownership** relationship between objects. Additionally, it often implies that the lifespan of the "part" is controlled by the "whole."

For example :
![[Pasted image 20241005172235.png]]

The `DomusController` can be implemented with the subclasses that are showed in this diagram.
A possible implementation can be :
```Java
public class DomusController { 
	private Lamp[] lamps;
	private TV[] tvs;
	private AirConditioner[] airs; 
	private Radio[] radios;
	
	public void switchAll ( boolean on ){ 
		for ( Lamp lamp : this . lamps ){ 
			if ( lamp != null ){ 
				if ( on ){ 
					lamp.switchOn() ;  
				} else { 
					lamp.switchOff(); 
				} 
			} 
		} 
		for ( TV tv : this.tvs ){ 
			if ( tv != null ){ 
				if ( on ){ 
					tv.switchOn() ; 
				} else { 
					tv.switchOff () ; 
				} 
			} 
		} 
	} 
}
```

# Java interfaces
In Object-Oriented Programming (OOP) in Java, an **interface** is a reference type used to specify a set of methods that a class must implement. An interface defines **what** methods a class should have, but not *how* those methods are implemented. 

Interfaces primarily contain method declarations. These methods have no body  and are abstract by default. Any class that implements the interface must provide concrete implementations for all the methods declared in the interface (unless the class is abstract).

In this way, when an objects implements an interface, that will be referred also as interface class and not only as that class.

For implementing an interface it must be used the keyword `interface` and the keyword `implements` with the derived class. 

For example : 
```Java
//Interface
public interface Device{ 
	void switchOn();
	void switchOff();
	boolean isSwitchedOn ();
}

//Interface implementations
	//Lamp implementation
public class Lamp implements Device { 
	private boolean switchedOn; 
	public void switchOn(){
		this.switchedOn = true; 
	} 
	
	public void switchOff(){ 
		this.switchedOn = false; 
	} 
	
	public boolean isSwitchedOn() { 
		return this.switchedOn;
	} 

} 

public class TV implements Device { 
	//Same methods
} 

public class Radio implements Device { 
	//Same methods
}
```

Reducing pasted code can be helpful for improving readability of our code, then we can create a class that uses the interface for controlling every class that implements that interface.

For example :
```Java
class DeviceUtilities { 
	public static void switchOnIfCurrentlyOff ( Device device ){ 
		if (!device.isSwitchedOn()){ 
			device.switchOn(); 
		} 
	} 
}

/*This class can handle any Device class without doing copy-and-paste. Without this technique, they have done :
	public static void lampSwitchOnIfCurrentlyOff ( Lamp lamp ){ 
		if (!lamp.isSwitchedOn()){ 
			lamp.switchOn(); 
		} 
	} 
	
	public static void tvSwitchOnIfCurrentlyOff ( Tv tv ){ 
		if (!tv.isSwitchedOn()){ 
			tv.switchOn(); 
		} 
	} 
*/
```

# Liskov Substitute Principle
Starting off with a class is a subtype of an interface every time that implements it, we can define a principle that is helpful for defining this type of object. 
This principle is called *Liskov Substitute Principle*, which it stands that if any object A that is a subtype of B, then A could be used every time the program needs a B object/value.

This theorem define a first definition of *polymorphism*, more specifically an *inclusive* one, called *subtyping*. When an interface is defined, all objects that implements this interface will set like a "contract" with the interface, then these objects will only have the methods declared in the interface.

# Late binding
There is phenomenon called *late* *binding* where if we call a method and its parameters includes an interface, then we can use all the objects that implement the interface. This occurs for the fact that class type is decided *dynamically*.

For example, if we define a `Lamp` object that implements `Device` interface, then if we call
```Java
public static void switchOnIfCurrentlyOff ( Device device ){ 
	if (!device.isSwitchedOn()){ 
		device.switchOn(); 
	} 
} 


//Client code
Lamp lamp = new Lamp();
switchOnIfCurrentlyOff(lamp);
```
The type matching is delayed until the method call. `switchOnIfCurrentlyOff` requires a `Device` (static type) but in execution we have a `Lamp` (runtime type).

In general we can say that :
* *Early* *binding* : with static/final method 
* *Late binding* : otherwise

# Multiple implementations and extension

If a class implements more than one interface, we can say it with a comma for each interface. The class will implements all the method declared inside each interface and also will have all types defined.

For example, if we have a `class C implements I1,I2,I3{ ... }`, `C` class will have methods declared in `I1,I2,I3`  and types `C,I1,I2,I3`

Another example : 
```Java
public interface Device { 
	void switchOn(); 
	void switchOff();
	boolean isSwitchedOn(); 
} 

public interface Luminous { 
	void dim(); 
	void bright(); 
} 

public class Lamp implements Device,Luminous{

	public void switchOn() {
	
	} 
	
	public void switchOff() { 
	} 
	
	public boolean isSwitchedOn() { 
	
	}
	
	public void dim() {
	
	} 
	
	public void bright() { 
	
	} 

}
```

Additionally, an interface can `extends` an interface, defining its methods and including the other ones.

For example : `interface I extends I1,I2,I3{...}`