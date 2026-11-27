Let's recall what a microcontroller is. A microcontroller is a special-purpose computing system where everything is embedded inside a single processor and I/O works with sensors and actuators.
# Microcontrollers elements
![[Pasted image 20250923184525.png]]
From the image above, we can see that microcontrollers are composed of several elements:
- CPU, with its registers and ALU
- Memory unit (RAM + EEPROM)
- Ports for *general-purpose I/O* (**GPIO**)
- Analog/digital converter for transforming signals
- Timers
- Bus for serial communication
- Power circuit

In this part we will work with Arduino UNO architecture and this should be an exhaustive example:
- Microcontroller used ->  **MCU ATMega328P** 
	- 8 bit, 16 MHz 
	- Flash memory: 32 KB 
	- SRAM memory: 2 KB, EEPROM: 1 KB 
- 14 pins for *digital* input/output • 6 can be used PWM outputs 
- 6 *analog* inputs 
- USB connector 
- power jack 
- ICSP header 
- reset button
- Operating voltage: 5V 
- Input voltage (recom.): 7-12 V 
- DC current per I/O pin: 40 mA 
- DC current per 3.3V: 50mA
![[Pasted image 20250923185400.png]]

# MCU Programming
- MCU programming is done by using an **external host system** (e.g. a PC), where programs are edited, compiled, creating the executable (or binary)
- The executable is then **uploaded** to the MCU by means of specific HW, or directly by means of the *serial communication* with the host PC

