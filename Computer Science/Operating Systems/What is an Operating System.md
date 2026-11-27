
# What is an operating system?
- resource allocator
- control program for managing the execution
- environment variables
- kernel -> program running continuously at all times on the computer, sometimes it is running alongside two other categories of programs:
	- *system programs*: associated with the operating system but they are not necessarily part of the kernel
	- *application programs*: programs not associated with the operating system.
- middleware for mobile operating systems: a set of framework that work a level above then the core kernel


# Computer system organization
- Generally the CPU is linked with other devices through the **system bus**, that provides access between components and shared memory
- More practically the CPU is linked with one or more **device controllers**, special hardware that interact with **device drivers** of external devices. Device drivers are software parts that provides an API for controllers to interact with the device in which they belong.
![[Pasted image 20260915131532.png]]
## Computer operations (interrupts)
- Start with the **bootstrap program**:
	-  stored in ROM / EEPROM / firmware
	- contains instructions for compiling the kernel
- then loaded into the main memory and provides services, some of them are provided outside by programs that are outside the kernel, called the **system daemons**
- init function process: boot the entire operative system and waits for event to occur
- The occurrence of events is usually signaled by **interrupts**.
- Hardware sends to CPU a signal (the interrupt) that signals the occurrence of an event via system bus
- Software interrupts instead are called *trap* and are discussed later on.
- When the CPU is interrupted, it stops the execution of the current instruction, stores it in a special register and immediately transfers execution to a fixed location, the starting address of the **interrupt service routine**. Once the CPU has finished the execution, it resumes the interrupted computation.
- The interrupt must transfer control to the appropriate *interrupt service routine*, special programs that are stored inside the **interrupt vector table**, in the "*low memory*" zone
- Measures for computational units:
	- 1 bit -> base
	- 1 byte -> 8 bit
	- 1 word -> computer architecture's native data, mainly one or more bytes (i.e. 8 bytes for 64-bit architectures)
- Once the ISR starts to be executed, then CPU can't receive any other interrupts.
- The operations(or more generally the program) that was executed before *must be stored* in a special storage.
- Each computer has its own interrupt mechanism, but several functions are common.
- The basic mechanism of interrupts has something more than a simple table of pointers:
	- When a controller *raises* an interrupt in the **interrupt-request line**, a wire that the CPU senses, and the CPU *catches* it.
	- The it reads the interrupt number and executes lookup operation in the interrupt vector table and it starts looking a list of pointers. -> this feature is provided with the **interrupt-controller hardware** and CPU working together and they are designed in a way that:
		- defer interrupt handling during critical processing
		- dispatch the proper interrupt handler
		- achieve *multileveling* and *priority*
	- Most CPU has two interrupt-request lines:
		- *nonmaskable* line (0-31) -> reserved for important events, such as unrecoverable memory errors, can't be turned off 
		- *maskable* line (32-255)-> can be turned off before the execution of special instructions, usually used for device-generated interrupts.
	- Once the interrupt index is located in the table, then it start the **interrupt chaining**, that is the list of pointers that addresses the location of ISRs of that type. This implementation comes from the fact that computers can have multiple devices that are connected and then they need to address more interrupt handlers.
	- Each element of the list is inspected one by one, until is found the one that can fulfill the request.
- This implementation was achieved for making a compromise between overhead of a huge interrupt table and the inefficiency of dispatching to a single interrupt handler.
![[Pasted image 20260915131611.png]]
## Storage structures
- There are two functions that are mainly used for handling memory storages: 
	- *load* for loading content in the **instruction register**
	- *store* for writing the data inside a memory storage
- General-purpose computers implement these operations with a DRAM as the main memory
- **Main memory** is the term for describing where all operations are executing. Generally there are two characteristics to be noted:
	- Main memory is usually too small to store all needed programs and data permanently
	- Main memory is implemented as a *volatile* memory, then once we have turned off our computer all the data inside are gone
- Then most computers must provide a *secondary storage*, or typically one or more that could be:
	- Magnetic disks
	- Solid-state disk (SSD)
	- Optical disks
	- Magnetic tapes
- There are several terminologies:
	-  Volatile storage will be referred to simply as memory. If we need to emphasize a particular type of storage device (for example, a register),we will do so explicitly.
	- Nonvolatile storage retains its contents when power is lost. It will be referred to as NVS. The vast majority of the time we spend on NVS will be on secondary storage. This type of storage can be classified into two distinct types:
		- Mechanical. A few examples of such storage systems are HDDs, optical disks, holographic storage, and magnetic tape. If we need to emphasize a particular type of mechanical storage device (for example, magnetic tape), we will do so explicitly.
		- Electrical. A few examples of such storage systems are flash memory, FRAM, NRAM, and SSD. Electrical storage will be referred to as NVM. If we need to emphasize a particular type of electrical storage device (for example, SSD), we will do so explicitly.
