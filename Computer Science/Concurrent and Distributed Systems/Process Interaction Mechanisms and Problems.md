- Interleaving communications provided by algorithms can help in achieving concurrency in bare machines.
- Interaction mechanisms are abstractions for process coordination and are supported by concurrent machines
- Assumption:
	- Process states:
		- Executing: The process is currently using the CPU and executing instructions (e.g., doing normal computations, or executing the `S.V = S.V - 1` part of a `wait` operation).
		- Ready: The process is able to run, but it is currently waiting for the CPU to be assigned to it.
		- Blocked: The process is suspended. It cannot run even if the CPU is free because it is waiting for a specific event to happen (like a semaphore becoming available).
# Semaphores
- Introduced by Dijkstra in 1968
- A semaphore is a synchronization variable in concurrent systems that controls access to a shared resource using a counter and atomic operations. It prevents race conditions and manages how many threads or processes can enter a critical section at the same time.
## Data Types
- A semaphore S is a compound data type with two fields:
	- `S.V` is an integer $k\geq 0$
	- `S.L` is a set of process that can be initialized with the empty set `{}` and it corresponds to the set of *waiting processes*
- Notation: `(k, {})`
- Operations:
	- `wait(S)`:
		- `wait(S)= < if (S.V > 0) then S.V = S.V - 1 else S.L = S.L + {p}; p.state = blocked >`
		- if the value of the semaphore V is > 0 (~the semaphore is green), then it is simply decremented
		- otherwise if the value V = 0 (the semaphore is red), then the process is blocked
		- Atomic Operation
	- `signal(S)`:
		- `signal(S)= < if (S.L = {}) then S.V = S.V + 1 else let q be an arbitrary element of S.L; S.L = S.L - {q}; q.state = ready >`
		- If no process is waiting, then the semaphore value is incremented
		- otherwise select a process q blocked on the semaphore, and unblock it.
		- Atomic Operation
- Let k be the initial value of the integer component of the semaphore, `#signal(S)` the number of `signal(S)` statements that have been executed, and `#wait(S)` the number of `wait(S)` statements that have been completely executed.
- Theorem: A semaphore S satisfies the following invariants:
```
S.V >= 0
S.V = k + #signal(S) - #wait(S)
```

## Types of semaphores
- **Mutex Semaphores**: semaphores in which integer component can take only two values, 0 and 1; useful for mutual exclusion
- **General or Counting or Resource Semaphores**: $k$ could be any number $\geq 0$
- **Event Semaphores**:
	- initialised with 0, used for synchronisation purpose
	- They are used in **reentrant OS**: **reentrancy** refers to the ability of a program, routine, or kernel to be safely interrupted during its execution and re-entered (called again) before the first invocation has finished, without corrupting data or causing errors
## Definitions of semaphore types
- differences relate to the specification of liveness properties, and do not affect the safety properties that follow from the semaphore invariants
### Strong and Weak Semaphores
- **Weak semaphores**: `S.L` is a set
- **Strong semaphores**: `S.L` is a queue
```
wait(S) 
	if (S.V > 0)
		S.V ← S.V - 1
	else
		append(S.L,p)
		p.state ← blocked
		
signal(S)
	if (S.L = empty_queue)
		S.V ← S.V + 1
	else
		let q ← take(S.L)
		q.state ← ready
```
- Thanks to *order property* in queues there is **no starvation**.
### Busy-wait Semaphores
- Semaphores without `S.L`
```
wait(S):
	await(S.V > 0)
	S.V = S.V - 1

signal(S):
	S.V = S.V + 1
```
- "No starvation" property can't be ensured
- Busy-wait semaphores are appropriate in a multiprocessor system when the waiting process has its own processor and is not wasting CPU time that could be used for other computation
- Or rather in systems with a little contention

## Critical Section with Semaphores
![[Pasted image 20261105182402.png]]
- Proving correctness with the reduced state diagram:
![[Pasted image 20261105182525.png]]

![[Pasted image 20261105182603.png]]
- States in which processes are blocked: `q1B, p1B`
- Properties ensured:
	- Mutual Exclusion: there is no state `<p2, q2, {0, {}}>`
	- No deadlock and no starvation: there is a deterministic path in which both `p2` and `q2` are executed
- With N processes there is no assurance that a process could be entirely executed due to the fact that the deterministic path became a non-deterministic path in which it could be chosen one process instead of another one -> no freedom from starvation
## Synchronization with Semaphores
- Semaphores provide a basic mechanism also to synchronize processes
- **Event Semaphores**: 
	- Used to *send/receive* a temporal signal
	- Initialized to $(0, \{\})$
