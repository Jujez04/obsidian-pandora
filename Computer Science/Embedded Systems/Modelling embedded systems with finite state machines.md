# Models for ES
- Embedded systems involves *discrete* and *continuous* systems 
- Generally in reality systems are modelled through mathematical models
- More specifically, other engineerings uses differential equations, while informatic engineering uses discrete models
- Discrete model involves programming parts of embedded systems and generally it shapes the behaviour of our system with **state machines**
- Example: parking garage
![[Pasted image 20251106091154.png]]
- We can see that *Counter* subsystem *reacts* to coming input *events* and produce and output event
- The *u* and *d* input signals can be modelled with a real function. e.i. u:$R$ -> {absent, present}
- **Pure** **signals**: At each time t, the signal u(t) is evaluated either as “absent" (no events in that moment), or “present”, meaning that there is an event in that moment
- The output signal *c* also can be represented with a real function: c:$R$ -> {absent} U N
- This is not a pure signal: it carries also an information content (besides possibly being “absent” like u and d)

## Reactions
- The dynamics of a discrete system can be described as a sequence of steps called **reactions** (because they react due to an event), being each reaction instantaneous (duration equals to zero) and **triggered** by the environment
- Some semantics:
	- *event-triggered reaction* -> reaction by input events

## Valuation
- The execution of a reaction lead to a **valuation** of the inputs and outputs
- a **variable** is associated to each input signal and the valuation of the input consists in assigning the value of the input signal at that time to the variable
- for each output signal we use a variable that is assigned to the value of the output function in that moment
## State
- the **state** of a system is a condition at a particular point in time and, in general, the state affects how the system reacts to inputs
- Formally, we define the state to be an encoding of everything about the past that has an effect on the system’s reaction to current or future inputs

# Finite state machines
- A **state machine** in general is a model of a system with discrete dynamics that at each reaction maps valuations of the inputs to valuations of the outputs, where the map may depend on its current state
- A **finite-state machine (FSM)** is a state machine where the set *States* of possible states is *finite*.
- Components:
	- *guards*:
	- *action*
- Variants of state machines
	- Milly 
	- UML defined (look for it)
![[Pasted image 20260122094403.png]]

## Transitions
- Transitions between states govern the discrete **dynamics** of the state machine and the mapping of input valuations to output valuations
- a transition may also start and end at the same state (es: State3) -> in this case, the transition is called a **self** **transition**.
- Transitions are characterised by a guard and an action 
	- the **guard** determines whether the transition may be taken on a reaction. 
		- it is represented by a predicate (a boolean-valued expression) that evaluates to true when the transition should be taken 
		- when a guard evaluates to true we say that the transition is enabled 
	- the **action** specifies what outputs are produced on each reaction 
		- it is an assignment of values (or absent) to the output ports 
		- any output port not mentioned in a transition that is taken is implicitly absent


## Asynchronous and synchronous 
- A FSM does not specify when valuation should be carried on to possible trigger the a reaction:
	-  -> **Asynchronous FSM**, also called *Event-triggered FSM* -> valuation occurs anytime there is an input event (generally modelled through *interrupts*) and the environment itself *establish* when the reactions will be evaluated and triggered 
	-  -> **Synchronous FSM**, also called *Time-triggered FSM* -> valuation occurs *periodically* with a **period** variable, establishing the work frequency
- HVAC example:
![[Pasted image 20251106093530.png]]
- This FSM can be conceived either as event-triggered — if the machine reacts each time the input temperature changes — or time-triggered, by evaluating (and reactive) at a regular pace
- This means that it's not important how we realize the general structure of a FSM, but how we can build the appropriate *model*

- When applying FSM to embedded systems, the input/output are then mapped variables that are used to define guards and actions
	- **input variables** -> modified by the **external** environment where the machine operates 
	- **output** **variables** -> modified by the machine by means of actions, to control the external environment
## Implementation
![[Pasted image 20260122100801.png]]
- Example:
![[Pasted image 20251106094231.png]]
```cpp
enum States { LightOn, LightOff } state

setup(){
 state = LightOff
}

step() {
 switch (state){
	 case LightOff:
		 if (button.isPressed()){
			 light.switchOn()
			 state = LightOn
		 }
	 case LightOn:
		 if (!button.isPressed()){
			 light.switchOff()
			 state = LightOff
		 }
 }
}

loop {
 step()
}
```

In a correct FSM model and implementation: – the computations related to actions should always terminate • there should be no infinite loop • in theory they should be instantaneous – the valuation of a condition should not change the state of variables

Recall -> [[State pattern]]

##  Extended FSM
- **Extended FSM** are FSM machines in which variables are flexibly used also to describe the state, to make the overall description more concise and effective
![[Pasted image 20251106094952.png]]

- Pedestrian traffic light example:
![[Pasted image 20251106095037.png]]
The total number of states in a EFSM can be computed by considering all possible admissible configurations of state variables $$|States| = n\times p^m$$
where:
- n = number of discrete states
- m = number of variables used to describe the state
- p = possible set of values that can be assigned to variables
## Mealy machines and Moore machines
- There is a conversion between them
- Mealy machines:
	- The output is the combination of present state and its inputs
	- During the clock cycle if input changes, also the output changes -> This will lead to inconsistency if not managed well
	- Output is not synchronized with the clock
	- It is reactive: it responds quite quickly to input changes
