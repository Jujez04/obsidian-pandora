# Basic concepts
- **Embedded systems** -> 
	- *special-purpose* computing systems 
	- embedded in electronical/physical systems
	- made interactions with **sensors** and **actuators**
	- several applications: automotive, avionics, health care, smart home,...
- What we mean for "special-purpose"?
	- *One specific task* is done in a *never-ending* program
	- designed for robustness and minimisation in terms of resources
	- user interface/experience made ad hoc
- This leads on the main property of *efficiency* -> reduce power consumption, size, cost and so on
- There are other aspects to be aware, like for example in *critical systems*, where many problems come up, like *safety, security, reliability, availability and dependability*
- In some cases we will also encounter problems like *reactivity* and *real-time* response

# CPS and Industry 4.0
- **Cyber-Physical Systems** (CPS):
	- system that integrates *computations* with *physical* *processes* (not *information* processing)
	- strong *time-dependent* -> not only for performance, but for correctness and *reactivity*
	- *concurrent* systems -> physical processes are typically in parallel
	- composed of three parts:
		- **physical part** -> devices, mechanical/biological systems, chemical processes
		- **computational part** -> computing platforms equipped with sensors and actuators
		- **network part** -> provides mechanisms to connect to the computing platforms
	- Examples:
![[Pasted image 20250921082906.png]]
![[Pasted image 20250921082942.png]]
- CPS are really involved in the **Industry 4.0** and also technological advancement that leads to the world of **automation**
- Factories have been starting to include:
	- CPS -> virtual representation of physical assets, allowing remote/distributed decision making, monitor physical processes
	- IoT systems -> CPS cooperate and communicate through the Internet and human operators
	- cloud computing -> communicating with external entities for decision making

# Embedded System Architecture
![[Pasted image 20250921092022.png]]

# Hardware architecture
![[Pasted image 20250921092055.png]]

## CPU technology
- CPU are divided in several kind of technologies:
	- **general-purpose processors** -> an instruction set architecture (ISA) available for building up a program that defines the behavior of the system
	- **single-purpose processors** -> digital circuits designed to implement a specific function. ASIC (Applciation-Specific Processors) belongs to this category
	- **Application-Specific Processors** (ASIP) -> programmable processors optimised for specific applications with common features
- Embedded systems belongs to the ladder category with *microcontrollers* and *System-on-a-Chip* circuits (SoC)

## Single-purpose processors
- Different implementations 
	- Full-Custom/VLSI -> the implementation accounts for designing the full processor in a custom way, optimising all levels 
	- Semi-custom -> ASIC in this case the starting point is a set of predefined levels, implementing the processor on top 
	- PLD (programmable logic device) -> all levels are already there, but can be configured/ “programmed” for the purpose. The levels implement programmable circuits, where the programmability is given by the possibility of create/destroy connections, gates, etc.
	- PLA (Programmable Logic Array) -> programmable array of AND gates + OR gates 
	- PAL (Programmable Array Logic ) -> single type of array 
	- FPGA (Field Programmable Gate Array) – >integrated circuits with programmable functions via software • more general level of programmability compared to PLA and PAL – allows for implementing complex logics, strong scalability • millions of gates for a single FPGA device – programming languages: Verilog, VHDL – schematic-entry visual-based programming languages • LabView

# Microcontroller unit (MCU)
- **integrate on a single** **chip** all the components required to have *full functional autonomy* for the tasks that the embedded is designed for
- In each of them we can find:
	- CPU, persistent memory, volatile memory, I/O channels (pins), interrupt controller unit, and other specialised components 
	- both CISC CPU based on Von Neumann architecture, and RISC based, such as the Texas Instruments MSP430
![[Pasted image 20250921094339.png]]
## Single-board microcontroller
- A **complete development board** that has:
	- An MCU chip (the core)
	- Supporting circuitry (voltage regulators, clock crystals, USB-to-serial converters, headers/pins, etc.)
	- Often extra features (onboard sensors, WiFi, LEDs, prototyping-friendly connectors).
- Arduino family and Raspberry Pico
## SoC
- Integrates a **high-performance CPU (or multiple cores)** + GPU + large memory controllers + advanced peripherals (USB, Ethernet, PCIe, display, etc.).
- Can run **full operating systems** (Linux, Android, Windows IoT). 
- Often used in smartphones, tablets, smart TVs, single-board computers.
- i.e. Raspberry main line, ESP32 and its predecessor ESP 8266

## Relationship among them
![[Pasted image 20250921095009.png]]
- The choice depends on the requirements of the project and it is always a trade-off 
- Nevertheless, solutions based on microcontrollers and SoC — and related single boards — are more and more adopted and pervasive:
	- they ease the development of families of products, that can be designed to provide different kind of functionalities, at different costs, flexibility, efficiency, scalability with the complexity 
	- the use of processors with a predefined instruction set often leads to a quicker implementation (than the use of custom logics) -> this is the effect of the continuous progress of the CPU
# Sensors
- transducers devices that make it possible to measure a certain specific physical phenomenon (e.g. temperature, humidity, acceleration, distance, etc) or detect and quantify a chemical concentration (e.g. smoke)
- provide a measurable representation of a phenomenon in a specific scale or interva
- can be either analog or digital:
	- analog => the electrical measurement produced in output — either voltage or current — changes according to variations of the measured phenomenon in input -> for analog sensor, the microcontroller integrates a analog-digitalconverter (ADC) 
	- digital => the electrical value is mapped into a discrete set of values (e.g. two values, HIGH and LOW)
![[Pasted image 20250921100747.png]]
# Actuators
- devices that produce some measurable effect on the environment

