Multithreading represents a way to implement a *task-oriented* design, where each *task* is defined work that can be done from each thread. Each thread needs to be synchronised with others for handling resources and dependencies.
In OOP languages there are some ways to implements *multithreading*:
- OOP pure + external libraries
- OOP extended with abstraction, like *actor* model, agents,...
- Hybrid: abstraction integrated and support from libraries: Java, C#, Objective C,...

As a reminder, here there are the fundamental distinctions between these concepts:
- *Concurrency*: temporal overlap of the activity execution, even with one processor.
- *Parallelism*: execution in distinct processors.
- *Multithreading*: concurrent handling of threads in a single processor.
# java.util.concurrent
Thread are supported since the creation of Java and the JVM maps them depending on the OS that runs the program ( typically one-to-one) . A thread is represented as an object by the abstract class `Thread` in the package `java.lang`, that has the abstract method `run` that defines the behaviour of our thread.
At runtime, we can create a thread with the method `start`.
When we call `run` instead of `start`, it means that the current thread that is running that piece of program is going to run what is inside the `run` method.

From Java 5 there is a new library called `java.util.concurret` that supports threads.

## Clock example
Thread `Clock` prints the date every `step` milliseconds:
```Java
public class Clock extends Thread {
	private int step;
	
	public Clock(int step){
		this.step = step;
	}
	
	public void run(){
		while (true) {
			System.out.println(new Date());
			try {
				Thread.sleep(step);
			} catch (Exception ex){
				ex.printStackTrace();
			}
		}
	}
}

public class TestClock {
	static public void main(String[] args) throws Exception {
		Clock clock = new Clock(1000);
		clock.start();

		BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
		String input = null;
		System.out.println("READY");
		do {
			input = reader.readLine();
			System.out.println("eco: " + input);
		} while (!input.equals("exit"));
		System.exit(0);
	}
}
```

## Interface `Thread`

Inside the interface `Thread` there are several methods that are significant for the implementation:
- `static void Thread.sleep(long ms)` : it will stop the thread for `ms` milliseconds.
- `getName`
- `boolean isAlive()`
- `void interrupt()` : interrupts the thread leaving it in a wait state.
- `static Thread Thread.currentThread()` : for taking up the reference of the current thread.
# Runnable interface
Exists another way to implement a new thread, based on an interface called `Runnable` :
![[Pasted image 20241205104550.png]]

This interface can be useful when we want a class that not implements a full thread explicitly, but it run in background as a simple thread that contains only the method `run`.

```Java
new Thread(new Runnable() {
	public void run() {
		while (true) {
			System.out.println(new Date());
			try {
				Thread.sleep(2000);
			} catch (Exception ex){}
		}
	}
}).start();
```

# JConsole and VisualVM

These are toolkits for monitoring threads that runs in background:
- *JConsole* :
	- http://java.sun.com/developer/technicalArticles/J2SE/jconsole.html 
	- Included by the JDK
	- Use native support from the JVM
- *VisualVM*: 
	-  https://visualvm.github.io/
	- for monitoring and profiling
	- more advanced than JConsole

# Thread as active component

Threads can be interpreted as objects and in concurrent programming we can resolve problems using them, exploiting hardware resources and consequentially increasing performance.

When we think about thread's design we can imagine them as an *active* *component*, that as a *fixed* *task* and has to be *coordinated* with others.

## `join` mechanism

With `join` method a thread can wait a result from another thread:
```Java
MyThread thread = new MyThread();
thread.start();
System.out.println(“Waiting for thread termination...”);
thread.join();
System.out.println(“Thread completed.”);
```
## Concurrent Sorting

All algorithms that sorts some elements that belong to a certain set until now are sequential, but what about a concurrent way to resolve this problem? Multithreaded solutions can be thought starting from this one:
- *worker*: active components where each of them has a task and a subset.
- *master*: a thread that gives instructions to the others. Once they have finished, it will merge sorted subsets in linear time (`merge` ---> $O(N)$).

