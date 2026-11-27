- The execution of a concurrent program can be formally represented by states and transitions between states
- The state is defined by a tuple consisting of
	- one element of each process that is a label (statement) from that process
	- one element for each global or local variable that is a value whose type is the same as the type of a variable
	- there is a transition between two states s1 and s2 if executing a statement in state s1 changes the state to s2
- the statement executed must be one of those pointed to by a control pointer in s1
- The state diagram is a graph containing all the reachable states of the programs
- scenarios are represented by directed paths through the state diagram from the initial state
- cycles represent the possibility of infinite computation in a finite graph

# Non-atomic Structures
- The notion of “atomic” can be referred not only to actions, but also to data structures:
	- a data object is defined atomic if it can be in a finite number of states equals to the number of values that it can assume
	- operations change (atomically) that state
- typically primitive data type in concurrent languages are atomic but not always: e.g. double in Java
- Abstract data types composed by multiple simpler data objects are typically non atomic
- In that case for the ADT (or more generally data object) it is possible to identify two basic types of states:
	- *internal*: the state of the object is not observable by other processes and are meaningful for data object definition
    - *external*: the state of the object is observable by other processes and are meaningful for the processes that use the data object
- The correspondence between internal and external states is partial:
	- there are internal states which have no correspondent external state
	- internal states which have a correspondent external state are defined *consistent*
- In ADT there are operations that aren't consistent. For example a simple list cannot go in read and write state at the same time: if a process is reading the list and another process is writing it, the first process will read an inconsistent state of the list
- In sequential programming this problem is resolved by information hiding.
- Then we will have to introduce mechanisms that would guarantee that processes work on data objects that are always in a consistent state. This is the *critical section problem*.
# Critical Section Problem #must
- Introduced by Dijkstra in 1965, the critical section problem can be considered one of the most important and well-studied problem in concurrent programming, in the context of process competition.
- The critical section problem is a problem of process synchronization that arises when multiple processes access shared resources (data structures, files, etc.) and at least one process modifies the resource. The critical section is the part of the code where the shared resource is accessed.
```
P[1..n]
loop {
    // non critical section
    enter_region(i) // protocol
    // critical section
    leave_region(i) // protocol
}
```
- The critical section problem is to design a protocol that the processes can use to cooperate, such that the following requirements are satisfied:
    - *Mutual Exclusion*: statements from the critical sections of two or more processes mustn't be interleaved
    - *No Deadlock*: if some processes are trying to enter their critical sections, then one of them must eventually succeed
    - *Bounded Waiting* (alias "no starvation"): if any process tries to enter its critical section, then that process must eventually succeed
    - *Progress*: once a process starts to execute the statements of its critical section, it must eventually finish.

## Critical Section Analysis
- The CS problem can be solved by using basic mechanisms, like semaphores and locks, and algorithms
- First attempt: *strict alternation*
```
int turn = 1

process: p

Local variables

while true
p1:	non critical-section
p2:	pre-protocol: await turn = 1 -> while (turn != 1); // busy wait
p3:	critical section
p4:	post-protocol: turn = 2
	
	
process: q

Local variables

while true
q1:	non critical-section
q2:	pre-protocol: await turn = 2 -> while (turn != 2);
q3:	critical section
q4:	post-protocol: turn = 1
```
- Protocols may require local or global variables
-  The statement `await turn = 1` is an implementation independent notation for a statements that waits until the condition turn = 1 becomes true.
- In order to prove correctness we can build up the state diagram and examine the set of reachable states and the transitions among them:
	- Triple: `<pi, qj, turn>` where:
		- `pi`  = current control pointer for process P
		- `qj`  = current control pointer for process Q
		- `turn` = content of turn variable
	- The diagram is constructed incrementally starting from the initial state and considering what the potential states are