## Arduino case
- Arduino programs are typically developed on a host PC and uploaded by means of USB cable, besides, an **ICSP** (In-Circuit Serial Programmer) *interface* is available for uploading the executable using a specific external programmer device 
- The upload via USB is done by means of a **bootloader**, a small program(~0.5KiB) pre-stored (via ICSP) 
- Programming environment/IDE:
	- **Wiring**: is an *open-source C/C++ framework*, available for different kind of microcontrollers and SoC as well ( http://wiring.org.co/ )
	- **Arduino IDE**: a simple but effective IDE for editing sources, compiling, uploading binaries (working not only for Arduino..), and it is based on GNU tools and other open source projects, like **avr-gcc** and open-source libraries (e.g. AVR Libc)

## MCU ATMega328P
### Code (Instructions)
- Stored in **FLASH** (non-volatile) – 32 KiB  
- 0.5 KiB used by the **bootloader**
### Data
- Variables / stack stored in **SRAM** (volatile) – 2 KiB  
- Long-term / persistent data stored in **EEPROM** (non-volatile) – 1 KiB  
### Remarks
- The amount of memory for data/variables is very small  
  ➝ disciplined use of data structures (e.g., minimizing the use of strings)  
- The stack is stored in **RAM**  
  ➝ small memory implies constraints on the level of call nesting and recursion
### AVR8 Microprocessor (inside the ATMega328P)
- **RISC architecture**  
  - Code: 8 bits  
  - Data: 16 bits  
  - Clock: 16 MHz  
#### CPU Registers
- **32 general-purpose registers (R0–R31, 8-bit each)**  
  - All arithmetic and boolean operations involve these registers  
  - Only load/store operations access the RAM memory  
- **Special-purpose registers**  
  - **PC**: 16- or 22-bit Program Counter  
  - **SP**: 8- or 16-bit Stack Pointer  
  - **SREG**: 8-bit Status Register  
    - **C**: Carry flag (used in subtraction, overflow handling)  
    - **Z**: Zero flag (set when an arithmetic result is zero)  
    - **I**: Interrupt flag (set when interrupts are enabled)  
- Both general-purpose CPU registers and I/O registers are mapped into low memory (and accessible using memory addresses in that range)
![[Pasted image 20250923191035.png]]

#  Super loop
- The super-loop is the simplest **control architecture** (a framework) adopted for microcontroller programming, since it does *not require* any specific *HW support* (such as *interrupts*)
- It is renowned for its *simplicity*, *efficiency*, *portability* and *safety* but it lacks in terms of *timing* and low *flexibility*
- There are two main operations that can be done
	- **setup** for initializing variables and allocating memory
	- **loop** procedure that will do a task endlessly
- *Wiring* as the IDE + avr-gcc as the compiler+linker will be sufficient for programming microcontrollers
- Disassembly of the control loop:
![[Pasted image 20250923192408.png]]
![[Pasted image 20250923192444.png]]
![[Pasted image 20250923192456.png]]

Practically it is done in this way:
```cpp
void main() {
    setup();
    while(true) {
        loop();
        if (Serial.available()) serialEvent(); // <-- Chiamato SOLO qui
    }
}
```

// AGGIUNGI DETTAGLI SULL'OGGETTO Serial: in pratica è uno stream di dati
# GPIO
- Every microcontroller typically features a *set of **pin*** che can be directly used to drive input/output 
- Pins typically are general-purpose ports: can be programmed to work as **input** or **output** depending on the necessity
- Pins can be either:
	- **digital**: can assume two values only, HIGH or LOW (1 o 0) 
	- **analog**: can assume any value in a range (0…5 volts)
- Arduino has 14 digital pins, configurable either as input or output, and 6 input-only input pins.
- There are some functions that can *multiplex* pins for implementing multiple functions
- Reference operating parameters for pins: 
	- **voltage** (in Volt) -> to be applied on the pin (input case) o produced by the pin (output case); example: 5 V (Arduino), 3.3 V (Raspberry Pi and ESP) 
	- **current** (in Ampère) -> that can receive (input) or that can be produced (output); example: Arduino: 40 mA, ESP8266 12mA
- There are circuits that can fix a signal line to a logical value when it is otherwise not driven. A **pull-up resistor** connects the line to +VCC through a resistor, ensuring the signal defaults to HIGH instead of floating.
- Arduino input pins are **high impedance**, meaning they don’t load the circuit or sink current significantly. However, without a pull-up (or pull-down) resistor, the input could float.
- The **pull-up resistor’s role** is to provide a stable HIGH level when idle, while still allowing the signal to be pulled LOW by another circuit element (e.g., a switch to ground) without creating a short circuit.
- The MCU interacts with pins through **ports**. A port is an interface that groups pins in which the MCU can communicate. There are three special registers called **SRF** (*Special Purpose Registers*) and they are **PORTB; PORTC; PORTD**:
	- each of them is an 8-bit register controlling 8 pins.
	- These allows Wiring API to write with `digitalWrite` operation
![[Pasted image 20251002172643.png]]
- Each **port is managed by 3 registers**: DDRx, PORTx, PINDx, where x is the port name. For instance, for the port D; 
	- DDRD -> this register is both Read/Write and stores the *configured* *direction* for the *pins* (bit set to 0 => pin configured as INPUT; bit set to 1 => pin configured as OUTPUT) 
	- PORTD -> this register is both Read/Write and stores the *pin state,* which changes depending on the pin configuration: 
		- for INPUT pins, then a bit set to 1 activates the PULL-UP resistor, while a bit set to 0 deactivates it; 
		- for OUTPUT pins, then a bit set to 1 represents a state set to HIGH on the corresponding pin, while a bit set to 0 represents a state set to LOW 
	- PIND -> this register is *read-only*, and stores, for *INPUT pins*, the signal value read connected to the pin: 1 if the signal value is HIGH; 0 if the signal value is LOW
- Therefore, to configure an OUTPUT pin to the HIGH value, one must: 
	- write on the DDRx register to set the direction (OUTPUT) 
	- write on the PORTx register to set the state (HIGH) 
- As already mentioned, in the ATmega328 MCU, and in many other microcontrollers and general-purpose microprocessors as well, I/O registers are mapped into the memory 
	- so that they can be uniformly accessed/manipulated bu writing/ reading bytes/words at the specific memory addresses 
	- in the ATMega328P, they are located starting from the address 0x20, just after the general purpose registers
##  Wiring API
- Wiring API works with pins, in fact there are several instructions that can manipulate pins:
	- `digitalWrite` : for setting a value in the INPUT pin
	- `pinMode` : for direction, it can be either INPUT or OUTPUT
	- `digitalRead`: for reading a value in the OUTPUT pin
- i.e. -> blinking a led:
```cpp
#define LED_PIN 13
void setup() {
 pinMode(LED_PIN, OUTPUT);
}
void loop() {
 digitalWrite(LED_PIN, HIGH);
 delay(1000);
 digitalWrite(LED_PIN, LOW);
 delay(1000);
}
```
- `delay` is a procedure available in Wiring that executes a **busy waiting** for the specified amount of time in milliseconds:
```cpp
void delay(unsigned long ms)
{
	 unsigned long start = millis();
	 while (millis() - start <= ms)
	 ;
}
```
- `millis` -> returns the current number of milliseconds elapsed since the power on of the device and it accesses variables managed by a timer -> it impacts on the reactivity of the system
- Besides being general purpose, some pins have a *further function* that can be configured/activated: 
	- pins 0 and 1 => TTL serial interface 
	- pins 2 and 3 => interrupts 
	- pins 3,5,6,9,10 e 11 => PWM 
	- pins 10, 11, 12, 13 => SPI bus –
	- pins 13 => Builtin led 
	- Analog pin –> pin A4 and A5 => I2C bus
## PWM
- Recall that a digital signal can be settled in either HIGH or LOW state, which represents the maximum flow of current and the minimum one.
- In Arduino One board there 5 pins for analog IO and 6 of digital one that can be used for PWM
- Analog signals can assume values in range of GND to VCC (0 -> 5 V)
- This means that an analog signal needs to converted firstly with an **ADC** (*Analog Digital Converter*), and then mapping values into a range of digital ones, depending on the number of bits used by the ADC. This feature is implemented with a value *resolution* 
- Wiring API provides two functions that handle conversion+mapping with `analogRead` or exclusively mapping with the function `map`
- An example can be done with a *potentiometer*, that has a lever that settles a certain analog value that can be converted in a range of 0 to 1023
- **PWM** or *Pulse-width Modulation* is a method to **emulate** *analog* *output* *signal* on *digital* *pins*, through a periodic square wave:
![[Pasted image 20251011160555.png]]
- The output voltage emulated is defined by the **duty cycle** of the signal -> percentage of time in which the signal is HIGH over the time in which the signal is LOW
- The **average** voltage produced is equal to the *duty cycle * VCC*
- PWM doesn't work with analog device/actuators, but it works with LEDS (controlling the brightness) or current motor (controlling the speed)
- Main function -> `analogWrite`, prove it with *led fading*

# Interrupts
- Interrupts are the fundamental **mechanism** used by the microcontroller to *react to events triggered by external devices*
- A CPU provides multiple pins, called **IRQ** (*Interrupt* *Request*) , to receive interrupt signals:
	- when an interrupt request is receive, the CPU suspends the execution of current instructions (e.g. the super-loop), it saves on a stack the address of the next instruction to be executed, and jumps (transferring the control) to the routine (procedure) configured to react to the event ,called **interrupt handler** or **interrupt service routine (ISR)**
	- the ISR address is stored into a table called **interrupt table**
- In microcontrollers, IRQ are typically connected to one or multiple GPIO
- The ATMega328P has 2 pins, pin 2 and pin 3, that can be configured to generate (to the CPU) external interrupts
- In particular, the pin can be **configured** to generate an interrupt on different conditions 
	- when there is a RISING or FALLING edge 
	- when there is a CHANGE 
	- when the signal is LOW
- actually it is possible to enable and manage interrupts for all pins, but limiting to a specific kind of interrupt: PIN CHANGE (vs. EXTERNAL) https://github.com/GreyGnome/EnableInterrupt

## Handling interrupts in Wiring
- In Wiring it is possible to directly specify and attach an ISR by means of the routine: `attachInterrupt(intNum, ISR, mode)`
	- `intNum`: interrupt number (int) 
	- `ISR`: pointer to the interrupt handler (`void *func(void)`) 
	- `mode`: when the interrupt should be generated: CHANGE/FALLING/RISING: at every change HIGH -> LOW, LOW -> HIGH LOW/HIGH: when the state is LOW or HIGH
- The utility function `digitalPinToInterrupt(numPin)` makes it possible to get the number of the interrupt (see interrupt vectors) given the specified pin
- Note that whether the program doesn't enable/disable interrupts, there could be *race conditions*
- In Arduino API there are several options for disabling/enabling interrupts.
- In a general CPU there two main function:
	- STI = Set Interrupt (enable)
	- CLI = Clear Interrupt (disable)
- In Arduino/Wiring:
	- `noInterrupts`
	- `interrupts`
- i-e.
```cpp
//Before
volatile int count = 0;
void setup()
{
 // 0 => detecting pin 2 changes
 attachInterrupt(0, inc, CHANGE);
 Serial.begin(9600);
}
void loop()
{
 Serial.println(count);
}
void inc()
{
 count++;
}

//After
#define BUTTON_PIN 2
volatile int count = 0;
int prev = 0;
void setup()
{
 Serial.begin(9600);
 attachInterrupt(digitalPinToInterrupt(BUTTON_PIN), inc, RISING);
}
void loop() {
 noInterrupts();
 int current = count;
 interrupts();
 if (current != prev){
 Serial.println(current);
 prev = current;
 }
}
void inc() { count++; }
```
- When interrupts are **disabled**, the system **is no more reactive** to external events
- For this reason, the design of an interrupt handler should be ruled by a careful discipline:
	- interrupt handlers should involve **essential instructions**, avoiding infinite loops 
	- *low interrupt latency* (amount of time used to react an interrupt) -> minimising the time period in which interrupts are disabled
	- use buffers and producers/consumers strategy to do longterm / heavy computational tasks in reaction to interrupts
- On AVR / Arduino, when the interrupt handler is executed, interrupts are disabled -> library functions/procedures whose function depends on interrupts cannot be used in interrupt handlers
- An useful tip: the critical section must be on the *main* super *loop*, not in ISR. This implies that all the variables that are manipulated (reading/writing) either inside the super loop and in a ISR must be protected by the `noInterrupt` / `interrupt` section

## Debouncing
- Pushbuttons often generate spurious open/close transitions when pressed, due to mechanical and physical issues: these transitions may be read as multiple presses in a very short time fooling the program.
- This example demonstrates how to **debounce** an input, which means *checking twice* in a short period of time to make sure the pushbutton is definitely pressed.
```cpp
/* From Arduino documentation
  Debounce

  Each time the input pin goes from LOW to HIGH (e.g. because of a push-button
  press), the output pin is toggled from LOW to HIGH or HIGH to LOW. There's a
  minimum delay between toggles to debounce the circuit (i.e. to ignore noise).

  The circuit:
  - LED attached from pin 13 to ground through 220 ohm resistor
  - pushbutton attached from pin 2 to +5V
  - 10 kilohm resistor attached from pin 2 to ground

  - Note: On most Arduino boards, there is already an LED on the board connected
    to pin 13, so you don't need any extra components for this example.

  created 21 Nov 2006
  by David A. Mellis
  modified 30 Aug 2011
  by Limor Fried
  modified 28 Dec 2012
  by Mike Walters
  modified 30 Aug 2016
  by Arturo Guadalupi

  This example code is in the public domain.

  https://www.arduino.cc/en/Tutorial/BuiltInExamples/Debounce
*/

// constants won't change. They're used here to set pin numbers:
const int buttonPin = 2;  // the number of the pushbutton pin
const int ledPin = 13;    // the number of the LED pin

// Variables will change:
int ledState = HIGH;        // the current state of the output pin
int buttonState;            // the current reading from the input pin
int lastButtonState = LOW;  // the previous reading from the input pin

// the following variables are unsigned longs because the time, measured in
// milliseconds, will quickly become a bigger number than can be stored in an int.
unsigned long lastDebounceTime = 0;  // the last time the output pin was toggled
unsigned long debounceDelay = 50;    // the debounce time; increase if the output flickers

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);

  // set initial LED state
  digitalWrite(ledPin, ledState);
}

void loop() {
  // read the state of the switch into a local variable:
  int reading = digitalRead(buttonPin);

  // check to see if you just pressed the button
  // (i.e. the input went from LOW to HIGH), and you've waited long enough
  // since the last press to ignore any noise:

  // If the switch changed, due to noise or pressing:
  if (reading != lastButtonState) {
    // reset the debouncing timer
    lastDebounceTime = millis();
  }

  if ((millis() - lastDebounceTime) > debounceDelay) {
    // whatever the reading is at, it's been there for longer than the debounce
    // delay, so take it as the actual current state:

    // if the button state has changed:
    if (reading != buttonState) {
      buttonState = reading;

      // only toggle the LED if the new button state is HIGH
      if (buttonState == HIGH) {
        ledState = !ledState;
      }
    }
  }

  // set the LED:
  digitalWrite(ledPin, ledState);

  // save the reading. Next time through the loop, it'll be the lastButtonState:
  lastButtonState = reading;
}
```
## FAQ
• Can a processor being interrupted when executing a single instruction?
– no, typically machine language instructions are executed atomically (modulo complex
instructions that account for multiple memory transfers)
• If two interrupts occur in the same time, which one is served?
– every CPU assigns some priority to interrupts, so that it executes always the interrupt
with higher priority
• About interrupt nesting: can an interrupt request interrupt the execution of an interrupt
handler?
– in most processors YES, so we need to disable interrupt in the interrupt handler (and
re-enable them)
– in AtMega328P / AVR8 NO, it does not support interrupt nestinge
• What happen to interrupt requests when interrupt are disabled?
– typically a CPU keeps track of such requests and execute them as soon as the
interrupts are enabled again
– AVR8 does not track them
• What happens if we disable interrupts and we forgot to enable them again?
– system malfunctioning, crash
• What happens if we disable interrupts multiple times (before re-enabling)?
– nothing, they are idem-potent
# Timers
- A timer can be used for: 
	- measuring time intervals
	- creating PWM signals, timeouts/alarms or multi-tasking based on preemptive scheduling
- The frequency can be configured for reading the current value of the counter or *attach* *interrupts* to implement time-triggered behaviours
- The ATMega328 has 3 internal timers 
	- timer0 -> 8 bit counter 
	- timer1 -> 16 bit counter
	- timer2 -> 8 bit counter
- They are incremented *each* *clock* *cycle*
- Also the API uses timers:
```cpp 
unsigned long millis()
{
 unsigned long m;
 uint8_t oldSREG = SREG;

 cli();
 m = timer0_millis; // timer0_millis contains the value of the timer0 counter
 SREG = oldSREG;

 return m;
}
```
- Or `analogWrite()` procedure, for PWM signals
## CTC and Interrupt frequency
- ATMega328 works in different modes in terms of timers and one of them **CTC** (*Clear Timer on Compare Match*) -> timer interrupts are generated when the counter reaches a certain value, stored into the *compare match register*; once it has reached that value, it starts counting again
- By choosing the value of the CTC register, we can specify *interrupts frequency*
- `timer speed (Hz) = (Arduino clock speed (16MHz)) / prescaler`
- the value of the `prescaler` can be 1, 8, 64, 256, and 1024.
- `desired interrupt frequency (Hz) = 16,000,000Hz / (prescaler * (compare match register value + 1))` -> reversed for obtaining the compare match register value
- Checking overflows:
	- for timers 0 and 2, this value must be less that 256 –
	- or timer 1 the value must be less than 65536
	- if there is an overflow, then we have to increase the prescaler
- Each timer has 2 8-bit registers to store timer configuration: `TCCRxA, TCCRxB` (where x = timer number 1..3)
![[Pasted image 20251011173240.png]]
- the most important bits are the 3 least significant ones CSx2, CSx1, CSx0, determining **timer clock configuration**
- CTC modality is enabled by activating specific bit in TCCRxB 
- CTC register is labelled as OCRxA
- Raw level example:
```cpp
void setup(){
 Serial.begin(9600);
 setupTimer(1000);
}
volatile boolean timerFlag = false;
ISR(TIMER1_COMPA_vect){
 timerFlag = true;
}
int count = 0;
void step()
{
 count++;
 if (count % 100 == 0){
 Serial.println(count);
 }
}
void loop(){
 /* wait for timer signal */
 while (!timerFlag){}
 timerFlag = false;
 step();
}
/* setup the timer1 (16 bit) to
 * a specific freq
 */
void setupTimer(int freq){
 // disabling interrupt
 cli();
 TCCR1A = 0; // set entire TCCR1A register to 0
 TCCR1B = 0; // same for TCCR1B
 TCNT1 = 0; //initialize counter value to 0
 /*
 * set compare match register
 * OCR1A = (16*2^20) / (100*PRESCALER) - 1
 * (must be < 65536)
 * by assuming a prescaler = 1024, then
 * OCR1A = (16*2^10)/freq
 */
 OCR1A = 16*1024/freq;
 // turn on CTC mode
 TCCR1B |= (1 << WGM12);
 // Set CS11 for 1024 prescaler
 TCCR1B |= (1 << CS12) | (1 << CS10);
 // enable timer compare interrupt
 TIMSK1 |= (1 << OCIE1A);
 // enabling interrupt
 sei();
}
```
- In Wiring there is a library that works with timer1 called **TimerOne** -> it provides classes that allow to configure the times and to attach interrupt routine
- https://code.google.com/p/arduino-timerone
- Time-Driven blinking example:
```cpp
#include<TimerOne.h>
const int LED_PIN = 13;
boolean flagState = false;
void blinkHandler(){
 if (!flagState){
 digitalWrite(LED_PIN, HIGH);
 } else {
 digitalWrite(LED_PIN, LOW);
 }
 flagState = !flagState;
}
void setup()
{
 pinMode(LED_PIN,OUTPUT);
 Timer1.initialize(1000000); // set a timer of length 10^6 micro sec = 1 sec
 Timer1.attachInterrupt(blinkHandler); // runs blinkHandler on each timer interrupt
}
void loop(){}
```
## Watchdog Timer
- A **watchdog timer (WDT)** is a hardware or software *mechanism* that helps a system **automatically recover from faults or hangs**
- A watchdog timer is a *countdown timer* that is periodically *reset* (or "kicked") by the running program. If the timer ever *reaches zero*, it assumes that the program has *crashed* or is stuck, and it *triggers* a system reset or another recovery action.
- It is used to ensure that the control software never freezes.
- Timers are used for **implementing PWM**, in particular:
	- Timer0 is used for pin 5 and 6
	- Timer1 is used for pins 9 and 10
	- Timer2 is used for pins 11 e 3 
- We need to pay attention to **interferences** when using *timers* and *PWM* *capability* -> i.e. if we use Timer1 directly, then we cannot use pin 9 and 10 for PMW
- In `TimerOne` library we can create sections where we can use the timer provided by the library and `analogWrite` but not in overlapping sections --> We must *stop* the timer before in an `analogWrite` section
- WDTs are more drastic since they call **hardware** **reset**:
```cpp
#include <avr/wdt.h>  // AVR watchdog library

void setup() {
  Serial.begin(9600);
  Serial.println("Starting...");
  
  // Enable watchdog with 2-second timeout
  wdt_enable(WDTO_2S);
}

void loop() {
  Serial.println("Running normally...");
  delay(1000);

  // Reset (kick) the watchdog before timeout
  wdt_reset();

  // Simulate a hang after a few loops
  static int counter = 0;
  counter++;
  if (counter == 5) {
    Serial.println("Simulating crash...");
    while (true) {
      // Program hangs here — no wdt_reset() anymore
      // → After ~2s, MCU will reset automatically
    }
  }
}
```
- The board prints “Running normally…” a few times.
- Then it “crashes” (infinite loop).
- The watchdog times out → the microcontroller **resets** itself → restarts `setup()`.
- You’ll see “Starting…” printed again — confirming recovery.
- Example of using WDT to detect a problem but not reset:
```cpp
#include <avr/wdt.h>

ISR(WDT_vect) {
  // Watchdog interrupt: handle before reset
  Serial.println("WDT interrupt triggered!");
}

void setup() {
  Serial.begin(9600);

  // Enable watchdog interrupt and reset mode
  WDTCSR = (1 << WDCE) | (1 << WDE);
  WDTCSR = (1 << WDIE) | (1 << WDP2);  // Interrupt first, 0.25s timeout
}

void loop() {
  // Intentionally not calling wdt_reset() to trigger the interrupt
  delay(1000);
}
```
- The watchdog triggers an interrupt before reset.
- You can use that ISR (interrupt service routine) to **save critical data** or **signal a fault** before reboot.

| Application                                           | Purpose of WDT                                 |
| ----------------------------------------------------- | ---------------------------------------------- |
| Embedded sensor node                                  | Restart device if communication freezes        |
| Industrial controller                                 | Recover from software crash                    |
| Remote IoT device                                     | Ensure system keeps running even if code hangs |
| Safety-critical systems (e.g., automotive, aerospace) | Guarantee continuous operation                 |
# Buses for serial communication
- Protocols to exchange data through single pins can become complex depending on the external communicating I/O device --> embedded engineers implemented specific communication *interfaces*, *buses* and *protocols*
- Two type of interfaces:
	- **parallel** --> transferring multiple bits at the same time with *multiple* *lines*
	- **serial** --> send and receive streams of bits in a sequential way, using a *single* *line*
- Then they are divided into two main categories:
	- **asynchronous**:
		- no clock line is used for synchronisation
		- two lines --> TX to transmit and RX to receive
		- examples: USB, RS-232, TTL
	- **synchronous**:
		- a **clock** **signal** is used to synchronise the communication between communication parts
		- examples: I2C and SPI
## Asynchronous serial lines
- In serial asychronous interface, since there is not a specific clock line, the *synchronisation* is achieved trough a **protocol**, to ensure an error-free and robust bit transmission
- The protocol can be configured by specifying some parameters, that must be adopted by both the communicating parts:
	- baud rate --> bits per second; typical values are 1200, 2400, 4800 and so on proceeding with multiple of 2 until 115200, that is the limit for microcontroller capabilities
	- data frame 
	- synchronous bits 
	- parity bit
- The `Serial.begin()` and in general the `Serial` class use this type of communication
- Each data block - typically one byte - is transmitted inside a (bit) packet called **frame**. These frames are created adding to the data block also a synchronization bit and a parity bit(called header):
![[Pasted image 20251018091152.png]]
	- The data chunk (block) represents the real data transmitted: each packet can have a data chunk from 5 to 9 bit (typical value: 8)
	- For default they use **little** **endian**
	- Synchronisation bits include a start bit and a stop bit, to mark the beginning and end of the packet
	- Parity bits (optional) are used for simple low-level error checking.
- 9600 8N1 - 9600 baud, 8 data bits, no parity, and 1 stop bit - is one of the most used protocol:
![[Pasted image 20251018091432.png]]
- Two pins are needed then - one to receive (RX) and one to transmit (TX):
![[Pasted image 20251018092758.png]]
- There are also other categories:
	- **full-duplex**:  if both the devices can simultaneously transmit and receive (different data lines)
	- **half-duplex**: if the devices can send or receive in turn
	- **single wire**: some serial bus can have a single wire, between a device which always send and a device which always receive
![[Pasted image 20251018092815.png]]
### UART
- A *main* *component* of a serial system is **UART** (*universal asynchronous receiver/transmitter*), that is the circuit block that manage the data to and from the serial interface.
- It functions as a converter between the parallel interface (data bus) and the serial interface
![[Pasted image 20251018092924.png]]
- UARTs exist also as *stand-alone integrate circuits*, but typically they are *embedded inside microcontrollers*
![[Pasted image 20251018093020.png]]

### Libraries
- Serial class is an extension of Stream and includes methods such as: 
	- begin(), end()  -> to begin and end the communication session 
	- read(), readBytes(), readBytesUntil(), peek(), available() -> to receive data 
	- parseFloat(), parseInt()
	- write(), flush() -> to transmit data 
	- print(), println() -> to transmit strings 
	- setTimeout() 
	- serialEvent()
## Synchronous serial lines
- Synchronous serial interfaces use a clock signal to synchronise and one or multiple lines for sending/receiving data
- *more* *complex* than asynchronous lines, but typically it can reach *higher* *speed*
- `Wire.h` as API
### I2C
- **I²C** ("eye squared see" or "eye two see") is a **synchronous**, **half-duplex**, **two-wire** serial communication protocol commonly used to connect low-speed peripherals to microcontrollers.
- Specs:
	- 7-bit or 10-bit bus
	- bus speed achieves 100 kbit/s in standard mode e 10 kbit/s in low-speed mode, but recent versions have a fast mode at 400 kbit/s, andf fast mode plus (1Mbit/s) and High-Speed mode (3.4Mbit/s)

|Feature|Description|
|---|---|
|Type|Synchronous serial (clocked by master)|
|Lines|2 (SDA = data, SCL = clock)|
|Communication|Half-duplex, multi-master, multi-slave|
|Typical use|Sensors, EEPROMs, displays, RTCs|
- I2C has a **master-slave architecture** for communication:
	- the I2C bus is controlled by a *master* *device*(microcontroller)
	- one or multiple slave devices that receive info from the master
	- All devices share two lines:
		- a *signal clock* (**Serial Clock Line**, SCL), necessary to synchronise the communication 
		- a *bidirectional data line* (**Serial Data Line**, SDA) to send and receive data
![[Pasted image 20251018102901.png]]
### Master-slave protocol
- *Communication* is always *started* by the *master* –> *slaves* can only *respond*
- each *slave* has its own 7 bit *unique ID*, which is sent by the master when it starts the communication
- **Master responsibilities:**
	- Generates the **clock (SCL)**
	- Initiates and terminates communication (via **START** and **STOP** conditions)
	- Selects which slave to talk to (using **7-bit or 10-bit address**)
- **Slave responsibilities:**
	- Listens for its **address** on the bus
	- Responds with an **ACK (acknowledge)** bit when addressed
	- Sends or receives data depending on the R/W bit
- Typical communication schema: 
	1) Master sends start bit 
	2) Master sends the 7-bit id of the slave device 
	3) Master sends read (0) o write (1) bit, either if it wants to write data on device registers or read data from 
	4) Slave responds with an ack (ACK bit, active when it is 0) 
	5) In write mode, the master sends data as a sequence of bytes and the slaves replies with ACKs. In read mode, the master receives a stream of bytes, sending an ACK after each byte received
	6) When communication is finished, the master sends a STOP bit
