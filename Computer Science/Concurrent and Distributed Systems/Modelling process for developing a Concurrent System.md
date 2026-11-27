- Main targets of modelling:
	- Describe the behaviour of a concurrent system
	- Support the design process of a concurrent system
	- Analyse the behaviour
- There are tools based on formal theories that allows to verify the correctness of a concurrent system.
- There are mainly two kind of approaches:
	- **Labelled Transition Systems**
		- based on *process algebra*
		- models concurrency using *interleaving*
	- **Petri Nets**
		- not interleaving approach
# LTS
- LTS are *finite state machines* described by the means of diagrams and well-defined properties
- There is a textual notation, called *Finite State Processes* (FSP), that allows to describe concurrent processes 
- LTSA - Labelled Transition System Analyser:
	- specifying a concurrent system in FSP
	- creating the corresponding LTS
	- analysing properties
## Atomic Actions
- **Process**: execution of a sequential program
- The process state evolves through the program execution by the evaluation of statements. 
- Each statement consists of one or more **atomic** **actions**, primitive instructions that changes process **state**
- Each action causes what is called a **transition** from the current state to the next state
- The order in which actions are allowed to occur is determined by a **transition** **graph**, an abstract representation of the program
### Light switch example

![[Pasted image 20260922162102.png]]
- Two states - state 0 and state 1, and actions on and off
- on causes a transition from state 0 to state 1 and off causes a transition from state 1 to state 0
- the symbol 0 and 1 to identify the states is just a convention here
- the initial state is always numbered 0 and transitions are always drawn in a clockwise direction
- Although this representation of a process is finite, the behavior described need not to be finite
- for example, the state machine of the light switch allows the following sequence of actions: on → off → on → off → on → off → …



## Labeled Transition System (LTS)
- A *transition system* is a pair $(S, T)$ where:
	- $S$ is a set of states
	- $T$ is a subset of $S \times S$
	- A transition from state $p$ to state $q$ if $(p,q)\in T$ and we denote it $p\rightarrow q$
- A *labelled transition system* is a tuple $(S,\Lambda,T)$ where:
	-  $S$ is a set of states
	- $\Lambda$ is a set of *labels*
	-  $T$ is a subset of $S \times \Lambda \times S$
	- We say that there is a transition from state $p$ to state $q$ with label α if $(p,α,q) ∈ T$ and denote it $p \rightarrow^\alpha q$
- Labels can represent different things depending on the language used

## Finite State Process (FSP)
- **FSP** is a algebraic notation to describe process models
- every FSP description has a corresponding state machine (LTS) description
- Main operators
	- **action** prefix: (a->P) where a is an action and P is a process
	- **choice**: (a->P|b->Q)
	- **parallel** composition (P||Q)
	- **interaction** through shared actions
- A process can be described in terms of other processes, by means of operators: 
	- `Q = (x->P)`
- Recursive process: `P = (x->P)`
- Multiple processes: `P = (x->Q), Q = (y->P).`
- Example:
```
SWITCH = OFF,
OFF = (on->ON),
ON = (off->OFF).
// Equivalent description (achieved by substitution)
SWITCH = (on->off->SWITCH).
```
- STOP is a predefined process with a specific (predefined) behaviour (i.e. stopping). It can defined by using the keyword `STOP`
- The alphabet of a process is the set of actions in which it can engage
	- Ex: `SWITCH = {on, off}`
	- A process may only engage in the actions in its alphabet 
		- for example, a process that writes to a store location may potentially write any 32-bit value that location; however, it will usually write a more restricted set of values
