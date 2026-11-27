# Overview
- Step #1: Analysis (identify concurrency)
- Step #2: Design (choose concurrent architecture)
- Note the recurring theme:
	- analysis is about *finding* concurrency,
	- design is about *structuring* it with agents + shared resources.

# The Architectural Discipline (Active vs. Passive)
- Strong partition: agents (active, threads of control, no public interface)
  vs. passive components (monitors/shared resources)
- No hybrid entities, hybrids are decomposed into active + passive parts
- Why: every interaction is mediated by shared passive components
# Step #1: Analysis
- Done in two steps
- Identifying **Task**: 
	- sequence of instructions that operate together as a group, corresponding to a logical part of an algorithm or a program
	- a specific piece of work required to be done as a duty or responsibility of some agent
## Decomposition
- **Task decomposition pattern** (divide-and-conquer; data decomposition follows tasks)
	- *dividi-et-impera*: he problem can be naturally decomposed into a collection of independent or nearly independent tasks
	- in this case, data can be decomposed given the task decomposition
	- e.g. producer/consumer systems
- **Data decomposition pattern** (decompose data first; tasks follow)
	- Problem's data can be easily decomposed into units
	- first data units are identified, then task related to that units can be identified
	- in this case, task decomposition follows data decomposition
  - e.g. matrix multiplication; scales with #processors
## Dependency Analysis
- Goal: group and order tasks according to the type of dependencies, in order to maximize performance
- Kinds of dependencies: 
	- **temporal**
	- **data/resource**

# Step #2: Design -> Conceptual Classes [CAR-89]
- Three "ways of thinking" about the problem (focus differs)
## Result Parallelism
- Organize around the *shape of the result* (data structure)
- Focused on data-decomposition
- Each agent produces one piece, working in parallel; shared structures wrap the result, encapsulating mutex and synchronization issues
- Building-a-house example (build all components simultaneously)
	- Proceed by building all the components simultaneously
	- Assembling them as they are completed
	- Use separate agents (workers) set to work on the different parts
	- proceeding in parallel up to the point where work on one component can’t proceed until another is finished
## Specialist Parallelism
- Organize around a *crew of specialists* in a logical network
- Each agent is assigned to perform one specified kind of task
- Coordination media (communication protocols, shared data structures) are used to support specialists communication and coordination (message boxes, blackboards, event services)
- Building-a-house example (surveyors, excavators, carpenters...)
	- Assemble a constructor crew in which each skill is represented by a separated specialist worker
	- once the project is well underway, however, many skills (hence many workers) will be called to play simultaneously
- Often pipelined; strong role of coordination
## Agenda Parallelism
- Organize around a *workflow/agenda of tasks*, in which activities are executed in parallel as many as possible; many workers per step
- Each agent is assigned to help out with the current item on the agenda
- Shared data structures are designed to manage data consumed and produced by agenda activities
- Generalist team that works stage-by-stage following the agenda
- Building-a-house example (work team of generalists, each capable of performing any construction step)
## Remarks & Method
- Boundaries are not rigid, approaches can be mixed & applied recursively
- e.g. a specialist approach might make secondary use of agenda parallelism, for example, by assigning a team of workers to some speciality
- However the three approaches represent three clearly separate ways of thinking about the problem
	- result parallelism focuses on the shape of the finished product
	- specialist parallelism focuses on the makeup of the work crew
	- agenda parallelism focuses on the list of tasks to be performed
- Basic steps [CAR-89]:
  1. Choose the most natural conceptual class
  2. Write the program naturally for that class
  3. If inefficient, transform methodically into a more efficient version

# Step #2: Design -> Concrete Concurrent Architectures [MAG-99][PAT-06]
- Some specific architectures can help to bridge the gap between the conceptual class and the implementation
## Master-Workers (a.k.a. Manager-Workers)
- The architecture is composed by a master agent and a possibly dynamic set of worker agents, interacting through a proper coordination medium functioning as a bag of tasks
- **Master agent**
	- decomposes the global task in subtasks
	- assigns the subtasks by inserting their description inside the bag
	- collects task results
- **Workers agent**
	- get the task to do from the bag
	- execute the tasks
	- communicate the results
	- variant: a proper monitor data structures can be used to aggregate results
- **Bag of tasks resource** -> typically implemented as a blackboard or a bounded buffer
- This architecture is typically implemented in *async programming*, in which the execution of task occurs *asynchronously* with respect to the control flow that created and submitted the task itself.
- Example: **Java Executor framework**
	- a task is submitted to an executor by means of an execute or submit method, which returns immediately a *future*
	- the task is asynchronously executed by one of the executor threads, according to the executor policy