# Bus and communication interfaces
- The interaction between the MCU/microprocessor and sensors/actuators is enabled by **communication channels (bus)** based on specific communication **protocols**
- In embedded systems buses/protocols are typically **serial**
- Examples: I$^2$C, SPI, JTAG, CAN-bus

## UART
- Universal Asynchronous Receiver-Transmitter
- oldest serial protocol, supported by any MCU
- allows for converting parallel bit streams into a sequential asynchronous stream, and viceversa
- actually UART does not generate or receive directly the signals to be converted: this is typically done by separate interface devices/protocols such as RS-232, Bluetooth, USB,…
- USART (Universal Synchronous/Asynchronous Receiver-Transmitter) extends the protocol with a line for a clock signal for synchronisation

## I$^2$C, SPI, JTAG
- I2C (Inter-Integrated Circuit) 
	- widespread serial bus/protocol synchronous 
	- very simple: only 2 lines — 1 for a clock, 1 for data 
- SPI (Serial Peripheral Interface) 
	- another synchronous bus/protocol 
	- more complex than I2C 
- JTAG 
	- standard protocol for testing devices and useful for debugging embedded software

## CAN-BUS
- Controller Area Network (CAN-bus) 
- standard serial protocol used in automotive, introduced by Bosch 
- designed to connect multiple control units 
- robust enough to work in environments with electromagnetic interferences
- based on message multicast

## Communication interfaces
- Technologies and standards that allow for a wired and wireless communication with other systems (either embedded or not)
- Wireless in particular -> Bluetooth and Bluetooth Low-Energy (BLE), ZigBee, Z-Wave, LoRoWan, WiFi, …
# Development of an embedded systems
### **Modeling**
- **Goal:** Build _understanding_ of the system to be developed with descriptions.
- Divided into two parts:
	- **physical** -> mathematical models & control theory
	- **logical** -> state machines and their composition, tasks and concurrent models
- **Output:** **Models** → abstract representations (diagrams, specifications, formal models).
- **Focus:** _What the system does_.
- **Example:** UML use case diagrams to describe system functionality.

---
### **Design**
- **Goal:** Create the **blueprint** of the system.
- **Output:** **Design artefacts** (architectures, component diagrams, interfaces, algorithms).
- **Focus:** _How the system does what it does_.
- **Note:** Modeling often supports design, providing high-level abstractions independent of implementation.
- **Example:** Class diagrams, database schema, architecture layers.

---
### **Analysis**
- **Goal:** Understand and evaluate the system’s **behavior** (often after or during implementation).
- **Output:** Insights about correctness, performance, or causes of problems.
- **Focus:** _Why the system behaves as it does (or fails to)_.
- **Example:** Debugging a software system, performance analysis, failure analysis.

![[Pasted image 20250921110343.png]]

# Requirements
## **Requirements Elicitation & Collection**
- Process of **gathering requirements** from stakeholders (customers, users, domain experts, regulators, etc.).
- Aimed at understanding **what the system is expected to do** and **under which constraints** it must operate.
- Produces the **requirements specification** (often the Software Requirements Specification – SRS).

---
## **Types of Requirements**

### 1. **Functional Requirements (FR)**
- Define the **functions, tasks, or services** the system must perform.
- Answer the question: **What should the system do?**
- Usually expressed as **use cases**, scenarios, or system responses to inputs.
**Examples:**
- “The system shall allow a user to log in using a username and password.”
- “The autopilot shall maintain altitude within ±10 m.”
### 2. **Non-Functional Requirements (NFR)**
- Define the **qualities and constraints** of the system.
- Answer the question: **How should the system be? / Under what constraints should it operate?**
- Often measurable through quality attributes.
**Common categories:**
- **Performance:** response time, throughput, latency.
- **Reliability & Availability:** uptime %, fault tolerance.
- **Usability:** user interface design, accessibility.
- **Cost:** manufacturing cost, NRE (Non-Recurring Engineering) costs.
- **Physical constraints:** size, weight, dimensions.
- **Energy constraints:** power consumption, battery life.
- **Security & Safety:** encryption, compliance, safety standards.

# Design & Architecture
- Description of how the system implements the functions described in detail
- **Architecture**:
	- plan of the overall structure of the system, to be used to drive the development of components
	- it describes which components are part of the system and how they interact
- **Block diagrams** are always used for the description of hardware + software architecture
![[Pasted image 20250921110858.png]]

# Programming embedded systems
## Embedded Software Development
### 1. **Development Approach**
- **Traditional development**
    - Manual coding directly in C/C++ (close to the hardware).
- **Model-driven development (MDD)**
    - Use of higher-level models (e.g., UML, Simulink) that can generate code automatically.
    - Focuses on abstraction and faster prototyping.

---
### 2. **Host vs Target**
- Development is **done on host computers** (e.g., PC with IDE, compiler).
- The **toolchain** (compiler, linker, debugger, cross-compiler) produces an **executable binary**.
- The binary is **uploaded to the embedded target** (MCU or SoC) for execution.

---
### 3. **Languages & Platforms**
- **Low-level, traditional choice:**
    - **C/C++ + system libraries** → dominant in microcontroller-based solutions.
    - Offers direct hardware control, efficiency, and portability.
- **When an embedded OS is present (RTOS, Linux, Android):**
    - Broader set of languages and runtime platforms can be used.
    - **Languages:** C, C++, but also higher-level languages like Java, JavaScript, C#, Python, Rust.
    - **Platforms:** Embedded Java, Android, MicroPython, NodeMCU (JS/Lua), .NET nanoFramework.

---
### 4. **Debugging & Testing**
- **Serial connection (UART/USB):**
    - Logging messages from the device to the host PC (“printf debugging”).
- **JTAG interface/protocol:**
    - Hardware debugging → step execution, breakpoints, memory/register inspection.
    - Used to program and test MCUs, SoCs, and FPGAs.