```cpp
// I2C MASTER CODE
// Upload this to the first Arduino (Master)
// Connections: SDA to SDA, SCL to SCL, GND to GND

#include <Wire.h>

#define SLAVE_ADDRESS 0x08
const int LED_PIN = 13;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  Wire.begin(); // Join I2C bus as master
  Serial.begin(9600);
  Serial.println("I2C Master Ready");
}

void loop() {
  // Send data to slave
  int sensorValue = analogRead(A0); // Read sensor value (0-1023)
  
  Serial.print("Sending to slave: ");
  Serial.println(sensorValue);
  
  Wire.beginTransmission(SLAVE_ADDRESS);
  Wire.write(highByte(sensorValue)); // Send high byte
  Wire.write(lowByte(sensorValue));  // Send low byte
  byte error = Wire.endTransmission();
  
  if (error == 0) {
    digitalWrite(LED_PIN, HIGH);
    Serial.println("Transmission successful");
  } else {
    digitalWrite(LED_PIN, LOW);
    Serial.print("Transmission failed, error: ");
    Serial.println(error);
  }
  
  delay(500);
  
  // Request data from slave
  Wire.requestFrom(SLAVE_ADDRESS, 2); // Request 2 bytes
  
  if (Wire.available() >= 2) {
    int receivedValue = Wire.read() << 8; // High byte
    receivedValue |= Wire.read();          // Low byte
    
    Serial.print("Received from slave: ");
    Serial.println(receivedValue);
  }
  
  digitalWrite(LED_PIN, LOW);
  delay(500);
}

// I2C SLAVE CODE
// Upload this to the second Arduino (Slave)
// Connections: SDA to SDA, SCL to SCL, GND to GND

#include <Wire.h>

#define SLAVE_ADDRESS 0x08
const int LED_PIN = 13;

volatile int receivedValue = 0;
volatile int dataToSend = 0;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  Wire.begin(SLAVE_ADDRESS); // Join I2C bus with address 0x08
  Wire.onReceive(receiveEvent); // Register receive event handler
  Wire.onRequest(requestEvent); // Register request event handler
  
  Serial.begin(9600);
  Serial.println("I2C Slave Ready");
}

void loop() {
  // Process received data
  if (receivedValue > 0) {
    Serial.print("Received from master: ");
    Serial.println(receivedValue);
    
    // Blink LED based on received value
    if (receivedValue > 512) {
      digitalWrite(LED_PIN, HIGH);
    } else {
      digitalWrite(LED_PIN, LOW);
    }
  }
  
  // Prepare data to send back (e.g., a counter or sensor reading)
  dataToSend = millis() / 1000; // Send seconds since startup
  
  delay(10);
}

// Handler for receiving data from master
void receiveEvent(int numBytes) {
  if (numBytes >= 2) {
    int highByte = Wire.read();
    int lowByte = Wire.read();
    receivedValue = (highByte << 8) | lowByte;
  }
  
  // Clear any remaining bytes
  while (Wire.available()) {
    Wire.read();
  }
}

// Handler for when master requests data
void requestEvent() {
  // Send 2 bytes back to master
  Wire.write(highByte(dataToSend));
  Wire.write(lowByte(dataToSend));
  
  Serial.print("Sent to master: ");
  Serial.println(dataToSend);
}
```
**Master:**
- `Wire.begin()` - Initialize as master
- `Wire.beginTransmission(address)` - Start transmission
- `Wire.write(data)` - Send data
- `Wire.endTransmission()` - Complete transmission
- `Wire.requestFrom(address, bytes)` - Request data from slave
- `Wire.read()` - Read received data
**Slave:**
- `Wire.begin(address)` - Initialize as slave with address
- `Wire.onReceive(handler)` - Register receive callback
- `Wire.onRequest(handler)` - Register request callback
---
- Component datasheet: http://ww1.microchip.com/downloads/en/DeviceDoc/21462D.pdf
- ![[Pasted image 20251018104314.png]]
- I2C sensors:
	- MPU6050 -> sensor with compass + accelerometer
	- LSM330 -> sensor with compass + accelerometer