### Task-oriented frameworks
- **Task** *concept*:
	- Domain level -> abstract, discrete, independent unit of work
	- Uncoupled from the notion of *thread*
- Two roles: 
	- **Master/Task submitter** *submits* the task 
	- **Worker thread** *executes* the task
- the task is executed **asynchronously** with respect to the control flow of the task submitter
- How can master collect the task results?
	- Bag of results
	- Specific designed collector/aggregator monitor
	- **Future mechanism**
		- by issuing an asynchronous computation (task), an object representing the future result or the state of the computation itself is created and immediately returned
		- The object could provide operation for checking (*poll*) the task state, blocking the task result, catching errors/exception related to the task
		- Blocking behaviour is still allowed in normal computation, except in *event-driven programming*
- In Java, this logical view is represented by *virtual threads* recently introduced (as a preview) in JDK 19
	- Java Enhancement Proposal (JEP) 425 https://openjdk.org/jeps/425
	- a main result of the project Loom  https://openjdk.org/projects/loom/
- Simplify concurrent programming by introducing an API for **structured concurrency**
	- treats groups of related tasks running in different threads as a single unit of work
	- streamlining error handling and cancellation
	- improving reliability, and enhancing debugging
![[Pasted image 20261108190801.png]]
### Fork-Join Variants
- Master-worker applied recursively
	- workers are masters, producing tasks and collecting results
	- effective approach to implement a concurrent divide-and-conquer strategies
- Examples: adaptive quadrature, merge-sort
## Filter-Pipeline
- Linear chain: generator → filter(s) → sink, via pipes/bounded buffers
	- **Generator agent**: the agent starting the chain, generating the data to be processed by the pipeline
	- **Filter agent**: an intermediate agent of the chain, consuming input information from a pipe and producing information into another pipe
	- **Sink agent**: the agent terminating the chain, collecting the results
- Example: image processing
## Shared-Space / Blackboard
- the architecture is composed by a set of agents communicating and indirectly interacting through a shared space, as a common resource functioning as a blackboard-like coordination media
- the space-based coordination media provides operations to add new information items and to query/retrieve/remove them, eventually including some synchronisation feature
- Features: data-driven coordination, temporal uncoupling
-  \[GEL-85]\[GEL-92]
## Announcer-Listeners
- the architecture is composed by an announcer agent and a dynamic set of listener agents, interacting through an event-service
	- **Announcer agent**: announce the occurrence of events on the event service
	- **Listener agent**: depending on what kind of event arrives on the event-service, it reacts with the proper behaviour to that particular event.
	- **Event-service resource**: uncouple announcer listener interaction and its main objective is to collect and dispatch events
- Example: parallelizing event-driven apps (parallel event handlers)

# Revisiting MVC for Concurrent Systems
## Mapping the roles
- Controller → agents (active, control logic)
- Model → shared passive components (mutex, coordination media)
- View → shared passive components (user interaction, reads model state)
## Issues
- GUI toolkit choice: single active component manages non-thread-safe GUI
- MVC uses event-based (observer) interaction -> original observer is
  synchronous with control coupling
- Rethinking *observer pattern* for concurrency: producer/consumer architecture
  (bounded buffer = event queue; producers generate, consumer reacts)


# Lab

## Java Executor Framework
- Provides direct support to task decomposition in terms of threads
- Defines in terms of domain level topics how the system should behave
- The executor *isn't an active component*, it is part of the system
## Virtual Threads


## GUI


- To be reactive, don't assign task to the EDT
- Producer-consumer for commands

## Bibliography
• [CAR-89] Nicholas Carriero and David Gelernter, "How to write
parallel programs: a guide to the perplexed", ACM Computing
Surveys (CSUR). Volume 21, Issue 3 (1989)
• [GEL-92] David Gelernter and Nicholas Carriero. 1992. Coordination
languages and their significance. Commun. ACM 35, 2 (Feb. 1992),
97–107. DOI:https://doi.org/10.1145/129630.129635
• [GEL-85] David Gelernter. 1985. Generative communication in
Linda. ACM Trans. Program. Lang. Syst. 7, 1 (Jan. 1985), 80–112.
DOI:https://doi.org/10.1145/2363.2433
• [MAT-05] T. Mattson, B. Sanders, B. Massingill, "Patterns for Parallel
Programming", Addison Wesley (2005)
• [MAG-99] Jeff Magee and Jeff Kramer, "Concurrency - State Models
and Java Programs", Wiley (http://www.doc.ic.ac.uk/~jnm/book/)
• [MAL-93] Thomas W. Malone, Kevin Crowston. "The interdisciplinary
study of coordination". Center for Coordination Science, Alfred P.
Sloan School of Management, Massachusetts Institute of
Technology, 1993


