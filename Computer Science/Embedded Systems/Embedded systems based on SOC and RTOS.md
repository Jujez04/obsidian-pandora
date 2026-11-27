- **System On a Chip**: chips featuring a CPU architecture and enough memory for hosting an OS (typically a real-time one)
- Examples: 
	- BROADCOM BCM2837 64bit ARMv8 Cortex A53 Quad Core (1.2Ghz) ->used in Raspberry Pi 3
	- ARM Sitara AM335x SoC -> including ARM Cortex-A8 processor and used in BeagleBone, Arduino Tre 
	- Texas Instruments OMAP3530
- A SoC integrates:
– one or multiple processors/cores
• including microcontrollers, microprocessors, DSP
– a memory module containing one or multiple blocks of
different types:ROM, RAM, EEPROM, FLASH
– a clock, timers
– connectors for standard interfaces, such
as USB, Ethernet, USART, I2C, SPI
– radio/network interfaces
– DAC and ADC 
![[Pasted image 20251127091446.png]]

## ESP 8266
- A tiny IOT SOC
![[Pasted image 20251127091634.png]]

## ESP32
![[Pasted image 20251127091708.png]]

- ESP32 is a series of low-cost, low-power system on a chip microcontrollers with integrated Wi-Fi and dual-mode Bluetooth.
- It is a successor of the ESP8266 SoC, created and developed by Espressif Systems
![[Pasted image 20251127091925.png]]

## Raspberry PI 5
![[Pasted image 20251127092016.png]]

# Embedded OS and Real-time OS (RTOS)
- Recall: Operating systems --> Slide 12-35
- Operating systems designed to be used in embedded systems
- General features:
	- compactness
	- efficiency
	- reliability
	- determinism/predictability -> not 
- Differently from desktop OS, typically a RTOS is designed to run **one** *application at a* **time**, but typically multi-threaded/multi-tasked
-  https://en.wikipedia.org/wiki/Comparison_of_realtime_operating_systems
![[Pasted image 20251127094801.png]]
- There are two main parts in which an RTOS is divided:
	- CPU-dependent -> it has to be very small, and also some parts are written in Assembly
	- CPU-independent -> Much larger
![[Pasted image 20251127094954.png]]![[Pasted image 20251127095010.png]]
![[Pasted image 20251127094544.png]]

## Real-time Embedded Systems
- they must react and manage events and inputs within specified time intervals
- they manage the execution of tasks/computations that have **deadlines**
- There are two main categories:
	- hard real-time -> deadlines are correctness requirements, they must be satisfied
	- soft real-time -> deadlines should be respected in normal conditions; deadlines can be violated in some cases and the violation is meant to be managed
## Determinism --
- Determinism is the most important aspect of real-time systems (hard one in particular)
- In this case it means that the system should be predictable:
// TODO 48-58

# Scheduling in RTOS
// Designing in terms of multiple tasks



## Scheduling algorithm
### Rate-Monotonic
- Based on fixed-priority (synchronous OS)

### Earliest deadline first(EDF)
- nearest deadline