- I2C actuators:
	- LED display
### SPI
- **SPI** is also **synchronous** but designed for **high-speed, full-duplex** communication between a single master and one or more slaves.

| Feature       | Description                                       |
| ------------- | ------------------------------------------------- |
| Type          | Synchronous, full-duplex                          |
| Lines         | 4 main lines: **MOSI**, **MISO**, **SCK**, **SS** |
| Communication | Multi-slave, full-duplex                          |
| Typical use   | Displays, SD cards, ADC/DACs, fast sensors        |
- full duplex -> differently from I2C, SPI *uses different lines for transmitting and receiving data*, and a specific line to select the slave to communicate with
- SPI has a **multi-slave architecture**:
	- There are **two common connection models**:
		- **Independent Chip Selects (standard method)** -> Each slave has its own **Slave Select (SS)** line, while Master drives only one SS low at a time.
		- **Daisy-chain configuration** -> Slaves connected in a series chain (MISO of one → MOSI of next); only one SS line used, but data shifts through all devices in sequence (like a shift register).
- Arduino/ATmega328P natively supports SPI -> 4 GPIO (multiplexed): 
	- pin 10 => SS 
	- 11 => MOSI
	- 12 => MISO 
	- 13 => SCK
```cpp
// SPI MASTER CODE
// Upload this to the first Arduino (Master)
// 
// Connections (Master -> Slave):
// MOSI (11) -> MOSI (11)
// MISO (12) -> MISO (12)
// SCK (13)  -> SCK (13)
// SS (10)   -> SS (10)
// GND       -> GND

#include <SPI.h>

const int SS_PIN = 10;
const int LED_PIN = 9;

void setup() {
  pinMode(SS_PIN, OUTPUT);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(SS_PIN, HIGH); // SS idle high
  
  SPI.begin();
  SPI.setClockDivider(SPI_CLOCK_DIV16); // Slower clock for reliability
  
  Serial.begin(9600);
  Serial.println("SPI Master Ready");
}

void loop() {
  // Read sensor value to send
  int sensorValue = analogRead(A0);
  byte commandByte = 0xAA; // Command identifier
  
  Serial.println("\n--- Sending Data ---");
  Serial.print("Sensor Value: ");
  Serial.println(sensorValue);
  
  // Begin SPI transaction
  digitalWrite(SS_PIN, LOW); // Select slave
  delayMicroseconds(10);
  
  // Send command byte and receive response
  byte response1 = SPI.transfer(commandByte);
  
  // Send high byte and receive response
  byte response2 = SPI.transfer(highByte(sensorValue));
  
  // Send low byte and receive response
  byte response3 = SPI.transfer(lowByte(sensorValue));
  
  delayMicroseconds(10);
  digitalWrite(SS_PIN, HIGH); // Deselect slave
  
  // Process received data
  int receivedValue = (response2 << 8) | response3;
  
  Serial.println("--- Received Data ---");
  Serial.print("Response 1 (Status): 0x");
  Serial.println(response1, HEX);
  Serial.print("Received Value: ");
  Serial.println(receivedValue);
  
  // Blink LED based on received value
  if (receivedValue > 100) {
    digitalWrite(LED_PIN, HIGH);
    delay(100);
    digitalWrite(LED_PIN, LOW);
  }
  
  delay(1000);
}

// SPI SLAVE CODE
// Upload this to the second Arduino (Slave)
//
// Connections (Slave <- Master):
// MOSI (11) <- MOSI (11)
// MISO (12) -> MISO (12)
// SCK (13)  <- SCK (13)
// SS (10)   <- SS (10)
// GND       <- GND

#include <SPI.h>

const int LED_PIN = 9;

volatile byte command = 0;
volatile byte dataHigh = 0;
volatile byte dataLow = 0;
volatile int receivedValue = 0;
volatile byte byteCount = 0;
volatile int dataToSend = 0;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  pinMode(MISO, OUTPUT); // Must set MISO as output for slave
  
  // Enable SPI in slave mode
  SPCR |= _BV(SPE); // Enable SPI
  SPCR |= _BV(SPIE); // Enable SPI interrupt
  
  Serial.begin(9600);
  Serial.println("SPI Slave Ready");
  
  // Initialize data to send
  dataToSend = 0;
}

void loop() {
  // Process received data
  if (byteCount >= 3) {
    receivedValue = (dataHigh << 8) | dataLow;
    
    Serial.println("\n--- Received from Master ---");
    Serial.print("Command: 0x");
    Serial.println(command, HEX);
    Serial.print("Value: ");
    Serial.println(receivedValue);
    
    // Control LED based on received value
    if (receivedValue > 512) {
      digitalWrite(LED_PIN, HIGH);
    } else {
      digitalWrite(LED_PIN, LOW);
    }
    
    byteCount = 0; // Reset for next transmission
  }
  
  // Update data to send back (e.g., counter or sensor reading)
  dataToSend++;
  if (dataToSend > 1023) dataToSend = 0;
  
  delay(10);
}

// SPI interrupt routine
ISR (SPI_STC_vect) {
  byte received = SPDR; // Read received byte
  
  if (byteCount == 0) {
    // First byte is command
    command = received;
    SPDR = 0x55; // Send status byte back
  } 
  else if (byteCount == 1) {
    // Second byte is high byte of data
    dataHigh = received;
    SPDR = highByte(dataToSend); // Send high byte back
  } 
  else if (byteCount == 2) {
    // Third byte is low byte of data
    dataLow = received;
    SPDR = lowByte(dataToSend); // Send low byte back
  }
  
  byteCount++;
}
```
- Functions of SPI library:
	- **Master:**
		- `SPI.begin()` - Initialize SPI as master
		- `SPI.setClockDivider(divider)` - Set clock speed
		- `SPI.transfer(data)` - Send and receive byte simultaneously
		- `digitalWrite(SS, LOW/HIGH)` - Select/deselect slave
	- **Slave:**
		- `SPCR |= _BV(SPE)` - Enable SPI in slave mode
		- `SPCR |= _BV(SPIE)` - Enable SPI interrupt
		- `ISR(SPI_STC_vect)` - Interrupt handler
		- `SPDR` - SPI data register (read received, write to send)

