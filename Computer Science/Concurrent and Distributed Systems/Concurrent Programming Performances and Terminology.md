---
last-change: 12/10/2026
"tags:": "[concurrency, concurrent-systems]"
---


# Performance
- **Throughput**: achieved through parallel hardware designed for programming
- **Responsiveness**: capability of interleaving CPU and I/O activities, in a way to optimize interrupt reactions
- **Speedup**: 
$$S = \frac{T_1}{T_N}$$
- Where:
	- $N$ is the number of processors
	- $T_1$ is the execution time of the sequential algorithm
	- $T_N$ is the execution time of the parallel algorithm with $N$ processors
- The maximum speed is $N$
- **Amdahl's Law:**
$$S= \frac{1}{1 - \mathcal{P} + \frac{\mathcal{P}}{N}}$$
- Where:
	- $\mathcal{P}$ is the proportion of a program that can be made parallel
	- $1-\mathcal{P}$ is the part that cannot be parallelized
- Linear speedup: $\mathcal{P} = 1$
- Super-linear speedup: $S > N$
![[Pasted image 20261012093014.png]]
- We can improve performances by adding processors only for those parts that can run in parallel, not for sequential parts
- Sequential parts are often needed for correctness
- **Efficiency**:
$$E = \frac{S}{N}$$
- Ideal efficiency: 1, where all processors are used at full capacity, but as in physics, it is lower than 1.
## Main targets
- Use the existing process resources more effectively as possible
- Enable our program to exploit additional processing resources if they become available
- These two objectives can be translated as: Keeping the CPUs as busy as possible (with useful work)

## Performance Testing
### Costs and Measurements
- Testing **throughput**:
	- Focus on avarage
	- “how long does it take to an operation or task to be completed on the average ?”
- testing **responsiveness**:
	- focus on variance
	- “what percentage of operations will succeed in under XX milliseconds?”
- There are also others: service time, latency, throughput, efficiency, scalability, capacity
- They can be classified in *how fast* the program can be executed or *how much* resources we could use to speed up the computation
- **Scalability**: describes the ability to improve throughput or capacity when additional computing resources are added
### Tradeoffs
- Nearly all engineering decisions involve some form of tradeoff
- avoid premature optimization rule: first make it right, then make it fast (if it is not already fast enough)
- In general don't trade safety for performance
### Profilers for measurement
- **Profilers** are tools that allow to evaluate the performance of program execution, monitoring thread state and use of the CPU and monitoring the time consumed by methods/procedures
- Examples for Java
	- VisualVM (free)
	- JProfiler
	- Java Mission Control
### Pitfalls
- Garbage Collection: Short test runs may never trigger a garbage collection cycle, masking high allocation rates and object churn. Alternatively, a single ill-timed stop-the-world pause in a 2-second test can skew the average latency or throughput drastically.
	- Run tests long enough for minor and full GC cycles to normalize across total execution time.
	- Monitor pause times and allocation rates explicitly (e.g., `-Xlog:gc*` in modern JVMs).
	- Separate throughput measurement from latency distribution (P 99, P 99.9).
- Dynamic Compilation: Modern runtimes interpret bytecode before compiling hotspots into native machine code (tiered compilation). Measuring execution during the interpreted or C1/Tier 3 stages reflects warmup overhead rather than peak steady-state performance.
	-  Implement distinct warmup phases where iterations run discarded, measuring only after steady-state execution is reached.
	- Use flags such as `-XX:+PrintCompilation` (or `-Xlog:compilation`) to verify that JIT tiering has settled and no deoptimizations are occurring mid-measurement.
- Unrealistic sampling of code paths: Overly simplified synthetic tests often execute only a single branch or concrete implementation. The JIT compiler optimizes monomorphic call sites with aggressive inlining. In production, however, bimorphic or megamorphic call sites force vtable lookups and prevent inlining.
	- Exercise realistic branch distributions and multiple concrete types if an interface is used polymorphically in production.
	- Ensure benchmark inputs do not become completely predictable unless that reflects the real-world domain.
- Unrealistic degree of contention:
	- **Under-contention**: Benchmarking concurrent data structures with a single thread or cleanly partitioned threads hides lock contention, thread suspension overhead, and false sharing.
	- **Over-contention**: Pounding a single shared lock with 64 threads on a tight loop creates artificial convoying that would never occur under balanced application logic.
	- Model the realistic read/write ratio and thread arrival rates.
	- Match worker thread counts to expected target hardware concurrency (accounting for CPU cores and NUMA node boundaries).
