- First of all there is a difference between testing, verification and validation:
	- **Testing**: looking for failures (that is different from *fault*)
	- **Verification**: checking if the system satisfies its specification
	- **Validation**: checking if the system satisfies customer's expectation

# Testing
- Typically there are different kind of testing:
	- *Safety*: verify that a class or system behaviour conforms to its specification, usually taking the form of testing invariants with *assertions*
	- *Liveness*: progress / non-progress tests in which we check that the system is able to make progress and not get stuck in a deadlock situation
- Problems and limits:
	- typically checking only a subset of the scenarios
	- *heisenbugs* phenomenon: test code can introduce timing or synchronisation artefacts that can mask bugs
- state explosion: different scenarios can occur at any time in the same moment
# Verification
- Two principal formal techniques:
	- **model checking**:
		- where verification is done by generating one by one all the states of the systems and by checking the properties state by state
		- can be automated by model checkers tools
	- **inductive proofs of invariants** (more mathematical):
		- invariant properties are proved by induction over the states of the system
		- can be automated by tools called deductive systems
- Both techniques rely on some kind of formal language / calculus to specify correctness properties
## Model Checking
- used for concurrent system
- Strategy based on exhaustively searching the entire state space of a system and verify if certain **properties** are satisfied
- properties are intended as predicates on a system state or states, expressed as a logical specification such as propositional temporal logic formula, that produces:
	- confirmation response or
	- trace (counterexample)
- Pentium bug (1994) and Mars Polar Lander incident (1999)
## State-space Explosion Problem
- The big problem of model-checking technique is the **size of the state space**, which is a graph with thousands or millions of states.
- There are different techniques that optimize the state exploration:
	- Applying heuristic rules to reduce the number of states
	- Incremental construction of the whole graph
	- Symbolic model checking, working with set of states
## Safety and Liveness Property
- A property is an attribute of a system that is true for every possible execution trace/scenario
- Properties of interest for concurrent programs fall into two categories:
	- A *safety property* asserts some condition that should be always (in every possible state) verified
	- A *liveness property* asserts that some condition will *eventually* be true in the future (in at least one state)
# Specifying Property in FSP
- Two main approaches
	- Ad hoc support:
		- property processes
		- progress properties
	- A more general support:
		- Fluents and Linear Temporal Logic
		- logical description, more general and flexible
## Safety Property in FSP
- Safety properties are specified in FSP by *property processes*
- They are composed with a target system to ensure that the specified property holds for that system
- Example:
```
property POLITE = (knock->enter->POLITE)
```
![[Pasted image 20261004113858.png]]
- A safety property P defines a deterministic process that asserts that any trace including actions in the alphabet of P, is accepted by P
- Thus, if P is composed with S, then traces of actions that are in the alphabet of S and the alphabet of P, must also be valid traces of P, otherwise ERROR is reachable
- Then we can say that a safety property P is satisfied by a system S, if and only if the composition of P with S does not reach an ERROR state
- Constrain to consider when describing the property process: it must be deterministic
```
property MUTEX = (enter -> exit -> MUTEX).
// System components
USER1 = (enter -> exit -> USER1).
USER2 = (enter -> exit -> USER2).

// Flawed composition allowing concurrent access
SYS_FLAWED = (USER1 || USER2).

// Verified composite system
||CHECK_SYS = (SYS_FLAWED || MUTEX).
```
## Progress(Liveness) Property in FSP
- A liveness property asserts that something good eventually happens
- The progress property is a specific important case of liveness, in which whatever state a system is in, it is always the case that a specified action will eventually be executed
- It's the opposite of starvation
```
progress P = {a1,a2..an}
```
- the property asserts that in an infinite execution of a target system, at least one of the actions a1,a2..an will be executed infinitely often
```
COIN = (toss->heads->COIN | toss->tails->COIN).
progress HEADS = {heads}
progress TAILS = {tails}
```
- Progress violation:
```
TWOCOIN = (pick -> COIN | pick -> TRICK).
COIN = (toss->heads->COIN | toss->tails->COIN).
TRICK = (toss->heads->TRICK).
progress HEAD = {heads}
progress TAILS = {tails}
```
![[Pasted image 20261004121318.png]]
- In this case TWOCOIN doesn't satisfy the property TAILS, due to the fact that a TRICK coin exists. The TAILS property detect the violation of the progress.
- Instead the property `HEADSorTAILS = {heads,tails}` is satisfied by TWOCOIN, since at least one of the two actions will be executed infinitely often
# Temporal Logic and Logical Description
- *logical description*: expressions composed out of propositional variables and logical connectives, that can be used to describe properties of a system
- In this case we want to introduce *temporal logic*, in a way in which propositions can be true for every possible execution of a program.
- The temporal logic is a formal logic obtained by adding temporal operators to propositional or predicate logic
	- **Linear Temporal Logic (LTL)**: to express properties that must be true (at a state) for every possible scenario
	- **Branching Temporal Logic**: to express properties that must be true in some or all scenarios starting from a state