- Given a state, there could be possible multiple transitions, depending on events or internal state. This is given in FSP by the use of the Choice construct `(x->P|y->Q)`
- after the first action has occurred, the subsequent behaviour is described by P if the first action was x and Q if the first action was y
- Ex:
```
DRINKS = (red->coffee->DRINKS
		 |blue->tea->DRINKS
		 ).
```
- Non deterministic choice: the same action could lead to different processes in which the FSP can be. 
- Non deterministic example:
```
COIN = (toss->heads->COIN
		|toss->tails->COIN
		).
```
- In FSP both local processes and action labels may be indexed, modelling processes and actions that can take multiple values
- Ex: `BUFF = (in[i:0..3]->out[i]->BUFF).`
- Range:
```
range T = 0..3

BUFF = (in[i:T]->STORE[i]),
STORE[i:T] = (out[i]->BUFF).
```
- Constants:
```
const N = 3
BUFF = (in[i:0..N]->out[i]->BUFF).
```
- Guarded actions:
```
COUNT (N=3) = COUNT[0],
COUNT[i:0..N] = (when(i<N) inc->COUNT[i+1]
				|when(i>0) dec->COUNT[i-1]
				). 

Trace:
think->talk->scratch
think->scratch->talk
scratch->think->talk
```
- Parallel composition:
```
ITCH = (scratch->STOP).
CONVERSE = (think->talk->STOP).
||CONVERSE_ITCH = (ITCH || CONVERSE).
```
- **Primitive** processes are defined using action prefix and choice
- **Composite** processes (beginning with ||, e.g. ||CONVERSE_ITCH) are defined using only parallel composition
- Characteristics of parallel composition:
	- Commutative: `(P||Q) = (Q||P)`
	- Associative: `(P||(Q||R)) = ((P||Q)||R) = (P||Q||R)`

# Interleaving
- How can we model the concept of speed in a concurrent environment?
	- This could depend on the number of processors, scheduling strategies, OS,...
-  we choose not to model relative speed but state simply that processes execute at **arbitrary relative speeds** (a process can take an arbitrarily long time to proceed from one action to the next)
- Abstraction from execution time:
	- The disadvantage is that we cannot say anything about real-time execution
	- The advantage is that we can verify *properties*, satisfying the concept of **portability**
- A classic approach to model concurrency, abstracting from time, is using **interleaving**:
	- The execution of concurrent processes each represented by a sequence of actions is modelled by a single sequence of actions (a **scenario** or **trace**) obtained by interleaving the actions of each process
- **non-determinism**: due to interleaving, we can have different (not finite) possible scenarios resulting from the execution of concurrent processes.
- Since processes are running at arbitrary relative speeds, then the order is not well defined and different processes are *arbitrary interleaved*
- Arbitrary interleaving turns out to be a good model of concurrent execution since it abstracts the way processors switch between processes as a result of external interrupts
## Interactions
- Interaction among processes is modelled through **shared actions**, that is processes having alphabets with actions in common
```
BILL = (play -> meet -> STOP).
BEN = (work -> meet -> STOP).
||BILL_BEN = (BILL || BEN).
```
- **Interaction rule**: while unshared actions may be arbitrarily interleaved, a shared action must be executed *at the same time by all the processes* that participate in that shared action.
![[Pasted image 20261003105812.png]]
- Another example:
```
MAKER = (make->ready->MAKER).
USER = (ready->use->USER).
||MAKER_USER = (MAKER || USER).
```
![[Pasted image 20261003110708.png]]
- Remark: the first item must be made by the MAKER process
## Handshake
- An action which is *acknowledged by another*
- For example:
```
MAKERv2 = (make->ready->used->MAKERv2).
USERv2 = (ready->use->used->USERv2).
||MAKER_USERv2 = (MAKERv2 || USERv2).
```
- The MAKER cannot proceed if the item it created isn't used by the USER.
## Multi-party synchronisation
- Many processes can engage in a shared action
```
MAKE_A = (makeA->ready->used->MAKE_A).
MAKE_B = (makeB->ready->used->MAKE_B).
ASSEMBLE = (ready->assemble->used->ASSEMBLE).
||FACTORY = (MAKE_A || MAKE_B || ASSEMBLE).
```
- assembly cannot take place until both parts are ready
- makers are not permitted to get ahead of users
## Composite process
- Since a parallel composition of processes is itself a process, called a composite process, it can be used in the definition of further compositions.
```
MAKE_A = (makeA->ready->used->MAKE_A).
MAKE_B = (makeB->ready->used->MAKE_B).
||MAKERS = (MAKE_A || MAKE_B).
ASSEMBLE = (ready->assemble->used->ASSEMBLE).
||FACTORY = (MAKERS || ASSEMBLE).
```
## Process Labeling
- When the composite process is composed by the same process, we must ensure that the primitive processes have disjoint labels:
- We can the *process labelling construct*: `a:P`, prefixes each action label in the alphabet of P with the label a
```
SWITCH = (on->off->SWITCH).
||TWO_SWITCH = (a:SWITCH || b:SWITCH).
```
- Also they can be parametrized by the use of `forall` construct: `forall [i:1..N] P(i)`
```
||SWITCHES(N=3) = (forall[i:1..N] s[i]:SWITCH).
||SWITCHES(N=3) = (s[i:1..N]:SWITCH).
```
- Labelling can be done using set of prefix labels: `{a1,..,ax}::P`
```
RESOURCE = (acquire->release->RESOURCE).
USER = (acquire->use->release->USER).
||RESOURCE_SHARE = (a:USER || b:USER || {a,b}::RESOURCE).
```
## Relabelling
- Individual processes can be designed with their own proper alphabet.
- When creating composite processes composing independent processes we may need to create a correspondence among the actions that should synchronise by means of *relabelling*
- General form: `/{newlabel_1/oldlabel_1,...newlabel_n/oldlabel_n}`
```
CLIENT = (call->wait->continue->CLIENT).
SERVER = (request->service->reply->SERVER).
||CLIENT_SERVER = (CLIENT || SERVER) /{call/request, reply/wait}.
```
![[Pasted image 20261003112400.png]]
## Hiding and Silent Actions
- Sometimes it is useful to make process actions “private”, that is: not to be used for interacting with other processes
- In FSP (and process algebra) this can be done by means of a *hiding operator*: `\{a1..ax}`
- Actions combined with the hiding operator are called *silent actions* and they are represented with the symbol $\tau$
- *exposing operator*: `@{a1..ax}`
- When applied to a process P, the interface operator `@{a1..ax}` hides all actions in the alphabet of P not labeled in the set a1..ax.
```
USER = (acquire->use->release->USER)\{use}.
USER = (acquire->use->release->USER)@{acquire,release}.
```
![[Pasted image 20261003113413.png]]
- **Minimisation**: Hiding is essential in reducing the complexity of large systems for analysis purposes since, minimising the size of state machines after removing tau actions
## Sequential Composition
- In FSP processes are divided in three categories:
	- *local processes*: define a state within a primitive process
	- *primitive processes*: defined by a set of local processes
	- *composite processes*: composed primitive processes that interacts parallel composition, relabelling and hiding