## Rule for the compute-intensive task
- *Thread pool ideal size:* related to types of tasks that will be submitted and characteristics of the deployment system, more specifically it should be dynamically bounded to the number of available processors
- *Empirical approach*: considering the number of processors, the available memory, task type (CPU or I/O), task resource utilization
- A system with $N_{CPU}$ processors usually achieves optimum utilization with a thread pool of $N_{CPU} + 1$ threads:
	- if you have $N_{\text{cpu}}$ cores and your workload is strictly compute-intensive (100% CPU bound, no explicit I/O or sleep calls), having exactly $N_{\text{cpu}}$ threads should theoretically maximize throughput while avoiding context-switching overhead.
	- The $(N_{\text{cpu}} + 1)$-th thread exists to prevent **involuntary hardware- and OS-level stalls** from idling a CPU core.
### What happens at OS level
If you configure exactly $N_{\text{cpu}}$ threads for $N_{\text{cpu}}$ cores:
- Core 1: Running Thread 1
- Core 2: Running Thread 2
- Core 3: Thread 3 hits a page fault $\rightarrow$ moves from **Runnable** to **Blocked/Waiting** state
- Core 4: Running Thread 4

Because there are no other runnable worker threads in the ready queue, **Core 3 goes idle**. The CPU cycle budget for that core during that stall window is wasted.

Adding a single spare thread creates a buffer without drowning the OS scheduler in context switches:
- Core 3: Thread 3 hits a page fault and transitions off the core.
- The OS scheduler immediately pulls **Thread 5** (the spare runnable thread) from the ready queue onto Core 3.
- Core 3 continues doing productive compute work instead of idling.

Every additional thread introduces overhead:
- **Context switching costs:** Saving/restoring register state, flushing pipelines, and scheduler scheduling lock overhead.
- **Cache pollution:** When Thread 5 replaces Thread 3, it evicts Thread 3's hot cache lines from L 1/L 2 caches. When Thread 3 wakes up and gets scheduled elsewhere, it suffers cold-cache penalties.
- **Memory footprint:** Each thread retains its own stack (typically 1 MB on 64-bit systems).

The $+1$ margin strikes the optimal balance for compute-heavy workloads: **just enough redundancy to fill involuntary CPU bubbles, without generating unnecessary context-switching churn.**

### What happens with I/O operations
- When tasks perform intentional, blocking I/O (database queries, HTTP calls, disk reads), threads spend a significant fraction of their time waiting.
- For mixed or I/O-bound tasks, Goetz defines pool sizing as:
$$N_{\text{threads}} = N_{\text{cpu}} \times U_{\text{cpu}} \times \left(1 + \frac{W}{C}\right)$$
- Where:
	- $N_{\text{cpu}}$: Number of available CPUs / hardware threads.
	- $U_{\text{cpu}}$: Target CPU utilization ($0 \le U_{\text{cpu}} \le 1$).
	- $\frac{W}{C}$: Ratio of **Wait time** (blocked on I/O) to **Compute time** (active processing).
- When a task is 99% compute and 1% wait, $\frac{W}{C} \approx 0$, which yields $N_{\text{cpu}}$ (adjusted to $N_{\text{cpu}} + 1$ for fault tolerance). When a task spends 80% of its time waiting on a remote database ($\frac{W}{C} = \frac{80}{20} = 4$), you would size the pool to $N_{\text{cpu}} \times (1 + 4) = 5 N_{\text{cpu}}$ threads.
## Multithreading Costs
- As discussed before, context switches implies overhead and the system suffers from overhead.
- `vmstat`: command in UNIX for measuing context switches operations.
- **Memory synchronisation**:
	- Distinguishing contended and uncontended synchronisation
	- Typically in VM uncontended synchronisation is fast
	- Instead contended synchronisation may require OS activity: scheduling, blocking control flows,...
- The principal threat to scalability in concurrent application is the **exclusive resource lock**, but it can be simplified by:
	- reducing the duration for which locks are held by narrowing lock scope
	- reduce the frequency with which locks are requested
	- replace exclusive locks with coordination mechanisms that permit greater concurrency
