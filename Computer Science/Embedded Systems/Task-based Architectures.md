To handle the complexity of embedded software, the system behavior is decomposed into a set of concurrent tasks.

- **Definition:** Each task represents a specific, well-defined unit of work or job.
    
- **Behavior:** The internal behavior of each task is described by a Finite State Machine (FSM).
    
- **Global Behavior:** The overall system behavior emerges from the execution and interaction of these concurrent FSMs.
    
- **Benefits:** This decomposition offers modularity, easier debugging, reduced complexity for individual components, and higher reusability

- The `Task` class is designed to track its own timing using variables like `period` and `timeElapsed` to determine if it is time to execute during a specific scheduler cycle.

The architecture is implemented using an abstract base class named `Task`.

- **Interface:** This class defines two primary virtual methods:
    
    - `init()`: To initialize the task (called once).
        
    - `tick()`: Equivalent to the "step" method in an FSM, encapsulating the behavior and called periodically.
        
- **Concrete Implementation:** Specific tasks (e.g., `BlinkTask`, `ThreeLedsTask`) extend this base class.
    
- **Time Management:** The `Task` class is designed to track its own timing using variables like `myPeriod` and `timeElapsed` to determine if it is time to execute during a specific scheduler cycle.

- Decompose in multiple tasks -> concurrency
- The behaviour of the embedded software is decomposed into a set of concurrent tasks – each task represents a specific well-defined confided unit of work/ job to be done – the behaviour of each task can be described by a FSM – the global behaviour is the result of the execution and interaction of the concurrent FSMs
- i.e. Led show -> one led blinking and three of them turn on and off with some period -> we can model it with a single task or two of them![[Pasted image 20251113092536.png]]
## Task Decomposition
- **Modularity**: 
	- task are **indipendent** among each other
	- the “interface” of the module in this case is the set of input/output variables / objects that the task is using
	- reduce complexity, easier debugging and reusability
- --> **Concurrency**
	- tasks are concurrent and their execution overlap in time
	- each of them has its own logical control flow
	- interaction and coordination mechanisms for **dependencies**
- i. e. Led show with task based architecture
- ![[Pasted image 20251113093447.png]]
```cpp
#ifndef __TASK__
#define __TASK__
class Task {
public:
 virtual void init() = 0;
 virtual void tick() = 0;

};
#endif

// Blinktask.h
#ifndef __BLINKTASK__
#define __BLINKTASK__
#include "Task.h"
#include "Led.h"
class BlinkTask: public Task {
 int pin;
 Light* led;
 enum { ON, OFF} state;
public:
 BlinkTask(int pin);
 void init();
 void tick();
};
#endif

// BLinktask.cpp
#include "BlinkTask.h"
BlinkTask::BlinkTask(int pin){
 this->pin = pin;
}

void BlinkTask::init(){
 led = new Led(pin);
 state = OFF;
}

void BlinkTask::tick(){
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
 
 // ThreeLeds.h
 #ifndef __THREELEDSTASK__
#define __THREELEDSTASK__
#include "Task.h"
#include "Led.h"
class ThreeLedsTask: public Task {
 int pin[3];
 Light* led[3];
 int state;
public:
 ThreeLedsTask(int pin0, int pin1,
 int pin2);
 void init();
 void tick();
};
#endif

 // ThreeLeds.cpp
 #include "ThreeLedsTask.h"
ThreeLedsTask::ThreeLedsTask(int pin0, int pin1,
 int pin2){
 this->pin[0] = pin0;
 this->pin[1] = pin1;
 this->pin[2] = pin2;
}

void ThreeLedsTask::init(){
 for (int i = 0; i < 3; i++){
 led[i] = new Led(pin[i]);
 }
 state = 0;
}

void ThreeLedsTask::tick(){
 led[state]->switchOff();
 state = (state + 1) % 3;
 led[state]->switchOn();
}

//main loop
#include "Timer.h"
#include "BlinkTask.h"
#include "ThreeLedsTask.h"
Timer timer;
BlinkTask blinkTask(2);
ThreeLedsTask threeLedsTask(3,4,5);
void setup(){
 blinkTask.init();
 threeLedsTask.init();
 timer.setupPeriod(500);
}
void loop(){
 timer.waitForNextTick();
 blinkTask.tick();
 threeLedsTask.tick();
}
```

## Task scheduler
- To execute tasks with different periods, a simple multi-tasking **cooperative scheduler** is implemented.

- **Strategy:** It uses a cooperative round-robin strategy.
    
- **Base Period:** The scheduler operates on a base period $p$, which is the greatest common divisor of all task periods.
    
- **Execution:** At every period $p$, the scheduler iterates through the list of tasks and advances their execution by calling the `tick()` method.
    