# Clock
- The **clock** is the **heartbeat** of the microcontroller — it defines _how fast_ the CPU and peripherals operate.
- On the Arduino UNO, the ATmega328P runs at 16 MHz
- This frequency is provided by an **external quartz crystal oscillator** connected to pins 9 and 10 of the MCU.
	- Each **CPU instruction** typically takes **1–2 clock cycles** (since AVR is a RISC architecture).
	- So the UNO executes roughly **8–16 million instructions per second**.
	- This 16 MHz signal also feeds into **timers**, **UART**, **I²C**, **SPI**, and other subsystems (sometimes divided by _prescalers_).

| Clock Source               | Typical Frequency                 | Accuracy | Notes                                   |
| -------------------------- | --------------------------------- | -------- | --------------------------------------- |
| **External crystal**       | 16 MHz (Arduino UNO default)      | ±0.005%  | Stable, good for serial comm            |
| **Internal RC oscillator** | 8 MHz (divided to 1 MHz or 2 MHz) | ±10%     | Lower accuracy, used in low-cost boards |
| **External clock input**   | user-provided                     | depends  | Used when synchronizing multiple MCUs   |
- All internal subsystems derive their timing from the same master clock:

| Subsystem          | How it uses the clock                                        |
| ------------------ | ------------------------------------------------------------ |
| **CPU**            | Executes instructions every cycle                            |
| **Timers**         | Count clock pulses (optionally prescaled)                    |
| **PWM**            | Generated from timer compare matches                         |
| **USART (Serial)** | Derives baud rate from main clock (e.g., 16 MHz / divider)   |
| **SPI**            | Clock generated from CPU clock divided by selectable factors |
| **I²C**            | SCL derived via clock divider registers                      |
| **Watchdog Timer** | Uses its _own_ separate internal oscillator (~128 kHz)       |
| **ADC**            | Needs a slower derived clock (typically ≤ 200 kHz)           |
- A **prescaler** divides the base clock frequency to generate slower “sub-clocks.”
- Example: 
	- Timer1 with prescaler = 64 →  `16 MHz / 64 = 250 kHz` counting rate (one tick = 4 µs).
	- `millis()` uses Timer0 configured this way — that’s why it increments every millisecond.