- Ex: `b2` executed after `a2` in the following code: 
```
R

c1
c2
signal(c2)
wait(a2)
wait(b2)

P

a1
a2
signal(a2)

Q

b1
b2
signal(b2)
```
- Ex: Merge sort
```
binary semaphore S1 ← (0,{})
binary semaphore S2 ← (0,{})
integer array A

S1

p1: sort 1st half of A
p2: signal(S1)

S2

q1: sort 2nd half of A
q2: signal(S2)

Merge

r1: wait(S1)
r2: wait(S2)
r3: merge halves of A

Trace


```
- *Synchronization barrier* -> r3

> [!question] In which part synchronization is involved with event semaphores?
> It is related to the fact that the synchronization event is `(0,{})` and both wait and signal operations are atomic. 
# Producer-Consumer Problem
- *order-of-execution problem*
- Two types of processes:
	- **producers**: producing data that must be consumed
	- **consumers**: consuming data produced by producers
- The problem is to ensure that the producer does not produce data when the buffer is full and that the consumer does not consume data when the buffer is empty.
- This problem is recurrent in CS and can be solved using semaphores.
- As a middleware we can use a *buffer*

| Producer                  | Buffer (Bounded)                          | Consumer                  |
| ------------------------- | ----------------------------------------- | ------------------------- |
| `Loop:`                   | `sem availablePlaces(N, {})`              | `Loop:`                   |
| `it = produce()`          | `sem available_items(0,{})`               | `wait(available_items)`   |
| `wait(availablePlaces)`   | `sem mutex(1)` #opt                       | `wait(mutex)` #opt        |
| `wait(mutex)` #opt        | `BoundedQueue<Item> buffer ← empty queue` | `it = buffer.remove()`    |
| `buffer.append(it)`       |                                           | `signal(mutex)` #opt      |
| `signal(mutex)` #opt      |                                           | `signal(availablePlaces)` |
| `signal(available_items)` |                                           | `consume(it)`             |
- Invariant: `AvailablePlaces + AvailableItems = N`
- Bounded buffer -> `N` is a finite number
- #opt As a generalisation of previous case, we consider the shared use of a non-atomic data structure (a buffer in this case), so with non-atomic operations
- #opt introducing a mutex for guaranteeing also mutual exclusion
# Dining Philosophers Problem
- originated by an examination question set by Dijkstra in 1971 on a synchronization problem where five computers competed for access to five shared tape drive peripherals
- Description: 
	- there is a secluded community of five philosophers who engage in only two activities: thinking and eating
	- meals are taken communally at a table set with 5 plates and 5 forks (or chopsticks)
	- in the centre of the table there is a bowl of spaghetti that is endlessly replenished.
	- the spaghetti is hopelessly tangled and a philosopher needs two forks in order to eat
	- each philosopher may pick up the forks on his left and on his right, but only one fork at a time
- Properties:
	- a philosopher can eat only if he/she has two forks
	- mutual exclusion: no two philosophers may hold the same fork simultaneously
	- No deadlock/starvation
	- Efficient behaviour

> [!info] First Attempt
>> |Philosophers $[i \in N]$ |Shared|
>> |---|---|
>> | `Loop:`<br>`think()`<br>`wait(fork[i])`<br>`wait(fork[(i+1)%N])`<br>`eat()`<br>`signal(fork[i])`<br>`signal(fork[(i+1)%N])` | `semaphore fork[N] = {1, 1, ...};` |
- Deadlock: no chopsticks are held by at least two philosophers in different times
- An important note to highlight is the *Coffman rule*, in which he write about the *necessary conditions* for a deadlock to occur:
	- **Mutual exclusion.** A resource can be used by only one process at a time.
	- **Hold and wait.** A process holding some resources can request additional ones while it keeps what it already has.
	- **No preemption.** Nobody can forcibly take a resource from a process. It is released only when the process itself calls `signal`.
	- **Circular wait.** There is a cycle of processes, each waiting for a resource held by the next.
- The idea is that a deadlock can happen only *if all four conditions hold at the same time*.
- In this case:
	- Mutual exclusion: each fork is a binary semaphore
	- Hold and wait: everyone holds one fork and requests another
	- No preemption: semaphores can’t be revoked
	- Circular wait: P0→P1→P2→P3→P4→P0
