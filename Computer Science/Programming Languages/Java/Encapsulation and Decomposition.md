# Decomposition
*Decomposition* in Java refers to a top-down approach of breaking down complex problems or large programs into smaller, manageable pieces, similar to the concept of "Divide et impera". The strategy consists of reducing dependencies between problems, in a way that in the future, if we want to modify our classes, we can do it without any drawback. Whether a class is divided or not depends on the standpoint we are facing the problem; more classes implies more code to develop but we are creating something more reusable and maintainable. This concept can help on checking independently our classes.

There are at least three level of decomposition:
1) Packages
2) Classes 
3) Methods 

If we want to design a class, we have to check every kind of dependencies it can have and what can imply adding new attributes.

# Encapsulation (or Enclosing)
*Encapsulation* refers to **bundling** data and methods that operate on that data within a single unit, typically a class. The idea is to restrict direct access to some of an object's components and only allow interaction through defined interfaces.

A way to protect something from other parts of our code is with *Information hiding*. Each class should have `public` methods that can be used outside the class, otherwise every method/field should be `private` 

From a client standpoint, creating a class seems like signing a contract with the programmer, and this contract describes what the client should see and what can be hidden.

## Getters and Setters
For the fact that we are creating `private` fields (in certain case cannot be changed due to the `final` keyword), there is a problem on showing values of object's fields. Then we can create `public` methods that can modify the object. These methods are called *getters* and *setters* and they can respectively "get" the field value and return it and "set" the value to certain constants.

For example :
```Java
//Class Lamp
public class Lamp {

	private static final int LEVELS = 10; 
	private static final double DELTA = 0.1; 
	
	private int intensity ; 
	private boolean switchedOn ;
	
	public Lamp () { 
		this.switchedOn = false ; 
		this.intensity = 0; 
	} 
	
	public void switchOn () { 
		this.switchedOn = true ; 
	} 
	
	public void switchOff () {     
		this.switchedOn = false ; 
	} 
	
	public boolean isSwitchedOn () { //Getter
		return this.switchedOn ; 
	}
	
	private void correctIntensity () { 
		if ( this.intensity < 0) { 
			this.intensity = 0;
		} else if ( this . intensity > LEVELS ) { 
			this.intensity = LEVELS ; 
		} 
	} 
	
	public void setIntensity ( final double value ) {   //Setter
		this.intensity = Math.round (( float ) ( value / DELTA )); 
		this.correctIntensity () ; 
	} 
	
	public void dim () { 
		this.intensity--; 
		this.correctIntensity () ; 
	} 
	
	public void brighten () { 
		this.intensity++; 
		this.correctIntensity () ; 
	} 
	
	public double getIntensity () {     //Getter
		return this.intensity * DELTA ; 
	}
```

A convention on the getter and setter methods is whether returns a boolean type or not :
* Boolean getter : `boolean isXYZ()`
* Normal getter : `namevariable getXYZ()`
* Normal setter : `void setXYZ(namevariable xYZ)`