- **Sleep Mode:** If the scheduler period is large enough, the system can enter sleep mode between ticks to save power.
- ![[Pasted image 20251113094030.png]]
- i.e. cooperative round-robin strategy:
	- the scheduler has a period p equals to the greatest common divisor among the periods of the tasks 
	- at each period p, it advances the execution of each tasks by calling its method tick • internally, it uses a timer to realise the periodic behaviour
	- Check vs
	- the execution of the tick method should have a duration always less than the scheduler period
	- Also there isn't any **race conditions** that can occur, and this is related to the fact that one task is executed at a time.
- A different way to use timers and interrupts in schedulers concerns implementing the **schedule step** (schedule, tick) directly inside the interrupt/timer handler -> **input-driven schedulers**
- The major drawback in this case is involving *interrupt handlers* (and it has its limitations)
- i.e. Motion-Triggered Lamp System -> an embedded system in which a led is switched on as soon as some movement is detected, in some environment
- ![[Pasted image 20251113100425.png]]
- Action are specified inside the state, not in the transition
- If they are implemented in a concurrent way with **preemptive scheduler** we should consider *race conditions*
### Shared variables, Atomic actions and race conditions
Tasks are concurrent and may have dependencies (temporal or data-oriented) that require interaction.

- **Mechanism:** These dependencies are typically managed through shared variables.
    
- **Race Conditions:** Because the scheduler is cooperative and executes task ticks sequentially, individual ticks are atomic, preventing low-level race conditions within a single step.
    
- **Logic Risks:** However, high-level race conditions can still occur because multiple ticks of one task are interleaved with ticks of others


Task can have dependencies that requires various form of interactions
- **temporal dependencies**: e.g.: a task T3 can be executed only after tasks T1 and T2  
- **producers/consumers**: e.g. a task T1 needs an info produced by a task T2 
- **data-oriented**: e.g. task T1 and T2 need to share some data 
-In our case we represent and manage these dependencies through **shared variables**
## Sleep mode
- If the working period of the scheduler is large enough, we can exploit time-driven behaviour and when we can activate the **sleep mode** for save energy and power.
- For example: if the scheduler has executed every single task in its list, we can turn on the sleep mode until the next tick!
- That also means that the period of our scheduler should be large enough to make irrelevant the overhead caused by the sleeping mode of our MCU board

# Execution Analysis and Constraints

Unlike abstract FSMs, real tasks take time to execute. Analysis is required to prevent timing failures.

- **Overrun Exception:** An overrun occurs if the execution time of actions exceeds the allotted period. It is called a timer overrun if the scheduler is timer-based using interrupts (interrupt-driven schedulers) ![[Pasted image 20260124091202.png]]
    
- **WCET:** The Worst-Case Execution Time (WCET) is the maximum time a task takes to execute in a period, calculated by analyzing the longest sequence of assembly instructions.
    
- **CPU Utilization ($U$):** This is the percentage of time the CPU is used. If $U > 100\%$, an overrun will occur.- $$U = \frac{\text{Time used for a task}}{\text{Total time}} \times 100\%$$
	-  If $U > 100\%$, the system is mathematically guaranteed to fail timing requirements, leading to an **Overrun Exception**.
    ![[Pasted image 20260124091232.png]]
	![[Pasted image 20260124091253.png]]
- **Hyper-Period:** When tasks have different periods, analysis is done over the "hyper-period" (the minimum common multiplier of all task periods).
    ![[Pasted image 20260124091334.png]]

# Advanced Timing (Jitter, Deadlines, and Priorities)

To manage strict timing requirements, the architecture must consider scheduling delays.

- **Jitter:** The delay between when a task is ready to execute and when it actually begins. Different scheduling strategies may lead to different jitters. In general, giving priority to shortest tasks leads to minimise the average jitter
    
- **Deadline:** The specific time interval within which a task _must_ complete to avoid system failure. If a task is not executed within its deadline, we have a missed-deadline exception that can result in a system failure. By default the interval is its period.
    ![[Pasted image 20260124091647.png]]
- **Priority Scheduling:** To minimize jitter or meet deadlines for critical tasks, priorities can be assigned (statically or dynamically).
    
    - giving priority to shortest tasks generally minimizes average jitter.
        
- **Aperiodic Tasks:** The system may also need to handle sporadic tasks (executed at arbitrary times) via dynamic insertion or active/inactive states. Basic capabilities of a scheduler managing aperiodic tasks:
	- dynamic insertion and removal of tasks
	- dynamic assignments of priorities
# Overrun exception
- when the execution time of actions exceeds the period
- Can we detect it only at runtime? Yes, with WCET but No, we can detect it during code analysis (assembly lines):
![[Pasted image 20251113101803.png]]
- There are some API provided by the compiler that checks it
- How to counter this problem? 