- All four hold, so the system deadlocks.
- There are systems that adopted some mechanisms of deadlock **detection** and **recovery**:
	- In databases the system is designed in a way that transactions have many locks and they must acquire to execute and commit.
	- Some systems are based on *is-waiting* dependency graph, in which deadlocks can be represented as cycles and an edge can be selected to abort that transaction
- If systems don't have this support there are other solutions, like restart the application or post-mortem analysis
- In the case of dining philosophers we have many solutions:
> [!info] Solution
>> |Philosophers $[i \in N]$ |Shared|
>> |---|---|
>> | `Loop:`<br>`think()`<br>`wait(ticket)`<br>`wait(fork[i])`<br>`wait(fork[(i+1)%N])`<br>`eat()`<br>`signal(fork[i])`<br>`signal(fork[(i+1)%N])`<br>`signal(ticket)` | `semaphore fork[N] = {1, 1, ...};`<br>`semaphore ticket = {4, {}}` |
>> 
>> |Philosophers $[i \in N]$ |Shared|
>> |---|---|
>> | `integer first = min(i,(i+1)%N)` <br> `integer second = max(i,(i+1)%N)`<br>`Loop:`<br>`think()`<br>`wait(fork[first])`<br>`wait(fork[second])`<br>`eat()`<br>`signal(fork[first])`<br>`signal(fork[second])` | `semaphore fork[N] = {1, 1, ...};` |
- How these solutions break the conditions:

| Solution                                | Condition broken                                                                                                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Tickets (N−1 philosophers at the table) | Circular wait: with at most 4 philosophers competing for 5 forks, at least one can always get both forks, eat and release, so a full cycle can’t close |
| Ordered forks                           | Circular wait: the last philosopher picks fork 0 before fork 4, so the chain is broken                                                                 |
- **General rule**: broke Circular wait by design; if everyone acquires locks in increasing order, a cycle is impossible
	- giving every lock a unique number and then agreeing that every process must acquire locks in increasing order of that number.
	- For any two different locks, one is always strictly before the other. There are no ties and no incomparable pairs.
	- acquire the locks always in the same order
- Circular wait is the condition attacked most often in practice. Mutual exclusion is usually the point of the lock, and preemption is generally unsafe, because forcibly taking a lock mid-critical-section can corrupt state. Hold-and-wait is harder to avoid, since it would mean acquiring everything atomically or nothing.
# Readers and Writers Problem
- There are two main classes of process:
	- **Readers**: which are required to exclude writers but not other readers
	- **Writers**: which are required to exclude both readers and other writers
- Description:
	- There are some readers that want to read a value stored inside a shared resource
	- There is a writer that wants to write a value inside a shared resource
- The problem is an abstraction of access to databases (or any kind of shared resource)
- There are some invariants that must satisfied when shared resource is accessed:
```
nR >= 0
nW = 0 || nW = 1
(nR > 0 implies nW = 0) and (nW = 1 and nR = 0)
nR = number of readers, nW = number of writers
```
> [!info] First attempt (over-constrained)
>> |Reader|Writer|Shared|
>> |---|---|---|
>> | `Loop:`<br>`wait(rw)`<br>`el = read(dbase)`<br>`signal(rw)` | `Loop:`<br>`wait(rw)`<br>`el = create_record()`<br>`write(dbase, el)`<br>`signal(rw)` | `binary semaphore rw = {1, {}};`<br>`DataBase dbase;` |
- Each reader and writer has exclusive access to the dbase
- **over-constrained** solution
> [!info] Solution
>> |Reader|Writer|Shared|
>> |---|---|---|
>> | `Loop:`<br>`wait(mutexR)`<br>`if (nr == 0)`<br>`  wait(rw)`<br>`nr = nr + 1`<br>`signal(mutexR)`<br>`el = read(dbase)`<br>`wait(mutexR)`<br>`nr = nr - 1`<br>`if (nr == 0)`<br>`  signal(rw)`<br>`signal(mutexR)` | `Loop:`<br>`wait(rw)`<br>`el = create_record()`<br>`write(dbase, el)`<br>`signal(rw)` | `binary semaphore mutexR = {1, {}};`<br>`binary semaphore rw = {1, {}};`<br>`int nr = 0;`<br>`DataBase dbase;` |
- This solution gives **priority to readers**. While readers keep arriving, `nr` never drops to 0 and writers can **starve**.