- Mutual exclusion can be asserted if the set of all accessible states does not contain a state of the form: `<p3, q3, _>`
- Mutual exclusion in LTS: `[]!(p3 && q3)`
- Freedom from starvation: `[](p3 -> <> p4)` and `[](q3 -> <> q4)`
- Freedom from deadlock is satisfied, in fact if some processes are trying to enter the CS (executing the await), then one eventually succeeds
- Freedom from starvation is not satisfied in this solution: a process wishing to enter the critical section may starve indefinitely if the other process remains in an infinite loop inside its non-critical section (where there is not the progress property):

|**Step**|**State / Action**|**Values**|
|---|---|---|
|**Initial**| `turn = 1` | `p` at `p1`, `q` at `q1` |
|**1**|Process $p$ passes `p2` ($\text{turn} = 1$) into critical section `p3`.| `turn = 1` |
|**2**|Process $p$ executes `p4`, sets $\text{turn} = 2$, and returns to `p1`.| `turn = 2` |
|**3**|Process $p$ finishes `p1` quickly and attempts to enter again at `p2: await turn = 2 == 1` (False).| `turn = 2` ($p$ blocks)|
|**4**|Process $q$ stays in non-critical work `q1` and never attempts to enter `q2`.| `turn = 2` (unchanged)|
|**Outcome**|**Process $p$ starves indefinitely.**|Critical section remains empty.|
- Second attempt:
```
boolean wantp = false
boolean wantq = false

process: p

Local variables

while true
p1:	non critical-section
p2:	pre-protocol: await wantq = false
p3:	wantp = true
p4:	critical section
p5:	post-protocol: wantp = false
	
	
process: q

Local variables

while true
q1:	non critical-section
q2:	pre-protocol: await wantp = false
q3:	wantq = true
q4:	critical section
q5:	post-protocol: wantq =false
```
- In this version, starvation is still not satisfied, and more critically, **mutual exclusion itself is violated**: Lines `p2` and `p3` (and similarly `q2` and `q3`) are not executed atomically:
	1. $p$ checks `wantq` at line `p2` and finds it `false`.
	2. Before $p$ executes line `p3` (`wantp = true`), the scheduler switches to process $q$.  
	3. $q$ checks `wantp` at line `q2` and finds it `false`.     
	4. Both processes now proceed to set their flags to `true` and enter the critical section at the exact same time.

| **Step** | **Process p**                          | **Process q**                          | **wantp** | **wantq** | **Note**                       |
| -------- | -------------------------------------- | -------------------------------------- | --------- | --------- | ------------------------------ |
| **0**    | at `p1`                                | at `q1`                                | `false`   | `false`   | Initial state                  |
| **1**    | executes `p2`: checks `wantq == false` | at `q1`                                | `false`   | `false`   | **Condition passes** for $p$   |
| **2**    | _(context switch)_                     | executes `q2`: checks `wantp == false` | `false`   | `false`   | **Condition passes** for $q$   |
| **3**    | executes `p3`: `wantp = true`          | at `q2` (passed)                       | `true`    | `false`   | $p$ claims intent              |
| **4**    | enters `p4: critical section`          | executes `q3`: `wantq = true`          | `true`    | `true`    | $q$ claims intent              |
| **5**    | **Inside Critical Section**            | enters `q4: critical section`          | `true`    | `true`    | **Mutual Exclusion Violated!** |
- Third attempt
```
boolean wantp = false
boolean wantq = false

process: p

Local variables

while true
p1:	non critical-section
p2:	pre-protocol: 	wantp = true
p3: await !wantq
p4:	critical section
p5:	post-protocol: wantp = false
	
	
process: q

Local variables

while true
q1:	non critical-section
p2:	pre-protocol: wantq = true
p3: await !wantp
q4:	critical section
q5:	post-protocol: wantq =false
```
- This suffers from Deadlock:

