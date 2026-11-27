# Main tree
Concurrent programming:
- multi-threaded programming
- asynchronous programming -> task-based, asynchronous and reactive
- message-passing and actor-based models
# General Definition

"In computer science, concurrency is a property of systems in which several computational processes are executing at the same time, and potentially interacting with each other.
Concurrency is concerned with the fundamental aspects of systems of multiple, simultaneously active computing agents, that interact with one another."

Main aspects:
- systems with *multiple* *activities* or *processes* whose execution *overlaps in time*
- activities can have some kind of *dependencies*, therefore can *interact*

Re-assignment keyword:
- Process: the execution of a sequential program
# Concurrent, Parallel and Distributed programming differences

- **Concurrent**: Building programs in which multiple processes overlap in time and typically with each other in some ways, without necessarily running on separate physical processors. Focus at programming/abstract level on program organisation
- **Parallel**: the execution of programs overlaps in time by running on separate physical processors. Focus at physical level on performance
- **Distributed**: running multiple activities  with processors that are distributed over a network. There is no shared memory, no shared clock, independent failures



# Architectures designed for running multiple tasks
- Multi-core
- Cloud core
- Multi-core hybrid architectures
- heterogeneous core
- Parallel computers
- Clusters/Grid
- Cloud Computing

## Flynn's Taxonomy
- SISD
- SIMD
- MISD
- MIMD

 MIMD further classification:
 - MPP
 - Clusters
 - Grid


# Concurrency Taxonomy

Active components and passive components

State + Behaviour + Interaction




Reactive:
x = 1
y = x + 1
x = 2

-> y = 3 (Instantiating a dependency between y and x)









Touring machine


Designing reactive coffee machine