```Java
public class SortingMaster extends Thread {

	private int[] vectorToSort;

	public SortingMaster(int[] v) {
		vectorToSort = v;
	}
	
	public void run() {
		int middle = vectorToSort.length / 2;
		SortingWorker w1 = new SortingWorker(0, middle, vectorToSort);
		SortingWorker w2 = new SortingWorker(middle, vectorToSort.length, vectorToSort);
		w1.start();
		w2.start();
		try {
			w1.join();
			w2.join();
			merge(vectorToSort, 0, middle, middle, vectorToSort.length);
		} catch (InterruptedException ex) {
		}
	}

	private void merge(int[] vectorToSort, int from1, int to1, int from2, int to2) {
		int index1 = from1;
		int index2 = from2;
		int i = 0;
		while (index1 < to1 && index2 < to2) {
			int a = vectorToSort[index1];
			int b = vectorToSort[index2];
			if (a < b) {
				vectorToSort[i] = a;
				index1++;
			} else {
				vectorToSort[i] = b;
				index2++;
			}
			i++;
		}
		for (int j = index1; j < to1; j++) {
			vectorToSort[i++] = vectorToSort[j];
		}
		for (int j = index2; j < to2; j++) {
			vectorToSort[i++] = vectorToSort[j];
		}
	}

	//Inner class but it can be a separeted class
	public class SortingWorker extends Thread {
	
		private int minIndex;
		private int maxIndex;
		private int[] data;
	
		public SortingWorker(int minIndex, int maxIndex, int[] data) {
			this.minIndex = minIndex;
			this.maxIndex = maxIndex;
			this.data = data;
		}
	
		public void run() {
			log("starting " + minIndex + "-" + maxIndex);
			Arrays.sort(data, minIndex, maxIndex);
			log("done " + minIndex + "-" + maxIndex);
		}
	
		private void log(String msg) {
			System.out.println(getName() + " " + msg);
		}
	}
}
```

Note: how can we generalize it in $N$ threads?
```Java
public long run() {
	final int size = list.size() % nthread + list.size() / nthread;
	final List<SortingWorker> workers = new ArrayList<>(nthread);
	for (int start = 0; start < list.size(); start += size) {
		workers.add(new SortingWorker(list, start, size));
	}
	for (final SortingWorker w: workers) {
		w.start();
	}
	long sum = 0;
	for (final SortingWorker w: workers) {
		try {
			w.join();
			sum += w.getResult();
		} catch (InterruptedException e) {
			throw new IllegalStateException(e);
		}
	}
	return sum;
}
```

# CPU-bound & IO-bound
A thread can evaluate exclusively computations (CPU-bound) or interactions (IO-bound). This behaviour, that a thread can manifest, can impact performance if program don't divide this two parts.

//Example slide 35

## How to obtain processors number

`Runtime.getRuntime().availableProcessors();`

# Thread cancellation

The thread cancellation is a situation where program doesn't need a thread anymore and it is called *target* *thread*. For example a thread that does a search inside a database, once it finished its task, the program wants its termination for a more successful resource management.
There are two types of terminations:
- *asynchronous* *cancellation*: an external thread that stop target threads (garbage collector)
- *deferred* *cancellation*: the target thread control its termination by itself.

In Java we can stop a thread with an asynchronous cancellation by the method `stop()`.
With deferred cancellation, `run()` controls the thread cancellation.

## `volatile` attribute

`volatile` is a keyword that defines a variable that needs doesn't need to be cached because the program checks it rarely. A program needs to declare volatile variables if they are *atomic* *operation*. 
```Java
public class StoppableClock extends Thread {
	private int step;
	private volatile boolean stopped;
	
	public Clock(int step){
	this.step = step;
	this.stopped = false;
	}
	
	public void run(){
		while (!stopped) {
			System.out.println(new Date());
		try {
			sleep(step);
		} catch (Exception ex){
				System.out.println(“Interrupted!”);
			}
		}
	}
	
	public void forceStop(){
		stopped = true;
		interrupt();
	}
}
```

## Interrupt
The method `interrupt()` can stop a thread by brute force, even if it's blocked inside a `sleep` or `wait` method (note that both generates an `InterruptedException`).

# Competition and communication

There are two types of interactions between threads:
- *Competition*: interaction where two threads need the same resources and do the same task. It needs a *mutual* *exclusion* or defines *critical* *sections*.
- *Cooperation*: interaction where two threads cooperate to resolve a problem. It's important to define a *communication* *system*, where each thread can send/receive messages, or *synchronisation*, where each thread isn't overlapped by others and has its own temporal section for doing a certain task.

## `synchronized` methods and sections

Declaring a section with the keyword `synchronized` it bounds that part of code with a mutual execution, where one thread at a time can execute it.
Other threads are handled by the JVM automatically. Meanwhile these threads can execute other non-synchronized methods.

