Arrays in Java can interpreted like objects and they can be manipulated from reference in the heap.
The notation is quite similar to C for access in arrays. Their length must be explicit in the declaration and it can't be modified. If the program goes out of bound there will be a execution error `ArrayIndexOutOfBoundsException`.

# Syntax

There are two notations:
```Java
int[] ar1 = new int[]{10, 20, 30, 40, 50}  //Listing, the size is 5
int[] ar2 = new int[200];   //Dimension

//Alternatives
var ar3 = new int[]{10, 20, 30};
int[] ar4 = {10, 20, 30};
int[][] matrix = new int[200][200];
```

## Access

```Java
ar1.length;   //length of the array
ar2[23] = 24;  //Access to the 24th element of the array, starting from 0
matrix[2][3] = 10;   //Access to the 3rd row and 4th column
```

## Array of objects

It can be declared an array of objects, where if it is initialized for dimension, it will have each element defined at `null`.

```Java
Object[] aro = new Object[]{new Object(), new Object()};
Object[] aro2 = new Object[200];  //Each element initialized at null
```


# Arrays package

In `Arrays` package there are some features that change our arrays in collection that belongs to the `java.util.Collection` class set. In other words we can change our collection as we want.

As regards `String` class, we can split our string depending on what type of delimiters we declare in the *regular* *expression* (or *regex*). When we want to declare them in this variable and define how our words can be split, we can write them with the string construct : `[\\delimeter'+]`.

For example:
```Java
private final static String REGEX = "[^\\w']+[^\\n']"; //All non-words and next line aren't considered

String myString = "Hello World\nA Java program"
String[] arrayString = myString.split(REGEX);
System.out.println(arrayString);
//[Hello, World, A, Java, program]
```