- Mechanical storage is generally larger and less expensive per byte than electrical storage. Conversely, electrical storage is typically costly, smaller, and faster than mechanical storage.
![[Pasted image 20260915131631.png]]
## I/O structure
- Interrupt-driven I/O system can work when there are a few data to be moved from one to another place in the PC, but it is pretty inefficient when we have to move tons of data.
- For this reason, a system called **DMA**(*Direct Memory Access*) allow to move large amount of data.
- After setting up buffers, pointers and counters, the device controller transfer an entire block of data directly to (or from) the device and main memory, with *no intervention* by the CPU.
![[Pasted image 20260915131702.png]]
# Typical computer system architectures
They are categorized according to the number of general-purpose processors involved:
## Single-Processor Systems
- A single processor system containing one CPU with a single core.
- A **core** is the component that executes instructions and registers for storing data locally
- These systems have other special-purpose processors, running limited instruction sets and do not run processes. They are handled by the operating system.
- The operating system receives in turn some information about these processors and their next tasks. This arrangement relieves the main CPU of the overhead of disk scheduling

## Multi-Processor Systems
- Systems with two or more general-purpose processors with a single-core CPU, sharing computer bus, clocks, memory and peripherical devices
- More processors means more throughput and more overhead for operations in keeping everything working correctly
- **SMP**(*symmetric multiprocessing*) -> each peer CPU performs all tasks, including operating system operations. (All kinds of tasks are executed by each CPU)
![[Pasted image 20260915131733.png]]
- This definition has evolved over time and now this category contains also **multicore** systems.
- These systems are ways faster, cheaper and more efficient thanks to *on-chip communication*
![[Pasted image 20260915131747.png]]
- As a drawback, also *cache coherency* must be achieved by each core.
- **NUMA**(*non-uniform memory access*) -> provide each CPU with its own local memory that is accessed via a small, fast local bus. A potential drawback might be the latency when a CPU must access remote memory across the bus.
![[Pasted image 20260915131802.png]]
- **Blade servers** -> multiple processor boards, I/O boards and networking boards are in the same chassis.

## Clustered Systems
- Systems that gather together multiple CPUs in the form of *individual systems*, called **nodes**
- Such systems are *loosely coupled* and each node could be a multicore system.
- They are designed to achieve:
	- High availability (for services)
	- Graceful degradation ( continue providing service proportional to the level of surviving hardware )
	- Fault tolerance (fallback mechanisms)
	- High performance computing
	- Parallelization
- *Asymmetric clustering* -> one machine is in *hot-standby mode* while the other is running the application
- *Symmetric clustering* -> every machine is running the application and at the same time monitoring itself and the others.
- **Distributed lock manager (DLM)** -> a lock mechanism to provide shared access, blocking conflicting operations
- **Storage-area Network(SAN)** -> for managing clusters that aren't physically in the same place/zone

# Operating-System Operations and Modes
- When the computer is booting, the bootstrap program must locate the operating-system kernel and load it into the main memory.
- Once the kernel is loaded and executed, it can start providing **services** to the system and its users. Some services are provided outside the kernel by system programs that are loaded into memory at boot time to become **system daemons**
- Events are signaled by the occurrence of an interrupt, more specifically if it comes from software they are called **trap** or **exception**, caused either by an error or from a specific request from a user program that an operating system service be performed by executing a special operation called a **system call**.
- 
## Multiprogramming and Multitasking
- A functional request of the operating system is that users typically want to run more than one program at a time. -> **Multiprogramming**
- The operating system keeps **several processes** in memory **simultaneously**.  The operating system picks and begins to execute one of these processes. Eventually, the process may have to wait for some task, such as an I/O operation, to complete.
- In a non-multiprogrammed system, the CPU would sit idle. 
- In a multiprogrammed system, the operating system simply switches to, and executes, another process. When that process needs to wait, the CPU switches to another process, and so on. The CPU is never idle.
- **Multitasking** is a logical extension of multiprogramming. In multitasking systems, the CPU executes multiple processes by switching among them, but the switches occur frequently, providing the user with a fast response time.
- Having several processes in memory at the same time requires some form of memory management and for this reason the response is **CPU scheduling**.
- Running multiple processes concurrently requires that their **ability to affect one another** **be limited in all phases of the operating system**, including process scheduling, disk storage, and memory management. 
- In a multitasking system, the operating system must ensure reasonable **response time**, achieved by the common method of the **virtual memory**, that allows the execution of a process that is not completely in memory. 
	- The main advantage is that it enables users to run programs that are larger than actual *physical memory*
	- Provides an abstraction that transform the main memory into a uniform array of storage, separating *logical memory* viewed by the users from physical memory.
## Dual-Mode and Multi-Mode Operations

### Dual-Mode Operations: Kernel and User mode 