# Power management
- A very important functionality supported by most microcontrollers is power management, with the possibility to exploit low power consumption modality (sleep mode)
- ATMega328 has 5 sleep modes:
	- **Idle Mode** –> stopping the CPU but allowing SPI, USART, Analog Comparator, ADC, Two-wire Serial Interface, Timer/Counters, Watchdog, and the interrupt system to continue operating. This sleep mode basically halts clkCPU and clkFLASH, while allowing the other clocks to run.
	- **ADC Noise Reduction Mode** –> stopping the CPU but allowing the ADC, the external interrupts, the Two-wire Serial Interface address watch, Timer/Counter2 and the Watchdog to continue operating (if enabled). This sleep mode basically halts clkI/O, clkCPU, and clkFLASH, while allowing the other clocks to run
	- **Power-save Mode** –> This mode is identical to Power-down, with one exception: If Timer/Counter2 is clocked asynchronously, Timer/Counter2 will run during sleep. 
	- **Standby Mode** –> this mode is identical to Power-down with the exception that the Oscillator is kept running. From Standby mode, the device wakes up in 6 clock cycles 
	- **Power-down Mode** –> the External Oscillator is stopped, while the external interrupts, the Two-wire Serial Interface address watch, and the Watchdog continue operating (if enabled). Only an External Reset, a Watchdog Reset, a Brown-out Reset, a Two-wire Serial Interface address match interrupt, or an external level interrupt on INT0 or INT1, can wake up the MCU. This sleep mode basically halts all generated clocks, allowing operation of asynchronous modules only. – When waking up from Power-down mode, there is a delay from the wake-up condition occurs until the wake-up becomes effective. This allows the clock to restart and become stable after having been stopped.