```Java
public class Resource {
	public synchronized void op() {
		System.out.println("[Resource] Thread " + Thread.currentThread() + " entered.");
		try {
			Thread.sleep(5000);
		} catch (Exception ex) {}
			System.out.println("[Resource] - Thread " + Thread.currentThread() + " exited.");
	}
}
```

### Thread-safe classes

*Thread-safe* classes are classes that ensure mutual exclusion when accessing resources.
With `synchronized` keyword the program this concept of mutual exclusion, where each thread can access one at a time.

If it isn't thread-safe, there could be some errors with operations:
```Java
public class CounterUser extends Thread {
	private long ntimes;
	private Counter counter;

	public CounterUser(String name, Counter c, long n) {
		super(name);
		ntimes = n;
		counter = c;
	}

	public void run() {
		log("starting - counter value is " + counter.getValue());
		for (long i = 0; i < ntimes; i++) {
			counter.inc();
		}
		log("completed - counter value is " + counter.getValue());
	}

	private void log(String msg) {
		System.out.println("[COUNTER USER " + getName() +"] " + msg);
	}

	public static void main(String[] args) throws Exception {
		Counter c = new UnsafeCounter();
		long ntimes = Long.parseLong(args[0]);
		CounterUser agentA = new CounterUser(c, ntimes);
		CounterUser agentB = new CounterUser(c, ntimes);
		agentA.start();
		agentB.start();
		agentA.join();
		agentB.join();
		System.out.println("Count value: " + c.getValue());
	}
}
```
It will get this result if compiled:
![[Pasted image 20241205175328.png]]

To build a thread-safe class we have to define critic sections and then declare them `synchronized`:
```Java
public class SafeCounter {
	private int cont;
	
	public SafeCounter (){ cont = 0; }
	
	public synchronized void inc(){
		cont++;
	}
	
	public synchronized void dec(){
		cont--;
	}
	
	public synchronized int getValue(){
		return cont;
	}
}
```
In terms of results, this program will get the exact result as expected, but on the other hand it costs in terms of performance:
![[Pasted image 20241205175524.png]]



## How it is implemented the JVM in terms of synchronization?
The JVM doesn't specify any particular concurrent politic, there is only one queue where threads acquires a *lock* that gives access to the method they call.
![[Pasted image 20241205175956.png]]
### `synchronized` sections
`synchronized` can be used for a local piece of code:
```Java
AnyClass myLockObject; 
... 
synchronized (myLockObject){
	...
}
```

`myLockObject` will enter the critical section once it receives the lock from other threads.


```Java
class MyWorkerA extends Thread {
	private Object lock;

	public MyWorkerA(Object lock) {
		this.lock = lock;
	}

	public void run() {
		while (true) {
			System.out.println("a1");
			synchronized (lock) {
				System.out.println("a2");
				System.out.println("a3");
			}
		}
	}
}

class MyWorkerB extends Thread {
	private Object lock;

	public MyWorkerB(Object lock) {
		this.lock = lock;
	}

	public void run() {
		while (true) {
			synchronized (lock) {
				System.out.println("b1");
				System.out.println("b2");
			}
			System.out.println("b3");
		}
	}
}

public class TestCS {
	public static void main(String[] args) {
		Object lock = new Object();
		new MyWorkerA(lock).start();
		new MyWorkerB(lock).start();
	}
}
```

## Explicit synchronization
There are 3 methods to do explicit synchronization:
- **`wait()`**:
    - Causes the current thread to **wait** until another thread invokes `notify()` or `notifyAll()` on the same object.
    - Releases the **lock** on the object, allowing other threads to acquire it.
    - The waiting thread is added to the **wait set** of the object (a list of threads waiting for a notification).
- **`notify()`**:
    - Wakes up **one** thread from the wait set of the object.
    - The thread removed from the wait set will attempt to re-acquire the lock before resuming execution.
- **`notifyAll()`**:
    - Wakes up **all** threads in the wait set of the object.
    - Each thread will attempt to re-acquire the lock before continuing execution.


## Monitor pattern
There is a requirement for using these methods --> obtain the lock:
- To call `wait()`, `notify()`, or `notifyAll()`, the thread must first acquire the **lock** on the object.
- These methods are typically called inside `synchronized` blocks or methods to ensure the lock is held

This practice is called *Monitor* *pattern*, where each public method is `synchronized`.