## LTL
- LTL is a modal temporal logic with modalities referring to time. It is used to describe sequences of states in a system.
- LTL is based on two basic temporal operators:
	- always (denoted by $\square$) - a property is true in every state of the system or the formula □ p is true in a state s of a computation if and only if the formula p is true in all state following s. It can be used for specifying safety properties.
	- eventually (denoted by $\diamond$) - a property is true in some state of the system or the formula ◇ p is true in a state s of a computation if and only if the formula p is true in some state following s. It can be used for specifying liveness properties.
- Properties:
	- Reflexivity: $\square p \rightarrow p$ and  $p \rightarrow \diamond p$
	- Duality: $\neg \diamond p \equiv \square \neg p$ and $\neg \square p \equiv \diamond \neg p$
	- Idempotence: $\square \square p \equiv \square p$ and $\diamond \diamond p \equiv \diamond p$
	- Sequence: $\diamond \square p \not \equiv \square \diamond p$
	- Distributivity:
        - $\square (p \wedge q ) \equiv \square p \wedge \square q$
        - $\diamond (p \vee q ) \equiv \diamond p \vee \diamond q$
	- Theorems:
		- $(◇ □ p ∧ ◇ □ q ) → ◇ □ (p ∧ q )$  is true
		- $(□ ◇ p ∧ □ ◇ q ) → □ ◇ (p ∧ q )$ is false
- Eventually(always p) = in a certain moment of time p will become always true
- always(eventually p) = p can become true in different moments of time
- $\square (req \rightarrow \diamond reply)$
## Fluent
- A fluent is a (logical) condition that can changes over time
- Construct: `fluent FL = <{s1,...sn}{e1..en}> initially B`
- Meaning: FL becomes true when any of the initiating actions `{s1,...sn}` occur and false when any of the terminating actions `{e1..en}` occur. If the term initially B is omitted then FL is initially false
- Example: The following fragment defines a fluent LIGHT which becomes true when the action on occurs and false when the action off or `power_cut` occurs
```
const False = 0
const True = 1
fluent LIGHT = <{on}, {off, power_cut}> initially False

## Indexed fluents
fluent LIGHT[i:1..2] = <{on[i]}, {off[i], power_cut}>
```
### Action Fluents
- An action fluent is a fluent that is true only when a specific action occurs
```
// A fluent that becomes true when a door opens, and false when it closes
fluent DoorOpen = <open, close> initially False

// An LTL property asserting the door cannot stay open forever
assert DOOR_SAFETY = []<>(!DoorOpen)
```
- A standard fluent is defined as:
$$\text{fluent } F = \langle I_F, T_F \rangle$$
- where $I_F$ turns it true, and $T_F$ turns it false.
- For an action fluent for action $a$:
	- **Initiating set ($I$):** $\{a\}$ — firing $a$ sets it to `true`.
	- **Terminating set ($T$):** $(\alpha \cup \{\tau\}) \setminus \{a\}$ — literally _any other action in the system's alphabet_, plus the internal silent action $\tau$.