| **Step**    | **Process p**                 | **Process q**                 | **wantp** | **wantq** | **Description**                                                         |
| ----------- | ----------------------------- | ----------------------------- | --------- | --------- | ----------------------------------------------------------------------- |
| **0**       | at `p1`                       | at `q1`                       | `false`   | `false`   | Initial state.                                                          |
| **1**       | executes `p2`: `wantp = true` | at `q1`                       | `true`    | `false`   | $p$ signals intent to enter.                                            |
| **2**       | _(context switch)_            | executes `q2`: `wantq = true` | `true`    | `true`    | $q$ signals intent to enter.                                            |
| **3**       | executes `p3`: `await !wantq` | at `q2`                       | `true`    | `true`    | $p$ checks `wantq`. Since `wantq == true`, **$p$ blocks / busy-waits**. |
| **4**       | blocked at `p3`               | executes `q3`: `await !wantp` | `true`    | `true`    | $q$ checks `wantp`. Since `wantp == true`, **$q$ blocks / busy-waits**. |
| **Outcome** | **Waiting at `p3`**           | **Waiting at `q3`**           | `true`    | `true`    | **Deadlock.** Neither can proceed to line 4 or 5.                       |

- Fourth attempt: 
```
boolean wantp = false
boolean wantq = false

process: p

Local variables

while true
p1:	non critical-section
p2:	pre-protocol: 	wantp = true
p3: while wantq
p4:		wantp = false
p5:		wantp = true
p6: 	critical section
p7: wantp = false
	
	
process: q

Local variables

while true
q1:	non critical-section
q2:	pre-protocol: 	wantq = true
q3: while wantp
q4:		wantq = false
q5:		wantq = true
q6: 	critical section
q7: wantq = false
```
- Deadlock occurs when both processes simultaneously insist on entering their CS: the next step to solve the problem is requiring a process to give up its intention to enter its CS if it discovers that it is contending with the other process.
# Dekker's Algorithm
- This solution was found for the first time by the dutch mathematician T.J. Dekker, in 1965
```
boolean wantp = false
boolean wantq = false
int turn = 1

process: p

while true
p1:	non critical-section
p2:	pre-protocol: 	wantp = true
p3: while wantq
p4: 	if turn = 2
p5: 		wantp = false
p6: 		await turn = 1
p7: 		wantp = true
p8: critical section
p9: turn = 2
p5:	post-protocol: wantp = false
	
	
process: q

while true
q1:	non critical-section
q2:	pre-protocol: wantq = true
q3: while wantp
q4: 	if turn = 1
q5: 		wantq = false
q6: 		await turn = 2
q7: 		wantq = true
q8: critical section
q9: turn = 1
q10:	post-protocol: wantq = false
```
- Mutual exclusion: `[]!(p8 && q8)`
- Freedom from starvation: `[]((p2 -> <> q8) && (q2 -> <> q8))`

# Peterson's Algorithm
- Peterson (1981), Manna-Pnueli, Doran-Thomas
- Peterson's solution is more concise, by collapsing the two await statements into one with a compound condition
```
boolean wantp = false
boolean wantq = false
int turn = 1

process: p

while true
p1:	non critical-section
p2:	wantp = true
p3: turn = 2
p4: while !wantq || if turn = 1
p5: 	critical section
p6	wantp = false
	
	
process: q

while true
q1:	non critical-section
q2:	wantq = true
q3: turn = 1
q4: while !wantp || if turn = 2
q5: 	critical section
q6	wantq = false
```
- Problem with atomicity

# Bakery Algorithm
- An alternative solution to CS problem for N-process accounts for introducing a ticket to establish the turn of a process
```
int num = 1
int next = 1
turn[1:n] = [0,0,0,0,...]

p[i]
while true
p1: NCS
p2: < turn[i] = num; num = num + 1 >
p3: await turn[i] = next
p4: CS
p5: next = next + 1
```
- Problem: arithmetic overflow for turn and num
# Compound Atomic Statements
- Many CS problems can be simplified by introducing *compound atomic statements*
- a commonly used notation for specifying atomicity is to put angled bracket around the group of statements
## Test and Set
- As seen in earlier broken protocols, a thread can be preempted between the read and the write, breaking mutual exclusion.
- **Test-and-Set** solves this at the hardware/CPU level by executing both actions **in a single, uninterrupted atomic cycle**:
	1. Reads the old value at a specific memory location.
	2. Writes `1` (or `true`) into that memory location.
	3. Returns the old value that was read.