```Java
public class SynchUserA extends Thread {

	private SimpleSynchronizer sync;
	
	public SynchUserA(SimpleSynchronizer sync) {
		this.sync = sync;
	}
	
	public void run() {
		a1();
		a2();
		sync.signal();
		a3();
	}
	//...
}

public class SynchUserB extends Thread {

	private SimpleSynchronizer sync;
	
	public SynchUserB(SimpleSynchronizer sync) {
		this.sync = sync;
	}
	
	public void run() {
		b1();
		try {
			sync.waitForSignal();
			b2();
			b3();
		} catch (Exception ex) {
			log("interrupted");
		}
	}
	//...
}

public class SimpleSynchronizer {

	private boolean signalArrived;

	public SimpleSynchronizer() {
		signalArrived = false;
	}
	
	public synchronized void waitForSignal() throws InterruptedException {
		while (!signalArrived) {
			wait();
		}
		signalArrived = false;
	}

	public synchronized void signal() {
		signalArrived = true;
		notifyAll();
	}

}
```

## Entry Set and Wait Set
![[Pasted image 20241205184119.png]]
Steps:
- Invoke `synchronized` method and enter in the Entry Set:
- Acquire lock
- Execute `wait()`, release lock and enter in the Wait Set
- `notify()` notifies the acquirement of the lock to another of the Wait Set
- Once it execute its statements, it releases lock and exit from the system

# java.util.concurrent
In the `java.util.concurrent` there are some utilities that supports multithreaded programming. For example there are:
- *Concurrent* *Collections* : lists(`CopyOnWriteArrayList`), maps(`ConcurrentHashMap`), queues (`Queue` and `BlockingQueue`), stacks,... (all thread-safe)
- *Synchronizers*: `Locks`, `Semaphores`, `Latches`, `Barriers`
Synchronizers have general properties:
- encapsulate a state that defines when a thread will call them
- provides methods that modifies that state
# Add-on
Java Concurrency in Practice - Brian Goetz et al - Addison Wesley
• Concurrent Programming in Java: Design Principles and Pattern, 2/E
- Doug Lea - Addison-Wesley Professional
• Software and the Concurrency Revolution - by Herb Sutter, James
Larus - ACM Queue - http://queue.acm.org/detail.cfm?id=1095421


# Reactive GUI
As discussed previously, a GUI is implemented with a *Event Dispatcher Thread* that repaints our GUI every time the user utilizes it. This can't update itself if multiple events occur. Then inside it there is an *Event* *Queue* that handles events.
![[Pasted image 20241205190434.png]]
With a threaded point of view, what can happen if an event creates a thread?
The answer is the EDT will be stacked inside and then the GUI collapses.
To manage this problem, there is a way to implement a *reactive* *GUI*:
- Operations done by listeners must be implemented in the controller part and executed by another thread
- The delegation can be performed by the creation of another thread or delegate it to another thread that handles it ([[Chain of Responsibility Pattern]])

## How to realize a GUI without critical sections overlaps

- First of all each component has to be a non-thread-safe classes, for the fact that the only thread that has access to the GUI is the EDT. Then the EDT has the role of *master* for the other threads and deals tasks.
- There are utilities from Swing that helps in facing this problem: 
	- `SwingUtilities.invokeLater` (It schedules a Runnable to be executed on the EDT at some point in the future)  
	- `SwingUtilities.invokeAndWait` (It schedules a Runnable to be executed on the EDT and waits for its completion)

### Chronometer
- Problem: we want a chronometer that has to be implemented in a GUI that has 3 buttons (start, stop and reset) and a edit box for showing the counter(starting from 0)
- First of all we can resolve this problem with an MVC architecture:
	- Model: `ChronoCount`
	- View: `ChronoView` and `ChronoFrame` (Swing)
	- Controller: `Controller` (interface), `ChronoController` (implementation) and `ChronoAgent` (thread)
- Solutions:
	- The Controller updates the View and checks Model
	- The View is a listener of the Model and Controller is an intermediary.

//Check in VS


# Virtual threads(jdk21)

## Executor framework

Native threads in Java can be considered as kernel threads and it can be quite tedious if we want to handle more threads that can do tasks like user threads. Then there is a framework called `Executors` that allows to divide problems in independent tasks (classes that implements `Runnable` and `Callable`) and the execution is delegated to an *executor* that encapsulates strategies to map threads to 1 or more threads (`FixedThreadPool`, `CachedThreadPool`, `SingleThreadPool`,...)

