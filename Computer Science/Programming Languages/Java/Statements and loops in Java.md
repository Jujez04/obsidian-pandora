As an imperative/structured programming language, Java requires a definition for each instruction in the program. 
# Control flow statements
## if

In a `if` statement there is a conditional check that is performed before a certain block of code. If the condition is *true*, then that block of code will be executed, otherwise it won't be executed.
```Java
if (x > 0) {
	System.out.println("Positive"); 
}
```

Also Java has an `else` keyword. It is placed immediately after an `if` statement. If the condition returns false then the *else* block will be executed. We can add other `if` statement with `else if`
```Java
if (x > 0) {
	System.out.println("Positive"); 
}else if(x < 0){
	System.out.println("Negative");
}else{
	System.out.println("Neutral");
}
```

## switch

`switch` statement evaluates what's inside the parenthesis and then it checks what case will be executed. It's useful with the `break` keyword, that jump over next instructions inside the block.
```Java
switch (day) {
    case 1: 
	    System.out.println("Monday"); 
	    break;
    case 2: 
	    System.out.println("Tuesday"); 
	    break;
    default: 
	    System.out.println("Other day");
}

```

# while/ do while

`while` and `do` will execute a block of code if the condition is *true*. The main difference is that `while` checks before the loop block, meanwhile  `do` checks after.
```Java
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++; // Increment i
}

i = 0;
do {
    System.out.println(i);
    i++; // Increment i
} while (i < 10);
```

## for / for each

`for` does certain instructions a certain number of times defined in the parentheses before the loop.
```Java
for(int i = 0; i <= 10; i++){
	System.out.println(i);
}
/*Output : 
0
1
2
3
4
5
6
7
8
9
10
*/
```

`for each` is similar but its implementations only for arrays, because we declare an element as each element of the input array in the condition.
```Java
int[] ar = new int[]{10, 20, 30};
int cont = 0;
for(int x : ar){
	cont = cont + x;
}
System.out.println(cont); //cont = 60
```

### More accurate example of for each
```Java
class Sum {
	public static int sum ( int [] array ) { 
		sum = 0; 
		for (int i = 0; i < array . length ; i ++) { 
			sum = sum + array [i ];
		}
		return sum;
	}
	public static int sum2 (int [] array ) { 
		int sum = 0; 
		for (int elem : array ) {
			sum = sum + elem ;
		} 
		return sum;
	}
	
	public static void main ( String[] s ) {
		int res = sum2(new int [] { 10 , 20 , 30 , 40 });
		System . out . println (" Somma : " + res );
	}
}
```
# static keyword with class
The `static` keyword is useful for defining global variables and pure methods. 

```Java
// Utility.java
class Utility{
	static int zero = 0;
	static int sum(int[] arr){
		int cont = 0;
		for(int x : arr){
			cont = cont + x;
		}
		return cont;
	}
}

// Main.java
class Main{
	static int avarege(int[] array){
		int sumResult = sum(array);
		sumResult = sumResult / arrauy.length;
		return sumResult;
	}
	public static void main(String[] args){
		int[] array = new int[]{10, 20, 30};
		System.out.println(Utility.zero);
		System.out.println(Utility.sum(arr));
		System.out.println(avarage(array));
	}
}
```

# Difference between static and non-static code

Non-static code is linked with the concept of method of objects, like a message that only object that belongs to a class can produce and nothing else. Static code is for declaring constants and utility method which have no sense if it is inside an object.
When we want to unify static with non-static we have to remember that non-static comes first than static code. Otherwise it's more appreciated if static and non-static are defined in separate classes.

All the classes that has only static code are called *module*.

Example Point3D :
```Java
class Point3D { 
    double x;
    double y;
    double z;

    // Constructor
    public Point3D(double a, double b, double c) {
        this.x = a;
        this.y = b;
        this.z = c;
    }

    // Default constructor for zero point
    public Point3D() {
        this(0, 0, 0);
    }

    void build(double a, double b, double c) {
        this.x = a;
        this.y = b;
        this.z = c;
    }

    double getSquaredModulus() {
        return this.x * this.x + this.y * this.y + this.z * this.z;
    }
}

class Points{    //Module Points
    static Point3D zero = new Point3D(); // 0,0,0

    static Point3D max(Point3D[] ps) { 
        if (ps == null || ps.length == 0) {
            return null; 
        }

        Point3D max = ps[0]; 
        for (Point3D elem : ps) {
            if (elem.getSquaredModulus() > max.getSquaredModulus()) {
                max = elem;
            }
        }
        return max;
    }
}

class UsePoint3D {
    public static void main(String[] s) {
        Point3D p1 = new Point3D();
        p1.build(10, 20, 30);
        Point3D p2 = new Point3D();
        p2.build(5, 6, 7);
        Point3D p3 = new Point3D();
        p3.build(100, 100, 100);
        Point3D p4 = Point3D.zero;
        
        Point3D[] array = new Point3D[]{p1, p2, p3, p4};
        
        Point3D max = Point3D.max(array);
        
        if (max != null) {
            System.out.println("Max: " + max);
        } else {
            System.out.println("No points in array.");
        }
    }
}

```

