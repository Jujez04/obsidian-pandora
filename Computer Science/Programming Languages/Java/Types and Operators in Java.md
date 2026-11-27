# Precedence
Java has specific rules that determine the order of operations. Like in maths, multiplication and division come first than addition and subtraction.
For an explicit order of evaluation, we can use parentheses :

```Java
//: operators/Precedence.java
public class Precedence {
	public static void main(String[] args) {
		int x = 1, y = 2, z = 3;
		int a = x + y - 2/2 + z; // (1)
		int b = x + (y - 2)/(2 + z); // (2)
		System.out.println("a = " + a + " b = " + b);
	}
} 
/* Output:
a = 5 b = 1
*///:~
```

# Assignment

For assigning values to our variables we can use the operator = . For primitive types is straightforward:
```Java
int a;
a = 5;
```

As regards objects is quite different, for some reasons we can imagine it like a pointer in C. Object variables are located in the stack, but the reference of the object is placed in the heap. When you assign objects, you are saying that variables is an alias for what the object address placed in the heap. We have to ensure that the assignment works with same class objects :
```Java
//: operators/Assignment.java
// Assignment with objects is a bit tricky.
import static net.mindview.util.Print.*;
class Tank {
	int level;
}
public class Assignment {
	public static void main(String[] args) {
		Tank t1 = new Tank();
		Tank t2 = new Tank();
		t1.level = 9;
		t2.level = 47;
		print("1: t1.level: " + t1.level +
		", t2.level: " + t2.level);
		t1 = t2;   //Objects assignment
		print("2: t1.level: " + t1.level +
		", t2.level: " + t2.level);
		t1.level = 27;
		print("3: t1.level: " + t1.level +
		", t2.level: " + t2.level);
	}
} /* Output:
1: t1.level: 9, t2.level: 47
2: t1.level: 47, t2.level: 47
3: t1.level: 27, t2.level: 27
*///:~
```

When two variables start to point to the same reference, there will be a phenomenon called *aliasing*.

## Aliasing in method calls

*Aliasing* will occur also when we pass an object into a method :
```Java
//: operators/PassObject.java
// Passing objects to methods may not be
// what you’re used to.
import static net.mindview.util.Print.*;
class Letter {
	char c;
}
public class PassObject {
	static void f(Letter y) {
	y.c = ‘z’;
	}
	public static void main(String[] args) {
		Letter x = new Letter();
		x.c = ‘a’;
		print("1: x.c: " + x.c);
		f(x);
		print("2: x.c: " + x.c);
	}
} /* Output:
1: x.c: a
2: x.c: z
*///:~
```

This example shows us that the method `f()` would make a copy of our object, but instead this will change our object outside the main method, because we are passing a copy of the reference into the method.

# Types

A types in Java is a process for classifying values and objects. Each type consist of a *name*, a *set of values* and *operators*.

Java has a "*static typing*", where each expression will be evaluated and there will be a compiling error if types are mismatched.
## Classification
* Primitive types : `boolean`, `byte`, `short`, `int`, `long`, `float`, `double`, `char`
* Array : `TypeName[] array = new TypeName[]`
* Class : `Object, String, ArrayList, JFrame,...`

## Primitive types

Size of each primitive type:
![[Pasted image 20240912170450.png]]

### boolean
* The size of #boolean type is not explicitly specified, instead it is defined by using the states *true* or *false*.
* Unary operators : `!  (not)`
* Binary operators : `& (and), | (or), ^(xor), && (and-c), || (or-c)`
* Numeric comparison operators : `>, <, >=, <=, ==, !=`
* Ternary operator (boolean, type, type) : `?:`

### integer : byte, short, int, long
* Basic : `+, -, *, /, %`
* Bit-by-bit : `& (and), | (or), ^(xor), ~(not)`
* Shift : `>> (dx with sign), << (sx), >>> (dx without sign)`
* Integers are represented in 2's complement.
* There are two decimal representation, like 1203 or 1_203 (the last one more useful for large numbers)
* There is the hexadecimal representation, such as 0xE4
* `int` more efficient than `long`

### float and double
* Basic : `+, -, *, /, %`
* These are represented in IEEE 754 standard but they can lead to precision errors
* Decimal representation (such as 1935.484) or and exponential representation (-1.483E+4)
* The default value is `double` but for setting `float` values we can add an `F` / `f` at the end of the value, such as `193.94f` 

### char
* They can be represented in more than one way
* Single character : `'a', 'b', 'c'`
* ASCII code: `65->A`
* Escape characters : `\n, \\, \a`
* UTF16 characters (16 bit) : `\u6C34`

# Cast and conversions
If we want to change types during compilation we can use a `cast`. *Casting* is a useful process that can perform a type change in operations. For example :
```Java
double den = ((double)10)/3;
short i = (short)100;
```
In some cases the compiler would change variable's types automatically, from the more specific type to the more close general one : `byte->short->int->long->float->double`.
If we want to perform the reverse process we have to pay attention because there will be a lost of information.