![[Pasted image 20260122095826.png]]
![[Pasted image 20260122100455.png]]
- Moore machines
	- The output is determined only the current state
	- During the clock cycle if input changes then the output changes at the active edge of the next clock cycle -> It is immune to input glitches during clock cycles
	- Output is synchronized with the clock
![[Pasted image 20260122095946.png]]
![[Pasted image 20260122100507.png]]
- the parking garage as a Moore machine:
![[Pasted image 20260122102019.png]]
## Time-triggered FSM
A Time-Triggered FSM is a state machine where valuations and reactions occur periodically at a fixed regular interval, known as a **period**.

- **Internal Clock:** The machine operates as if it has an internal clock; transitions and state updates happen only at the "clock tick".
    
- **Frequency:** The period determines the working frequency (or rate) of the machine.
    
- **Contrast with Asynchronous:** Unlike asynchronous (event-triggered) FSMs, which react immediately whenever an input event occurs, time-triggered FSMs wait for the next scheduled tick to process inputs.

Synchronous FSMs are specifically designed to handle **time-oriented behaviors** effectively.

- **Time Management:** They ease the specification of behaviors involving deadlines, delays, or periodic actions (e.g., blinking an LED every 500 ms).
    
- **Handling Multiple Timings:** If a system has different timing requirements (e.g., 500 ms on, 750 ms off), the machine's period is typically set to the greatest common divisor (GCD) of those times (e.g., 250 ms
### Timers
Implementation typically relies on **programmable timers** and a control loop architecture.
```cpp
volatile int timerFlag = 0;
void timerISR(){
 timerFlag = 1;
}
/* procedure implementing the step of the state machine */
void step(){…}
loop(){
 while (!timerFlag){}; /* wait for a tick for doing the next step */
 timerFlag = 0;
 step();
}
```
- **Timers:** A timer is programmed to generate an interrupt at the desired frequency.
    
- **The Loop:** The code usually involves a main loop that waits for a timer flag (the "tick") before executing the machine's logic.
    
- **The `step()` Function:** The core logic is encapsulated in a `step()` function, which checks the current state and inputs to determine if a transition should occur

- Example:
```cpp
#include "Led.h"
#include "Timer.h"
#define LED_PIN 13
Light* led;
Timer timer;
enum { ON, OFF} state;
void step(){
 switch (state){
 case OFF:
 led->switchOn();
 state = ON;
 break;
 case ON:
 led->switchOff();
 state = OFF;
 break;
 }
}

void setup(){
 led = new Led(LED_PIN);
 state = OFF;
 timer.setupPeriod(500);
}
void loop(){
 timer.waitForNextTick();
 step();
};
```

- I would like that the behaviour structure is independent on FSM we choose for its implementation
- There are more **robust** methods -> see lab 12 nov

## Input sampling
The most critical design decision for a Time-Triggered FSM is choosing the **Sampling Rate** (where Sampling Rate = $1 / period$). This choice involves several trade-offs:

#### A. Power vs. Reactivity

- **High Frequency (Small Period):** Results in higher reactivity and lower latency but increases microcontroller usage and power consumption13.
    
- **Low Frequency (Large Period):** Saves power but increases the risk of missing short-duration events14141414.
    
-> jitter in [[Task-based Architectures]]
#### B. Guaranteeing Detection (MEST)

To ensure the system functions correctly without losing data, you must consider the **Minimum Event Separation Time (MEST)**.

- **Definition:** MEST is the smallest time interval possible between two input events in the environment. *...in a sync FSM, by choosing a period less than MEST, we have the guarantee that all events will be detected, i.e. no events will be lost*
    
- **The Rule:** By choosing a period **less than the MEST**, you guarantee that all events will be detected. (but it has to be large enough to guarantee that no resources will be wasted and also the computation that has been triggered is executed, otherwise it will to **overrun** **exception**)

#### C. Latency

- **Definition:** Latency is the time interval between an input event occurring and the system generating the corresponding output.
    
- **Relationship:** Smaller periods lead to smaller latencies, which is crucial for user perception (e.g., a button press should respond within 50 ms to feel "instant").

#### D. Input Conditioning (Debouncing)

Time-triggered FSMs can help filter "bouncing" (mechanical signal noise in switches) via software.

- **Debouncing Strategy:** By selecting a sampling period that is larger than the bouncing period (e.g., sampling at 50 ms for a switch that bounces for 20 ms), the machine ignores the spurious intermediate signals19.

```cpp
#include "Led.h"
#include "Timer.h"
#define LED_PIN 13
#define BUTTON_PIN 2
Light* led;
Button* button;
Timer timer;
enum { ON, OFF} state;
void setup(){
 led = new Led(LED_PIN);
 button = new ButtonImpl(BUTTON_PIN);
 state = OFF;
 timer.setupPeriod(50);
}
void loop(){
 timer.waitForNextTick();
 step();
};

void step(){
 switch (state){
 case OFF:
 if (button->isPressed()){
 led->switchOn();
 state = ON;
 }
 break;
 case ON:
 if (!button->isPressed()){
 led->switchOff();
 state = OFF;
 }
 break;
 }
}
```


[IES] Introduction to Embedded Systems. Lee, Sheshia. – capitolo 3 • [PES] Frank Vahid, Tony Vargis, Bailey Miller. Programming Embedded Systems: An introduction to Time-Oriented Programming. University of California, Riverside. • [GOF] Erich Gamma; Richard Helm, Ralph Johnson, John M. Vlissides (1995). Design Patterns: Elements of Reusable Object-Oriented Software. Addison-Wesley