![[Pasted image 20260915160835.png]]
- A properly designed operating system must ensure that malicious program cannot cause other program to execute incorrectly.
- This comes with necessity to separate two modes of operation: **user mode** and **kernel mode** (or **supervisor mode**).
- Computer systems are designed in a way that hardware this separation.
- There is a bit called **mode bit** that is added to the hardware of the computer to indicate the current mode: kernel (0) or user (1). With the mode bit we can distinguish tasks that are executed on behalf of the operating system from the ones on behalf of the user.
-  When the computer system is executing on behalf of a user application, the system is in user mode.
- when a user application requests a service from the operating system (via a system call), the system must transition from user to kernel mode to fulfill the request.
- At system boot time, the hardware starts in kernel mode.
- The instructions executed while the operating system is in kernel mode are called **privileged instructions**.

### Multi-Mode and VMM
![[Pasted image 20260915164010.png]]
- The concept of dual-mode can be extended beyond two modes.
- Modern processors have 4 or more **protection rings**, where:
	- Ring 0 is the kernel
	- Ring 1 and 2 belongs to the operating system services (rarely used)
	- Ring 3 belongs to applications and users.
- CPUs that support virtualization have a separate mode to indicate when the **virtual machine manager (VMM)** is in control of the system. In addition it has more privileges than user processes but fewer than the kernel.

### Advantages of Mode structure
- System calls provide the means for a user program to ask the operating system to perform tasks reserved for the operating system on the user program’s behalf.
- A system call usually takes the form of a *trap* to a specific location in the interrupt vector. More specifically:
	1. Control passes through the interrupt vector to a *service routine* in the operating system, and the mode bit is set to kernel mode.
	2. The *kernel* examines the *interrupting* *instruction* to determine what system call has occurred; a parameter indicates what type of service the user program is requesting. Additional information needed for the request may be passed in registers, on the stack, or in memory 
	3.  The kernel verifies that the *parameters* are correct and legal, *executes* the *request*, and *returns* *control* to the instruction following the system call
- Once hardware protection is in place, it detects errors that violate modes. These *errors* are normally *handled* by the operating system in *kernel mode*.
- When a program error occurs, the operating system must terminate the program *abnormally*. This situation is handled by the same code as a user-requested abnormal termination. An appropriate error message is given, and the *memory* of the program may be *dumped*. The *memory dump* is usually written to a file so that the user or programmer can examine it and perhaps correct it and restart the program.
## Timer
- We must ensure that the operating system maintains control over the CPU and we cannot allow a user program to get stuck in an infinite loop.
- For this reason, a *timer*, called **watchdog timer**, is used to interrupt the CPU after a specified period.
- If the timer interrupts, control transfers automatically to the operating system, which may treat the interrupt as a fatal error or may give the program more time.
- On Linux systems there is a variable `HZ` that specifies the frequency of timer interrupts. The value depends on the kernel configuration.
# Resource Management
- The OS is a resource manager, handling CPU's memory space, file-storage space, I/O devices and so on.
## Process Management
- **Process**: a program in execution, that is a unit of work within the system. It needs resources, including CPU time, memory, files and I/O devices, to accomplish its task.
- **Program**: a passive entity, a set of instructions stored on disk (or other storage media).
- A single-threaded process has one **program counter**, specifying the next instruction to execute. Therefore the execution of a program must be sequential
- A multi-threaded process may be associated with the same program and has several program counters, one for each thread. Therefore the execution of a program may be *concurrent* (parallel, concurrent and distributed differences in [[What is a Concurrent System]]).
- The operating system is responsible for the following activities in connection with process management:
	- Creating and deleting both user and system processes
	- Scheduling processes and threads on the CPUs
	- Suspending and resuming processes
	- Providing mechanisms for process synchronization
	- Providing mechanisms for process communication

## Memory Management
- Main memory is a large array of bytes, ranging in size from hundreds of thousands to billions. Each byte has its own address.
- The CPU reads instructions from main memory during the instruction-fetch cycle and both reads and writes data from main memory during the data-fetch cycle (on a von Neumann architecture).
-  In the same way, instructions must be in memory for the CPU to execute them.
- For a program to be executed, it must be mapped to absolute addresses and loaded into memory. As the program executes, it accesses program instructions and data from memory by generating these absolute addresses. 
- For the fact that general-purpose computers must support the execution of multiple processes concurrently, the operating system must have a *memory management algorithm*, and this is strongly related to the hardware support.
- Responsibilities of the memory management system:
	- Keeping track of which parts of memory are currently being used and by whom
	- Deciding which processes (or parts thereof) and data to move into and out of memory
    - Allocating and deallocating memory space as needed
## File-System Management
- Operating systems provides a uniform logical view of information storage called **file system** where each logical unit of information is called **file**, abstracting from physical properties of its storage devices.
- A file system is usually organized into **directories** (or folders) for easy navigation and usage.
- A file is a collection of related information defined by its creator. A file may be a program or may contain data. 
- The operating system provides services to access files and directories, including:
    - Creating and deleting files and directories
    - Supporting primitives for manipulating files and directories
    - Mapping files onto secondary storage
    - Backing up files on stable (nonvolatile) storage media

## Mass-Storage Management


## Cache Management

## I/O Management


# Protection and Security


## Operating Systems Implementations and Techniques