- test-and-set is an atomic statement defined as the execution of the two following statements with no possibility of interleaving:
```
test-and-set (x,r): 
	< r := x, x := 1 >
	
bool test_and_set(bool *target) {
    bool old_val = *target;
    *target = true;
    return old_val;
}
```
- Because it is atomic, if multiple cores execute test_and_set on the same address at the exact same instant, the hardware serializes them. Exactly one core reads false and writes true; all other cores read true and write true.
- Starvation with multiple processes unless we make fair assumption in scheduling
- CS with test-and-set:
```
integer lock := 0;

p 

int was_locked

while true
	NCS	
	do
		test_and_set(lock, was_locked)
	until was_locked = 0
	CS
	lock = 0
	
q

int was_locked

while true
	NCS	
	do
		test_and_set(lock, was_locked)
	until was_locked = 0
	CS
	lock = 0
```
## Locks
- By exploiting compound atomic statement we can easily realize a basic lock mechanism
- We define two atomic operations:
	- `acquire` the lock
	- `release` the lock
```
lock sharedlock;

p
while true
	NCS
	acquire(sharedlock)
	CS
	release(sharedlock)
	
q
while true
	NCS
	acquire(sharedlock)
	CS
	release(sharedlock)
```
# Model-Checking Concurrent Programs
## SPIN and PROMELA 
- SPIN is a widely used model- checker used in both academic research and industrial software development – extremely efficient – used in modelling and designing concurrent and distributed systems in general 
- PROMELA is the language that is used in Spin to write concurrent programs modelling language – limited number of constructs intended to be used to build models of concurrent systems
![[Pasted image 20261020113621.png]]
## TLA and PlusCal 
- A formal specification language introduced by Leslie Lamport 
- Based on simple discrete math (set theory and predicates) –
- http://research.microsoft.com/en-us/um/people/lamport/tla/ tla.html 
- Used to specify and verify complex real-world systems. i.e.: Amazon Cloud •
- "Use of Formal Methods at Amazon Web Services” 
- http://research.microsoft.com/en-us/um/people/lamport/tla/amazon.html 
- A TLA+ specification describes the set of all possible legal behaviors (execution traces) of a system. 
- can be used to describe both 
	- the desired correctness properties of the system (the ‘what’) 
	- the design of the system (the ‘how’) 
- TLA+ objective: make it possible to show that a system design correctly implements the desired correctness properties
	- either via conventional mathematical reasoning 
	- by using tools such as the TLC model checker • a tool which takes a TLA+ specification and exhaustively checks the desired correctness properties across all of the possible execution traces 
- PlusCal (formerly called +CAL) is an algorithm language based on TLA+ for writing algorithms, just as a programming language is for writing programs 
- a PlusCal algorithm is translated to a TLA+ specification,
![[Pasted image 20261020114013.png]]
## Java Path Finder (JPF)
- JPF is a special JVM executing programs theoretically along all possible scenarios (execution paths), checking for property violations
![[Pasted image 20261020111544.png]]
[[Java Path Finder]]

- Bibliography: “Principles of Concurrent and Distributed Programming", M. Ben-Ari, Addison-Wesley



## Thread Safety
- Definition: a class is *thread-safe* if it continues to behave correctly when accessed from multiple threads
- guarantees that _nothing bad happens_
- Thread-safe classes encapsulate any needed synchronization so that clients need not to provide their own
- Specifically, this shifts the responsibility of concurrency management entirely onto the class itself:
	- **Encapsulation of Synchronization:** Any necessary locking, atomic operations, or state management are built directly _inside_ the thread-safe class.
	- **Plug-and-Play Usage:** Multiple threads can call the object's methods directly (e.g., `counter.increment()`) without wrapping those calls in their own `synchronized` blocks or external locks.
- How to check if a class is thread safe:
	- looking for **invariants** constraining an object's state
	- we can define **post-conditions** describing the effects of its operations
- Writing thread-safe code is about managing access to state and in particular to shared, mutable state:
	- **shared**: variable or object could be accessed by multiple threads
	- **mutable**: its value could change during its lifetime