- There are local processes that have special names, like STOP, ERROR and END
- A *sequential process* is a process that can *terminate* its execution, or in other words the local process END is reachable from its start state, means that the process is successfully terminated
- **Sequential composition**:if P is a sequential process and Q is a local process, then `P;Q` represents the sequential composition such that when P terminates, `P;Q` becomes the process Q
- A sequential composition in FSP always takes the form: `SP1;SP2;..SPn;LP` where `SP1,..,SPn` are sequential processes and `LP` is a local process
- A sequential composition can appear anywhere in the definition of a primitive process that a local process reference can appear
- Sequential composition can be used in a recursive context with the *if construct*: `P = if <cond> then Q else R`
- Example:
```
const N = 3
R(I=0) = if (I<N) then
			(a[I]->R(I+1);END)
		else
			END.
```
- The parallel composition `SP1||SP2` of two sequential processes SP1 and SP2 terminates when both of these processes terminate.
```
P = (x->END).
||S = (a:P || b:P).
```
# Petri Nets
- Abstract, formal model of information flow, used for describing and analysing the flow of information in asynchronous and concurrent systems
- Introduced by Carl Adam Petri ~ 1965
- Used for modelling systems of events in which it is possible for some events to occur concurrently but there are constraints on the concurrence, precedence, or frequency of these occurrence
- Takes the form of bipartite graph:
	- Two type of nodes
		- *Places* (circles) and *transitions*(bars)
	- Connected by directed edges
![[Pasted image 20261003121815.png]]
- the execution of a Petri net is controlled by the position and movement of markers called **tokens** in the net:
![[Pasted image 20261003122308.png]]
- Tokens are indicated by black dots, residing in places
- A Petri Net with tokens is called *marked Petri Net*
- Tokens are moved by the **firing** of transitions of the net
	- a transition must be enabled in order to fire
	- a transition is enabled when all of its input places have a token in them
	- the transition fires by removing the enabling tokens from their input places and generating new tokens which are deposited in the output place of the transition
![[Pasted image 20261003123141.png]]
- The distribution of tokens in a marked Petri Net defines the state of the net and is called *marking*
- *Different transitions may fire, with different result markings* (inherent non-determinism)
![[Pasted image 20261003123400.png]]