# Other problems
- Barber shop problem (Djikistra)
- Cigarette smokers problem (Patil, 1971)
- ...
- The Santa Claus problem (Bansal 2006)

## Cigarette Smokers Problem
- Synchronisation problem proposed by S. Patil in 1971, to investigate the limits of the semaphore primitive 
- Problem statement 
	- assume that there is a group of four people: 3 smokers and 1 agent (arbiter). To roll and smoke a cigarette three ingredients are needed: paper, tobacco, matches. One of the smokers has an infinite supply of papers, another has an infinite supply of tobacco, and another has an infinite supply of matches. The agent has an infinite supply of all three ingredients. 
	- the four participants repeatedly perform the following: the agent puts two ingredients on the table; the smoker who has the remaining ingredient takes the two ingredients, rolls a cigarette, smokes it, and notifies the agent on completion. Then the agent puts another two ingredients on the table, and so on 
	- the problem is to write a program to synchronize the agent and the smokers
- Patil's Argument:
	- Patil's argument was that Edsger Dijkstra's semaphore primitives were limited by using the cigarette smokers problem to illustrate this point by saying that it cannot be solved with semaphores
	- However, Patil placed heavy constraints on his argument:
![[Pasted image 20261108110117.png]]
	- the solution is not allowed to use conditional statements or an array of semaphores.
- With these two constraints, a semaphore-based solution to the cigarette smokers problem is impossible
# Monitors

>[!Definition] Monitor
>A **monitor** is a data abstraction that bundles together some shared state, the operations that access it, and the synchronization rules for using it concurrently. More in particular it is an object that uses a *mutual exclusion policy* and a *synchronisation* mechanism to enforce correctness.
- The synchronization lives **inside** the abstraction, and the callers just invoke operations.
- It is a synchronization object: state + operations + concurrency policy
- This is a generalization of the *kernel/supervisor* problem, in which memory allocation must be done by a single program with privileged 
- **Implicit mutual exclusion**: At most one process can be active inside the monitor at a time. If a process calls a procedure while another is already executing one, it is suspended until the monitor is free. You don’t write any `wait(mutex)` yourself.
- Monitors uses **Condition variables** as *synchronization* *events* to determine if the process is inside or outside itself.
- Abstract Representation:
```
monitor MonitorName {
	declaration of permanent variables
	initialization statements
	procedures(or entries)
}
```
- Main properties:
	- **only operations (procedures) name are visible outside the monitor**
	- **statements within the monitor cannot access variables declared outside del monitor**
	- **permanent variables are initialised before any procedure is called**
- Example:
```
monitor Counter {
    private int count;
    
    procedure inc() { 
	    count := count + 1 
	}
	
    procedure getValue() {
	    return count 
	}
}
```
- Important point: There is no explicit queue associated with the monitor procedure -> **starvation** problem

## Condition variables
- Gives an *explicit* synchronization mechanism
	- used inside the monitors by the programmers to delay a process that cannot safely continue executing until the monitor’s state satisfies some boolean condition
	- used also to awake a delayed process when the condition becomes true
- They can be intended as a form for modeling **events**
- Mutual exclusion alone isn’t enough when a process must wait for something to become true (e.g. “buffer not empty”). For that, a monitor has condition variables with two operations:
	- `waitC(cond)`: block the calling process and **release the monitor lock**, so others can enter and change the state.
	- `signalC(cond)`: wake up one process waiting on that condition (no effect if nobody is waiting).
- Each condition variable refers to a shared FIFO queue of blocked processes
- Abstract representation:
```
waitC(cond) {
	append p to cond.queue
	p.state := blocked
	monitor.lock := release 
}

signalC(cond) {
	if cond.queue != empty
	q := remove head of cond.queue
	q.state := ready
}
```
- Example:
```
monitor SynchCell {
    int value;
    boolean available := false;
    cond isAvail;

    procedure set(int v) {
        value := v
        available := true
        signalC(isAvail)
    }
    procedure get(): int {
        if (!available)
            waitC(isAvail)
        return value
    }
}
```
- A `get` called before any `set` blocks on `isAvail`. When `set` runs, it wakes the waiting reader.
- **wait operation releases the monitor lock**: This is essential to avoid that a process executing a waitC would block the access to the monitor
	- Condition variables only exist inside a monitor
	-  `waitC` releases the monitor lock: consider what happens when a process inside a procedure discovers it cannot proceed (e.g., `get()` finds `available == false`) and calls `waitC(isAvail)`.
		- The waiting process would remain blocked **while still holding the monitor lock**.
		- No other process could enter the monitor, including the producer that needs to call `set(v)` to make `available = true` and `signalC(isAvail)`.
		- The producer could never wake the consumer → **deadlock**.
	- So `waitC` does two things atomically:
		- Adds the process to the condition's queue and blocks it.
		- **Releases the monitor lock** (`monitor.lock := release`).
