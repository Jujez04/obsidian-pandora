It's a [[JDK]] component that stands inside the JVM, where automatically works with a frequency which depends on the memory state. Every time it's called up, it starts looking for objects that are not used in the program. Once it checked up all the heap, it deallocate all the objects that does not a reference in the stack.

Example on how it works : 
```Java
class GC{  
	private static long size = 1000;
	public static void main(String[] s){ 
		 Runtime r = Runtime.getRuntime(); 
		for(long l = 0; true; l++){
			new Object();
			if ((l%size)== 0){ 
				System.out.print(" Objs (*10^6) : " + l / 1000000); 
				System.out.println(" Freemem (MB):" + (r. freeMemory () >> 20) );
			} 
		} 
	} 
}
```

We notice that every time the compiler creates some objects, the garbage collector's algorithm starts to deallocate objects that don't have a reference in the heap (in a manner that reminds a lung). 
# Mark and Sweep