### Fluent expressions
- Fluent expressions are logical combinations of fluents, allowing for complex conditions to be represented.
- Fluent expressions can be formed using the following logical operators:
	- Negation: `!F` (not fluent F)
    - Conjunction: `F1 && F2` (both fluents F1 and F2 are true)
    - Disjunction: `F1 || F2` (at least one of the fluents F1 or F2 is true)
    - Implication: `F1 -> F2` (if fluent F1 is true, then fluent F2 must also be true)
    - Equivalence: `F1 <-> F2` (fluent F1 is true if and only if fluent F2 is true)
- Example:
```
fluent POWER = <power_on, power_off>
fluent LIGHT = <on, off>
LIGHT->POWER
```
- This expression states that the light can only be on if the power is on. If the power is off, the light cannot be on.
- `forall` and `exists` quantifiers can be used to express properties over sets of fluents. For example:
```
# all lights are on
forall i:1..N (LIGHT[i] -> POWER[i])

# at least one light must be on
exists[i:1..2] LIGHT[i]
```
- Combined with temporal operators they are used to express safety and liveness properties:
```
# eventually all lights are on
forall i:1..N []<>(LIGHT[i])
```
## Critical section
- A critical section is a part of the program that accesses shared resources and must not be concurrently executed by more than one thread of execution.
- The critical section problem is to design a protocol that the processes can use to cooperate in order to ensure that only one process at a time is executing in its critical section.
- **Mutual Exclusion**: If process Pi is executing in its critical section, then no other processes can be executing in their critical sections. 
```
# property violated
USER = (ncs -> enterCS -> cs -> exitCS -> USER).
||USERS = (a:USER || b:USER).

# introducing fluents
fluent USER_A_IN_CS = <a.enter, a.exit>
fluent USER_B_IN_CS = <b.enter, b.exit>

MUTEX = □!(USER_A_IN_CS && USER_B_IN_CS)

assert MUTEX = []!(USER_A_IN_CS && USER_B_IN_CS)

# Solving with a lock system
USER = (ncs -> acquire->enterCS -> cs -> exitCS -> release->USER).
LOCK = (acquire -> release -> LOCK).
||USERS =
(a:USER || b:USER || {a,b}::LOCK).
fluent USER_A_IN_CS = < a.enterCS, a.exitCS >
fluent USER_B_IN_CS = < b.enterCS, b.exitCS >
assert MUTEX = []!(USER_A_IN_CS && USER_B_IN_CS)
```
- **No Starvation**: If a process requests to enter its critical section, then it will eventually be granted access. 
```
# property violated
USER = (ncs -> acquire->enterCS -> cs -> exitCS -> USER). # never releasing it
LOCK = (acquire -> release -> LOCK).
||USERS =
(a:USER || b:USER || {a,b}::LOCK).
||USERS = (a:USER || b:USER).
assert NO_STARV =[](a.acquire -> <> a.enterCS)
```
### Until and Weak Until Operators
- The **until** operator (denoted as `U`) is used to express that a certain condition must hold until another condition becomes true.
```
assert MUTEX_U =
[]((a.enterCS -> (!b.enterCS U a.exitCS)) &&
(b.enterCS -> (!a.enterCS U b.exitCS)))
```
- The **weak until** operator (denoted as `W`) is similar to the until operator, but it does not require the second condition to eventually become true; the first condition can hold indefinitely.
```
assert POLITE_W = (!enter W knock)
```
- Equivalences:
```
<> p = True U p
[] p = !<>!p
p W q = [] p || (p U q)
```
### Rigid Expressions
- A rigid expression is a logical expression that maintains its truth value across all states of a system. It is used to express properties that are invariant throughout the execution of a program.
- Thus we can define:
```
assert True = rigid(1)
assert False = rigid(0)
```