- There are also other primitives:
	- `emptyC(cond)`: check if the queue is empty
	- `signalAllC(cond)`: like signal, but all the processes waiting on the condition are resumed
	- `waitC(cond,rank)`: wait in order of increasing value of rank
	- `minrank(cond)`: returns the value of rank of process at front of wait queue
- Example:
```
monitor ImprovedSynchCell {

	int value;
	boolean available;
	cond isAvail;

	procedure set(int v){
		value := v
		available := true
		signalC(isAvail)
	}

	procedure get():int {
		if (!available){
			waitC(isAvail)
			signalC(isAvail)
		}
		return value
	}
}
```
- Differences with semaphores:

| Semaphore                                                         | Monitor condition variable                                                                                                             |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `wait` may or may not block                                       | `waitC` always blocks                                                                                                                  |
| `signal` is never lost (increments the value)                     | `signalC` is lost if no one is waiting                                                                                                 |
| a process unblocked by signal can<br>resume execution immediately | depending on the specific signaling semantics, a process unblocked by signalC must wait for the signaling process to leave the monitor |
| `signal` unblocks an arbitrary<br>blocked process                 | `signalC` unblocks the process at the head of the queue                                                                                |

## Signaling Discipline
- In a monitor, at most **one process can be active inside the monitor at a time** (implicit mutual exclusion). But `signalC` does something special: it makes a _blocked_ process `ready`. Now you have **two processes** that both want to run inside the monitor:
	- The **signaller**: the process that just executed `signalC` and is still in the middle of its procedure.
	- The **signalled process**: the one that was just woken up and was waiting on the condition variable.
- Only one can have the lock. **The signaling discipline is the rule that decides which of the two gets to run (and who must wait).**
- We define
	- `E` = processes blocked waiting to _enter_ the monitor (at the entry)
	- `W` = the signalled/waiting process
	- `S` = the signaller
