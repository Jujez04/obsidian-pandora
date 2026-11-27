- Normally at the low level of the OS we have the *fetch-decode-execute* cycle, that at the same time has  *interrupt* **checks**
- The interrupt mechanism can be exploited to design high-level **event-based architectures**:
	- interrupts themself can be conceived as low-level events interrupting the control flow of the super-loop to execute the corresponding interrupt (event) handlers
	- interrupts make it possible to avoid polling in managing sensors and input
- Mainly there are two high-level
	- [[Observer pattern]]
	- asynchronous FSM

# Observer pattern in FSM
Parts
– Observer pattern library
• abstract class EventSource
– represents a generic base event source, generating
events using interrupts
• class InterruptDispatcher
– functions as a bridge between interrupt handlers and the
event source
– Event-based Tactile Button component
• Button interface
• AbstractButton abstract class
– base class for event-based button
• ButtonImpl implementation

Note this: adding listeners increases interrupts time

The listener codes is executed by interrupt handlers
– executing in interrupt context
– this could be a strong limitation, as discussed in previous
modules
• listeners cannot contain long-term computations or use
functions/procedures that need interrupts to be enabled
• To avoid race conditions, it is necessary to make the superloop code accessing variables modified by listeners atomic
– by disabling and re-enabling interrupts


# Asynchronous event-triggered FSMs
- As soon as there is an event in the **event queue** 
- there is a thread pool that works as an API


continuation pasting time -> pyramid of doom -> promises -> asynchronous engine


producer -- consumer