![[Pasted image 20251018084609.png]]
- For interesting data consult: https://learn.sparkfun.com/tutorials/reducing-arduino-power-consumption
- Example in Arduino:
```cpp
#include <avr/sleep.h>
#include <avr/power.h>

const int LED_PIN = 13;
const int BUTTON_PIN = 2; // Must be an interrupt pin (2 or 3 on most Arduinos)

void setup() {
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Blink LED to show we're awake
  digitalWrite(LED_PIN, HIGH);
  delay(1000);
  digitalWrite(LED_PIN, LOW);
  
  // Attach interrupt to wake up from sleep
  // INT0 = pin 2, INT1 = pin 3
  attachInterrupt(digitalPinToInterrupt(BUTTON_PIN), wakeUp, LOW);
}

void loop() {
  // Blink LED to show activity
  digitalWrite(LED_PIN, HIGH);
  delay(100);
  digitalWrite(LED_PIN, LOW);
  delay(900);
  
  // After 5 blinks, go to sleep
  static int blinkCount = 0;
  blinkCount++;
  
  if (blinkCount >= 5) {
    blinkCount = 0;
    goToSleep();
  }
}

void goToSleep() {
  // Set sleep mode to POWER_DOWN (deepest sleep)
  set_sleep_mode(SLEEP_MODE_PWR_DOWN);
  
  // Disable ADC to save power
  power_adc_disable();
  
  // Enable sleep mode
  sleep_enable();
  
  // Enter sleep mode
  // The processor wakes up when the interrupt is triggered
  sleep_mode();
  
  // --- SLEEPING ---
  // Execution continues here after waking up
  
  // Disable sleep mode after waking
  sleep_disable();
  
  // Re-enable ADC
  power_adc_enable();
}

void wakeUp() {
  // This function is called when the interrupt occurs
  // Just waking up is enough, no code needed here
}
```
# LAB
- We need to specify board and the port where is connected while uploading program
- In the bootloader is contained inside the EEPROM
- CPU is working at 16 Hz, then it means that compiles million lines of instructions per second
- Serial line for debugging
- Several phases during compiling:
	- Open the connection with a certain protocol
- .ino is the extension of the file, underneath there are some c/cpp plugins
- With *serial monitor* with can increase/decrease the baud of our pc that is talking the Arduino chip
- TX blinks every time that receives data from the serial line
-  Sequence of actions to communicate with a slave: 
	- we set the slave select pin SS on LOW 
	- by means of a digitalWrite on pin SS 
	- we call SPI.transfer() to transfer the data
	- we set the slave select pin SS on HIGH