- There are three main disciplines:
	- **Signal and Continue (non-preemptive)**
		- The **signaller keeps running** and finishes its procedure normally.
		- The signalled process is moved to ready but **executes at some later time** (it must compete for the lock again).
		- This is the simplest and most common (e.g., Java's `wait`/`notify`, POSIX condition variables).
		- **Precedence:** `E < W < S`
	- **Signal and Wait (preemptive)**:
		- The **signalled process runs immediately** (it "takes over" the monitor).
		- The **signaller is suspended** and must re-compete with other processes waiting to enter the monitor once the signalled process leaves (or waits again).
		- **Precedence:** `E = S < W`
	- **Signal and Urgent Wait (Immediate Resumption Requirement, IRR)**:
		- Like Signal and Wait, the **signalled process runs immediately**, but the **signaller has priority** over processes waiting to enter the monitor.
		- **Precedence:** `E < S < W`
- The last one is the **classic solution for monitors** (Hoare's original semantics). It preserves the "as-if" reasoning: the condition that was signaled is still true when the signalled process resumes, because the signaller cannot proceed until the signalled process finishes/wait again.
- Consequences of choosing one instead of another:
	- With **Signal and Continue**: between the `signalC` and the signalled process actually resuming, other processes can enter and change the monitor state — so the condition may no longer hold. Therefore the signalled process must **re-check the condition in a loop**: `while (!B) then waitC(cond)`.
	- With **Signal and Wait** and **Signal and Urgent Wait**: the signalled process resumes immediately, with no interleaving, so the condition is guaranteed to still hold; a single `if (!B) then waitC(cond)` is sufficient.
## Problem Resolved by using Monitors
- Monitors can be used to implement any resource or data structure which is used concurrently by multiple processes and in which we want to encapsulate the synchronization policies
- Revisiting the main examples:
	- **Producers–Consumers**: implementing the bounded-buffer as a monitor
	- **Readers-and-Writers**: implementing the rw-lock as a monitor
	- **Resource allocation and management**: implementing the resource allocator as a monitor
### Producer-Consumer with Bounded Buffer monitor
```
monitor BoundedBuffer {

    ElemType buffer := <EmptyBuffer>
    cond notFull, notEmpty;

    procedure put(ElemType elem) {
        if (buffer is full)
            waitC(notFull)
        append(buffer, elem)
        signalC(notEmpty)
    }

    procedure take(): ElemType {
        if (buffer is empty)
            waitC(notEmpty)
        ElemType el := head(buffer)
        signalC(notFull)
        return el
    }
}

Producer                              Consumer
loop                                  loop
    p1: ElemType el := produce            q1: ElemType el := BoundedBuffer.take()
    p2: BoundedBuffer.put(el)             q2: consume(el)
```


### Producers–Consumers (circular array implementation)
```
monitor BoundedBuffer {

    int[] elems := new int[MAX_ELEMS]
    int first := 0, last := 0
    cond notFull, notEmpty

    procedure put(int elem) {
        if (((last + 1) % MAX_ELEMS) = first)
            waitC(notFull)
        elems[last] = elem
        last := (last + 1) % MAX_ELEMS
        signalC(notEmpty)
    }

    procedure take(): int {
        if (first = last)
            waitC(notEmpty)
        int elem = elems[first]
        first = (first + 1) % MAX_ELEMS
        signalC(notFull)
        return elem
    }
}
```

### Readers-Writers with RWLock as a monitor

```
monitor RWLock {

    integer readers := 0
    integer writers := 0
    cond okToRead, okToWrite;

    procedure startRead() {
        if writers != 0
            waitC(okToRead)
        readers := readers + 1
        signalC(okToRead)
    }

    procedure endRead() {
        readers := readers - 1
        if readers = 0
            signalC(okToWrite)
    }

    procedure startWrite() {
        if writers != 0 or readers != 0
            waitC(okToWrite)
        writers := writers + 1
    }

    procedure endWrite() {
        writers := writers - 1
        if !emptyC(okToRead) then
            signalAllC(okToRead)
        else
            signalC(okToWrite)
    }
}

Reader                          Writer
p1: RWLock.startRead()          q1: RWLock.startWrite()
p2: read the dbase             q2: write the dbase
p3: RWLock.endRead()           q3: RWLock.endWrite()
```

### Resource Allocation
```
monitor ResourceAllocator {

    procedure request(<Params>) {
        < block the request until the resource or a resource is available,
          according to some policy >
    }

    procedure release(<Params>) {
        < possibly unblock some pending request >
    }
}
```
- Monitors are typically used to function as a resource allocator, enforcing some policy in allocating resources to processes requesting resource access.

### Monitor Implementation using Semaphores
A monitor can be realised using semaphores, in particular:
- **one semaphore `mutex`** for mutual exclusion
- for each condition variable, **a semaphore `condsem`** and a **counter `condcount`** keeping track of the number of processes suspended on the variable
- Signal and Continue semantics:
```
Prologue for each operation:
    wait(mutex)
Epilogue for each operation:
    signal(mutex)

waitC(cond) =
    condcount++;
    signal(mutex);
    wait(condsem);
    wait(mutex);

signalC(cond) =
    if (condcount > 0) {
        condcount--;
        signal(condsem)
    }
```
- the signaller does **not** yield after `signalC`, it continues (just decrements the count and signals `condsem`), then later releases `mutex` in the epilogue.
- Signal and Wait semantics:
```
Prologue for each operation:
    wait(mutex)
Epilogue for each operation:
    signal(mutex)

waitC(cond) =
    condcount++;
    signal(mutex);
    wait(condsem);
    wait(mutex);

signalC(cond) =
    if (condcount > 0) {
        condcount--;
        signal(condsem);
        wait(mutex);
    }
```
- after `signalC` wakes the signalled process, the signaller **waits on `mutex`** (`wait(mutex);` at the end of `signalC`), effectively handing the monitor over to the signalled process, which will re-acquire `mutex` at the end of its own `waitC`.
### Monitors in Java
- handling at low level synchronized block can't provide enough model checking
- Monitors can be implemented in Java by exploiting the two basic mechanisms:
	- `synchronized` blocks with builtin locks
	- set of mechanisms used for explicit synchronization among threads, through shared objects
```
public class SynchCell {
	private int value;
	private boolean available;
	
	public SynchCell() {
		available = false;
	}
	
	public synchronized void set(int v) {
		value = v;
		available = true;
		notifyAll();
	}
	
	public synchronized int get() {
		while(!available) {
			try {
				wait();
			} catch(InterruptedException ex) {}
		}
		return value;
	}
}
```