```Java
int size = 1000;
		int n = size; 
		int k = size; 
		int m = size; 
		
		System.out.println("Testing A[" + n + "," + k + "]*B[" + k + "," + m + "]");

		System.out.println("Initialising...");

		Mat matA = new Mat(n,m);
		Mat matB = new Mat(m,k);

		matA.initRandom(10);
		matB.initRandom(10);
		
		System.out.println("Initialising done.");
		System.out.println("Computing matmul...");

		long t0 = System.currentTimeMillis();
		
		Mat matC = new Mat(matA.getNRows(), matB.getNColumns());
	
		int threadPoolSize = Runtime.getRuntime().availableProcessors() + 1;
		ExecutorService exec = Executors.newFixedThreadPool(threadPoolSize);		
			for (int i = 0; i < matA.getNRows(); i++){
				for (int j = 0; j < matB.getNColumns(); j++){
					
					Runnable task = new ComputeElemTask(i,j,matA,matB,matC);

					/* execute task, asynchronously */
					exec.execute(task);
					
					// Alternative: using a lambda expression to specify the task
					/* 					
					exec.execute(() -> {
						double sum = 0;
						for (int k = 0; k < matA.getNColumns(); k++){
							sum += matA.get(i, k)*matB.get(k, j);
						}
						matC.set(i,j,sum);
					});
					*/
				}
			}
		exec.shutdown();
		exec.awaitTermination(Long.MAX_VALUE,TimeUnit.SECONDS);
```

## Virtual thread API

From JDK 21, developers introduces a new light implementation for threads, called *Virtual* *Threads* and can interpreted as *user* *threads*. They are managed partially by JVM by a system that arrange them as a dynamic object that can be attached to a kernel thread, called *carrier* *thread*.
![[Pasted image 20241207102829.png]]
The JVM can deallocate virtual threads if they call a *blocking* *operation* inside the carrier thread. For example, there are several tasks to be done and each virtual thread has one of them. The carrier thread allocate one of them each time it's going to run. When a virtual thread performs a blocking operation, the JVM may decide to detach the virtual thread from its carrier thread and reassign the carrier thread to another task. This allows efficient use of kernel threads despite blocking calls.
```Java
try (final var executor = Executors.newVirtualThreadPerTaskExecutor()) { //Generates a new thread pool
	for (int i = 0; i < 10; i++) {
		final int taskNumber = i;
		executor.submit(() -> {
			System.out.println("Starting task " + taskNumber + " in " + Thread.currentThread()); 
			// Simulating a blocking operation (e.g., reading from a database or file)
			try {
				Thread.sleep(1000); // This blocks the carrier thread
			} catch (InterruptedException e) {
				Thread.currentThread().interrupt();
			}
			
			System.out.println("Finished task " + taskNumber + " in " + Thread.currentThread()); 
		});
	}
}
```
The program demonstrates how each task runs on a virtual thread while still behaving like a traditional thread. Even though blocking calls occur, the JVM ensures minimal impact on performance by managing the carrier threads dynamically. In fact the output isn't sequential:
![[Pasted image 20241207105214.png]]

## Structured Concurrency -> Pattern fork-join (preview)  
```Java
package playtest;
import java.util.concurrent.Callable;
import java.util.concurrent.Executors;
import java.util.concurrent.StructuredTaskScope;
import java.util.stream.IntStream;

public class ForkJoinVirtualThreadsFibonacci {
    public static void main(String[] args) {
        var start = System.currentTimeMillis();
        try (var scope = new StructuredTaskScope<Integer>()) {
            final var subtasks = IntStream.range(0, 10)
                                            .mapToObj(i -> scope.fork(new FibonacciTask(i)))
                                            .toList();
            try {
                scope.join();
            } catch (Exception e) {
                System.err.println(e.getMessage());
            }
            subtasks.stream()
                .map(t -> t.get())
                .toList()
                .forEach(i -> System.out.println("Result: " + i));
        }
        System.out.println("End in: " + (System.currentTimeMillis() - start) + " milliseconds");
    }
}

class FibonacciTask implements Callable<Integer> {
    private final int number;

    FibonacciTask(final int number) {
        this.number = number;
    }

    @Override
    public Integer call() throws Exception {
        // Base cases
        if (number <= 1) {
            return number;
        }

        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            var task1 = executor.submit(new FibonacciTask(number - 1));
            var task2 = executor.submit(new FibonacciTask(number - 2));
            return task1.get() + task2.get();
        }
    }
}

```
## Actor programming
![[Pasted image 20241207134748.png]]
# Bibliography
- Java Concurrency in Practice - Brian Goetz et al - Addison Wesley • Concurrent 
- Programming in Java: Design Principles and Pattern, 2/E - Doug Lea - Addison-Wesley Professional