## Transitions
- *Source transition*:
	- without any input place
	- just produce tokens
	- ![[Pasted image 20261003123603.png]]
- *Sink transition*:
	- without any output place
	- just consume tokens
	- ![[Pasted image 20261003123628.png]]
## Weighted edges
- a transition `t` is enabled if each input place `p` of t is marked with at least `w(p,t)` tokens, where `w(p,t)` is the weight of the edge from `p` to `t`
- a firing of an enabled transition t removes `w(p,t)` tokens from each input place `p` of `t`, and adds `w1` tokens to each output place `p` of `t`, where `w(p,t)` is the weight of the edge from `t` to `p`
![[Pasted image 20261003124838.png]]
- Petri nets can be used to model quite naturally concurrent systems in terms of *events*, *conditions* and relationships among them
- Firing of a transition in the context of concurrent systems means the occurrence of an event, implying an atomic change of the system 
![[Pasted image 20261003125319.png]]
![[Pasted image 20261003125330.png]]
- There are some interpretations for places and transitions:
![[Pasted image 20261003125425.png]]
- Also Petri Net are useful for synchronisation:
![[Pasted image 20261003125506.png]]

## Conflict and Concurrent events
- **Conflict events:** two events e1 and e2 are in conflict if either e1 or e2 can occur but not both
- **Concurrent events:** two events e1 and e2 are concurrent if they can occur independently of each other
- *Confusion*: A situation where conflict and concurrency are mixed
	- Ex:
![[Pasted image 20261004103124.png]]
## Locality and Atomicity
- In Petri Nets there is no measure of time or a proper ordering of events
- **Locality**: the occurrence of an event is determined by the availability of resources (tokens) in its input places, independently of the occurrence of other events
- The occurrence of (primitive) events is instantaneous
	- non primitive events (with a duration) must be modelled by multiple events
	- e.g.: activity with a beginning and an ending event
- Petri nets are not considered interleaved because their underlying semantics is fundamentally based on **true concurrency** (or partial-order semantics) rather than total sequential ordering:
	- **Interleaving Semantics (e.g., LTS, process calculi like CCS):** Concurrency is simulated by sequencing. If two actions $a$ and $b$ are concurrent and independent, the system models this as a nondeterministic choice between $a$ followed by $b$, or $b$ followed by $a$
	- **True Concurrency / Step Semantics (Petri Nets):** Concurrency is an explicit structural primitive. If two transitions $t_1$ and $t_2$ have disjoint input conditions (they do not compete for the same tokens), they can fire **simultaneously in a single step**
## Hierarchies
- Petri Nets can be structured in a hierarchical manner, allowing for the decomposition of complex systems into simpler subnets:
	- **Abstraction**: an entire net may be replaced by a single place or transition for modelling at a more abstract level
	- **Refinement**: a single place or transition may be replaced by a more detailed subnet for modelling at a more detailed level
![[Pasted image 20261004104154.png]]
## Application to Concurrent Design and Programming
- PN can be naturally used to model software systems, concurrent software systems in particular
- Representing problems
	- critical sections and mutual exclusion problems
	- synchronisation
- Representing mechanisms behaviour
	- semaphores
	- synchronisers
- Representing entire problems
	- Producers / Consumers
	- Readers-Writers
	- Dining Philosophers
## Extensions
- Zero-testing extension
	- extending the basic PN with the possibility of firing a transition only if a certain place has zero tokens
	- *inhibitor edge* represented by an arc with a small circle at the end
![[Pasted image 20261004104330.png]]
- **Timed Nets**
	- introducing time delays associated with transitions and/or places
	- deterministic time delays: a transition fires after a fixed time delay
	- stochastic time delays: a transition fires after a random time delay, following a given probability distribution
- **High-level Nets**
	- allowing tokens to carry data values and transitions to perform operations on these values
	- Coloured Petri Nets: tokens are distinguished by colors, representing different data values or types
## Tools
- Many tools available for creating and analyzing Petri Nets http://www.informatik.uni-hamburg.de/TGI/PetriNets/
- Examples:
	- PIPE 2
		- Platform Independent Petri net Editor 2
		- Java-based, open-source: http://pipe2.sourceforge.net/
	- TINA
		- Time Petri Nets analyzer
		- https://projects.laas.fr/tina/index.php