- if multiple threads access the same mutable state variable without appropriate synchronization, the program is broken due to *race conditions*
- This means that stateless objects are always thread-safe
- **Race conditions**: occurs when a computation’s correctness depends on the relative timing or interleaving of multiple threads by the runtime scheduler. A more formal systems-level definition consists of three necessary conditions occurring simultaneously:
	- **Shared Mutable State:** Two or more concurrent execution flows (threads or processes) access the same memory location or resource, and at least one of them modifies (writes to) it.
	- **Non-Atomic Compound Operations:** The logical operation requires multiple low-level steps (such as _read-modify-write_ or _check-and-act_) that can be interrupted mid-way by a context switch.
	- **Lack of Mutual Exclusion / Synchronization:** Nothing prevents another thread from observing or altering that shared state in the middle of those steps.
- Main examples (usually are compound statements):
	- **Lost update**: when executing concurrently non-atomic read-modify-write operations
	- **Check and Act**: when a potentially state observation is used to make a decision on what to do next
### Thread Safety in Java
- `synchronized` blocks for defining atomic statement blocks:
```
synchronized(lock){
	statement
	statement
	statement
}
```
- There are 2 parts that has to be fulfilled:
	- a reference to an object that will serve as the lock
	- block of code to be guarded by the lock
- The lock is automatically acquired and then released by a thread respectively when entering and exiting the block
	- if the lock is already acquired, the thread is blocked (suspended) and added to the entry set
	- when a thread exited the block, one thread of the entry set is selected and re-activated
	- no ordering policy is specified
	- if the lock is not released by the thread inside the block, threads in the entry set are blocked forever, leading to **starvation**
- A critical architectural property of any lock is how it tracks ownership when a thread that _already_ holds a lock attempts to acquire it again:
	- **Per-Invocation Locks (Non-Reentrant):** The lock only tracks whether it is "locked" or "unlocked," not _who_ owns it. If a thread holding the lock tries to acquire it a second time, it blocks waiting for itself to release it, resulting in an immediate **self-deadlock**. This is the default behavior for standard POSIX threads (`pthread_mutex_t`).
	- **Per-Thread Locks (Reentrant):** Ownership is tracked by **thread identity** alongside an internal **acquisition count**.
		- When an unheld lock is acquired by Thread A, its count becomes `1` and its owner is set to Thread A.
		- If Thread A requests the same lock again, the JVM recognizes Thread A as the current owner, allows it through immediately, and increments the count to `2`.
		- The lock is only truly freed and made available to other threads in the entry set when Thread A exits all nested blocks and the count drops back to `0`.
- Why Reentrancy Matters in Object-Oriented Design:
```
public class Widget {
	public synchronized doSomething(){...}
}

public class LoggingWidget extends Widget {
	public synchronized void doSomething(){
		System.out.println(toString()+”: calling doSomething”);
		super.doSomething();
	}
}
```
- Without reentrancy, standard OOP inheritance breaks down under concurrency. In the `LoggingWidget extends Widget` example, both the subclass and superclass override `doSomething()` as `synchronized` methods (both locking on `this`). When a thread calls `loggingWidget.doSomething()`, it acquires the lock on `this`, and then calls `super.doSomething()`, which attempts to acquire the _exact same lock_ on `this`. Because Java intrinsic locks are reentrant, this succeeds seamlessly instead of deadlocking the thread.
- While locks guarantee safety, they inherently force **sequentialization**, turning parallel execution back into single-threaded execution for the duration of the critical section. Holding a lock for too long (coarse-grained locking) severely degrades throughput and scalability, so critical sections should be kept as narrow as possible without compromising atomicity.
## Thread Liveness
- Definition: a class guarantees that threads continue making forward progress rather than getting permanently stuck or starved of
- guarantees that _something good eventually happens_
- Fundamental tension: the very mechanisms used to prevent race conditions can halt program progress entirely.
- **Deadlock**: occurs when two or more competing threads are permanently blocked, each waiting for a resource held by another thread in the group so that none can ever finish. There are four necessary conditions (Coffman  conditions) [[Process Interaction Mechanisms and Problems]]
- There are many techniques to detect and recover from deadlocks, for example:
	- Database's transactions typically acquire many locks until they commit
	- How to check if multiple could encounter a deadlock state? See the *dependency graph*
		- Looking for cycles
		- if a cycle is found, a victim is selected and the transaction aborted