# Shared Memory problem
![[Pasted image 20261012094201.png]]
- Shared memory and bus as potential bottleneck as only one processor at a time can access main memory
- This is the reason why the cache system exists -> [[What is an Operating System]]

# Terminology
- Active component and passive components:
	- Active: assign tasks
	- Passive: 
- Actors: active and reactive logical entities
- Agents: active, reactive and proactive logical entities
- Processes: a sequential program in execution -> [[What is an Operating System]]
- Instead here a thread is a sequential program in execution
- **Task**: unit of work
- Non determinism
- *Process interaction*: any non trivial concurrent program is based on multiple processes that need to interact in some way
# Process Interaction
- Taxonomy of interactions:
	- Cooperation
	- Competition/Contention
	- Interference
## Cooperation
- Refers to interactions which are both expected and wanted
- Two types: 
	- *Communication*: concerns the need of realising an information flow among processes, typically realised in terms of messages. Needs specific supports for exchanging messages
	- *Synchronization*: concerns the explicit definition or presence of temporal relationships or dependencies among processes and among actions of distinct processes. Needs support for exchanging temporal signals.
- Synchronisation does not necessarily require any kind of shared data and the mutual exclusion

## Competition
- Refers to interactions which are expected and necessary, but not wanted
- Coordination for accessing the shared resource
- Class of problems:
	- **Mutual Exclusion**: ruling the access to shared resources by distinct processes. Typically require some forms of implicit synchronisation
	- **Critical Sections**: ruling the concurrent execution of blocks of actions by distinct processes
## Interference, Critical Situations and Race conditions
- Refers to interactions which are neither expected, nor wanted
- producing bad effects only when the ratio among the process speeds assumes specific values
- Heisenbugs
- **Race condition** or race hazard or simply race: whenever two or more processes concurrently access and update shared resources, and the result of the single update depends on the specific order occurring in process access
- Interferences and errors in concurrent programs can lead to critical situations for the concurrent system in the overall:
	- **Deadlock**: Situation wherein two or more competing actions (processes) are waiting for the other to finish, and thus neither ever does. Typically concerns the release of a locked shared resource, the reception of a temporal signal or a message. It concerns multiple processes
	- **Starvation**: Situation wherein a process is blocked in an infinite waiting and it is denied in accessing necessary resources. It concerns a single process 
	- **Livelock**: similar to a deadlock, except that the states of the processes involved in the livelock constantly change with regard to one another, none progressing
# Concurrent Machines
- To describe / specify a concurrent program we need concurrent programming languages
- To execute a concurrent program we need a concurrent machine
- A concurrent machine provides:
	- a support for the execution of concurrent programs and realizing then concurrent computations
	- as many virtual processors as the number of processes composing the concurrent computation
- Providing basic mechanisms for:
	- multiprogramming: virtual processors generation and management
	- synchronisation and communication
	- access control to resources
# Basic Mechanisms
- Multiprogramming: set of mechanisms that make it possible to create new virtual processors and allocate physical processors of the lower-level machine to the virtual processors by means of scheduling algorithms 
- Synchronisation and Communication 
	- two different typologies of mechanisms, related to two different architectural models for concurrent machines: 
		- shared memory model: presence of a shared memory among the virtual processors 
		- message passing model: every virtual processor has its own memory and no shared memory among processors is present. Every communication and interaction among processors is realized through message passing
# Design Approaches
- Three main design approaches
	- sequential language + library with concurrent primitives. e.g. C + PThreads
	- language designed for concurrency. e.g. OCCAM, ADA, Erlang, Go
	- hybrid approach: sequential paradigm extended with a native support for concurrency. e.g. Java, Scala
	- library and patterns based on basic mechanisms. e.g. java.util.concurrent


# Scenario
• Task-based frameworks
 - e.g. Java Executor framework
• Asynchronous Programming
 - event-loop based models
 - AsyncTasks everywhere
• Actors
 - Concurrent OOP
 - active objects variant
• Synchronous Message Passing
 - Processes + synchronous message passing
• Reactive Programming
 - e.g. Functional Reactive Programming, Reactive extensions (Rx).
• Parallel computing
 - GPGPU programming
 - Lambda Architecture / Parallelism for BigData