- If there is no automated deadlock detection/recovery mechanism (like in JVM) then *a post-mortem diagnosis* must be executed 
- Thread dump content:
	- Stack trace for each running thread
	- locking information
### Common deadlock Patterns in Java
- **Lock-Ordering Deadlocks:** Thread A acquires lock `L` and tries to acquire `M`, while Thread B holds `M` and tries to acquire `L` (e.g., concurrent `transferMoney(acc1, acc2)` and `transferMoney(acc2, acc1)` calls). if we must acquire multiple locks, lock ordering must be part of the design
	- minimizing the number of potential locking interaction
	- document a lock-ordering protocol for locks that may be acquired together
	- There are also techniques that could help in this way: timed locks, detecting and recovering from deadlocks using tryLock feature of Lock classes instead of intrinsic lock
- **Cooperating Object Deadlocks (Alien Method Calls):** Even when no single method explicitly nests two `synchronized` blocks, calling an external (`synchronized`) method while holding a lock creates hidden lock nesting. In the Observer pattern example, `MyThreadA` locks `objA` (`changeState1`) and calls `objB.notifyStateChanged()`, while `MyThreadB` locks `objB` (`getOverallState`) and calls `objA.getState()`—forming a cyclic dependency across object boundaries.
```Java
interface IObserved {
	int getState();
	void register(IObserver obj);
}

class MyEntityA implements IObserved {

	private List<IObserver> obsList;
	private int state;

	public MyEntityA(){
		obsList = new ArrayList<IObserver>();
	}

	public void register(IObserver obs) {
		obsList.add(obs);
	}

	public synchronized int getState() {
		return state;
	}

	public synchronized void changeState1() {
		state++;
		for (IObserver o: obsList){
			o.notifyStateChanged(this);
		}
	}
	
	public synchronized void changeState2() {
		state--;
		for (IObserver o: obsList){
			o.notifyStateChanged(this);
		}
	}
}

interface IObserver {
	void notifyStateChanged(IObserved obs);
}

class MyEntityB implements IObserver {
	List<IObserved> obsList;
	
	public MyEntityB(){
		obsList = new ArrayList<IObserved>();
	}
	
	public synchronized void observe(IObserved obj){
		obsList.add(obj);
		obj.register(this);
	}
	
	public synchronized void notifyStateChanged(IObserved obs) {
		synchronized(System.out){
			System.out.println("state changed: "+obs.getState());
		}
	}

	public synchronized int getOverallState() {
		int sum = 0;
		for (IObserved o: obsList){
			sum += o.getState();
		}
		return sum;
	}
}

class MyThreadA extends Thread {
	MyEntityA obj;
	public MyThreadA(MyEntityA obj){
		this.obj = obj;
	}
	public void run(){
		while (true){
			obj.changeState1();
			obj.changeState2();
		}
	}
}

class MyThreadB extends Thread {
	MyEntityB obj;
	public MyThreadB(MyEntityB obj){
		this.obj = obj;
	}

	public void run(){
		while (true){
			log("overall state: "+ obj.getOverallState());
		}
	}

	private void log(String msg){
		synchronized(System.out){
			System.out.println("["+this+"] "+msg);
		}
	}

}

public class Test2 {
	public static void main(String[] args) {
		MyEntityA objA = new MyEntityA();
		MyEntityB objB = new MyEntityB();
		objB.observe(objA);
		new MyThreadA(objA).start();
		new MyThreadB(objB).start();
	}
}
```
- **Resource Deadlocks:** Occur when bounded resources (like fixed-size thread pools or semaphores) are exhausted because tasks inside the pool block waiting for results from other tasks queued in the same pool.