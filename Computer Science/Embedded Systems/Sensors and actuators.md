An embedded systems interact with external environment using sensors and actuators:
- **Sensors** –>
	- **transducer** **device** that allow to *measure* some *physical* *properties* (e.g., temperature, humidity, radiations, etc), or to detect or quantify chemical concentrations (e.g. gas) 
	- it provides a measurable representation of a phenomenon in some measuring scale or interval, e.g.: voltage 
	- can be *analog* (if the electrical value produced in output - e.g. voltage or current - is continuous) or *digital* (discrete set of values) • to convert analog signals, microcontrollers have a specific device included called analog-digital-converter (ADC) 
- **Actuators** – >devices that produce some effect on the environment • analog or digital (equipped of a DAC, Digital Analog Converter)

# Sensors
## Signals measured
Physical quantities measured by sensors can classified as:
- **continuous** -> temperatura, distance, angular speed,...
- **discrete** -> rotation direction, ...

## Information taxonomy carried by sensors
Sensors can be distinguished by the information they could compute:
- **Analog sensors** -> continuous electrical signal (in Wiring goes from 0 to 1023, around 10 bits of values)
- **Digital sensors**, and they can be divided into:
	- *logical signals* -> boolean output
	- *codified signals* -> string of bits

Analog and codified signals could be processed by a signal **sampling**:
![[Pasted image 20251021183238.png]]

And then the sampled signal can be approximated by **quantisation**:
![[Pasted image 20251021183340.png]]
## Typologies
- Proximity sensors 
- Movement detection sensors 
- Angular position transducers 
- Acceleration sensor 
- Contact sensors 
- Optical sensors 
- Electricity and magnetism sensors 
- Environmental parameter sensors 
- Physiological/vital parameter sensors 
- Sound sensors 
- Localisation sensors 
- Identity detection sensors
--> In general you have to know library + how it works (protocols)
### Proximity sensors
- Proximity sensors can **detect** the **presence** of some object at some distance 
- max distance -> *rated or nominal* distance or range, but it is rarely measured
- There are several technologies in which it is based on:
	- ultrasonic sensors (or sonars)
	- optical sensors
	- inductive sensors
	- magnet sensors
#### Ultrasonic Proximity sensors
- they emit an sonar impulse (240 KHz), and detect the echo back (like a bat) — in the case of an object in the nominal range:
	- Pros:
	- Cons
- Examples:
	- HC-SR04 sensor – datasheet and docs: https://docs.google.com/document/d/1YyZnNhMYy7rwhAgyL_pfa39RsB-x2qR4vP8saG73rE/edit

##### HC-SR04
![[Pasted image 20260121092028.png]]

```cpp
const int echoPin = 7;
/* supposing to have a temperature
 of 20 °C */
const float vs = 331.5 + 0.6*20;
void setup()
{
 Serial.begin(9600);
 pinMode(trigPin, OUTPUT);
 pinMode(echoPin, INPUT);
}
float getDistance()
{
 /* sending impulse */
 digitalWrite(trigPin,LOW);
 delayMicroseconds(3);
 digitalWrite(trigPin,HIGH);
 delayMicroseconds(5);
 digitalWrite(trigPin,LOW);

 /* receiving the eco */
 float tUS = pulseIn(echoPin, HIGH);
 float t = tUS / 1000.0 / 1000.0 / 2; // IMPORTANT d = vs*t / 2
 float d = t*vs;
 return d;
}

void loop()
{
 float d = getDistance();
 Serial.println(d);
 delay(200);
}
```
- `pulseIn` -> for receiving the frequency back
- Also time can be a huge factor
$$d = \frac{v_s \times dt}{2}$$
- **Operating Principle:** It works like a bat's echolocation. It emits a sonar impulse at **40 kHz** (ultrasound) and detects the echo returning from an object.
    
- **The Logic:**
    
    1. You send a **Trigger** signal (at least 10 microseconds).
        
    2. The sensor automatically sends a burst of 8 cycles at 40 kHz.
        
    3. The **Echo** pin goes high for a duration equal to the time the sound took to travel to the object and back.
#### Optical Proximity Sensors
- Based on light (also called photoelectric sensors) 
- Normally *infrared* light is used –> typically not influenced by environment light 
- **Pros/Cons:** They are faster than ultrasonic sensors but are strongly affected by the **surface orientation** and **color** of the object (dark objects absorb light and might not be detected)
- Operating principle based on **light reflection**
	- strongly affected by surface orientation
	- typical distance range: from 10 to 100 cm 
	- by mounting a reflex reflector on the object to be detected, the nominal range can arrive up to 50 m
- Sharp GP2Y0A41SK0F IR – infrared sensor http://www.alldatasheet.com/view.jsp?Searchword=Gp2y0a41sk0f

### Movement detection sensors
**Movement Detection** relies on two main categories of sensors: those that detect the _presence_ of movement (like alarms) and those that measure the _characteristics_ of the movement (like orientation or speed).

#### PIR - Passive Infrared Sensor
The **PIR** is the most common sensor for detecting motion (e.g., in burglar alarms or automatic lights).

- **How it works:** It is "Passive" because it does not emit a signal; it only measures the infrared radiation (heat) radiated by objects in its field of view.
    
- **The Logic:** It detects **abrupt variations** in temperature compared to a reference state. When a warm body (like a person) enters a zone that was previously colder (the background), the sensor triggers.
    
- **Limitation:** If an object has the same temperature as the background, the PIR will not detect it. It is crucial to calibrate it to establish the "normal" background state.

- Example:
	- PIR sensor – https://learn.adafruit.com/pir-passive-infraredproximity-motion-sensor/

### Angular Position Transducers

Two types:
- one detects the position without having any specific mechanical constraint, just exploiting the gravity force • e.g. inclinometer 
- one detects the position given a mechanical constraint => **encoder**: is an electromechanical device that converts the angular position of its rotating axis into electrical pulses, further processed and converted into a digital numerical signal

While PIR detects _if_ something moved, these sensors measure _how_ it moved.

- **Accelerometer:** Measures the acceleration of the object it is attached to. It can detect changes in speed or the force of gravity (static tilt).
    
- **Gyroscope:** Uses a rotating mechanism (or MEMS equivalent) to maintain a fixed direction, allowing it to measure angular velocity and orientation (often used as a compass).
- Example: 
	- MPU-6050 - accelerometer + gyroscope – http://playground.arduino.cc/Main/MPU-6050
#### MEMS - Micro Electro-Mechanical Systems
These modern sensors are based on **MEMS** (Micro Electro-Mechanical Systems) technology.

- These are microscopic mechanical structures (sized in micrometers) integrated directly onto silicon chips.
    
- They are essential for embedded systems because they allow complex mechanical measurements in a tiny, low-power package.
### Contact sensors
**Contact Sensors** are devices that require physical interaction or proximity to detect input. They are widely used for user interfaces (buttons, touch) or measuring mechanical forces.

#### Tactile Buttons and Microswitches

These are the simplest form of contact sensors. They operate as binary switches:

- **Function:** They simply close or open a circuit when pressed.
    
- **Usage:** Used for basic user input (on/off, start/stop).
    

#### Potentiometers (The "Knob")

A potentiometer is an electromechanical transducer that converts rotary or linear motion into a change in resistance.

- **Operating Principle:** It acts as a **voltage divider** with a variable resistance.
    
    - By turning the knob mechanically, you change the internal resistance ratio.
        
    - This changes the voltage level at the output terminal.
        
- **Linearity:** In a linear potentiometer, the output voltage is a direct linear function of the input voltage (and position).
    
- **Example in slides:** ALPS 10KOhm.
    ![[Pasted image 20260121095818.png]]

#### Capacitive Sensors (Touch)

These sensors do not necessarily require force, just contact (or very close proximity) with a conductive object (like a finger).

- **Operating Principle:** They detect changes in **electrical capacitance**.
    
- **How it works:** A nearby object (your finger) creates a capacitance that is detected by the internal circuits. This is the technology used in modern touchscreens and touch buttons.
    
- **Example in slides:** AllLife TTP223B.
    ![[Pasted image 20260121095832.png]]

#### Pressure Sensors (FSR)

Unlike buttons (which are on/off), these sensors measure _how hard_ you are pressing.

- **Type:** Force-Sensitive Resistor (FSR).
    
- **Function:** It measures the **force exerted** on the sensor surface. The resistance of the material changes depending on the pressure applied.
    
- **Example in slides:** Adafruit Round FSR.
![[Pasted image 20260121095848.png]]
### Optical sensors
**Optical Sensors** (or image sensors) are devices that convert an optical image or light into an electrical signal. They are fundamental in devices like digital cameras.

- **Classification:** They are broadly classified into sensors that detect **colors** and those that detect **black & white** (e.g., for night vision).
    
- **Mechanism:** An image is focused onto a grid of many small point-sensors (pixels), each converting light into a signal.

#### Photodetectors (Photocells)

These are specific devices that detect electromagnetic radiation and provide an output signal (current or voltage) proportional to the radiation's intensity.

The document lists several technologies:

- **Photoresistors:** Their operation is based on electrons in semiconductors being excited by light. arduino.cc/documents/datasheets/LDRVT90N2.pdf – resistance depends on the quantity of light perceived (inversely proportional) • strong light => small resistance

- **Photodiodes:** Based on photogenerated charge on a P-N junction.
    
- **Photovoltaic cells:** Similar to photodiodes but require no polarization.
    
- **CCD / CMOS:** Integrated circuits that use photogenerated charge in a semiconductor (used in cameras).
##### Photoresistor (LDR)
- **Behavior:** Its electrical **resistance depends on the quantity of light** perceived12.
    
    - **Strong Light** $\rightarrow$ **Small Resistance**13.
        
    - **Darkness** $\rightarrow$ **High Resistance**.
        
- **Wiring (Voltage Divider):** Since a microcontroller reads voltage, not resistance, you cannot connect an LDR directly to a pin. You must use a **voltage divider** circuit14141414.
    
    - Connect the LDR in series with a resistor of a known value (e.g., 1 kOhm or 10 kOhm)15.
        
    - Measure the voltage at the point between the LDR and the fixed resistor using an **Analog Pin** (e.g., A0)16.
![[Pasted image 20260121100122.png]]
### Current sensors for detecting electricity and magnetism
**Electricity and Magnetism Sensors** are designed to measure electrical properties or magnetic fields.

#### Electric Voltage and Current Sensors

These sensors are used to detect the electrical state of a circuit.

- **Function:** They detect the **voltage level** between two points or the **current** flowing through a circuit.
    
- **Application:** They are particularly useful for measuring the energy consumed by a device.
    
- **Example:** project http://www.openenergymonitor.org

#### Magnetometers
These sensors measure the **magnetic field** in the environment.
- **Types:**
    - **Scalar:** Measures only the _intensity_ of the magnetic field.
        
    - **Vectorial:** Measures the intensity along a specific _spatial direction_.

#### Compass
A specific application of magnetic sensing.
- **Function:** It measures the angle with respect to **magnetic north**.
- **Usage:** Essential for navigation and orientation in embedded systems (like robots or drones).

### Weather and Climate Sensors
**Weather and Climate Sensors** are used to monitor environmental parameters like temperature, humidity, pressure, and air quality.

#### Temperature Sensors
- **Analog Sensors (e.g., TMP36):**
    ![[Pasted image 20260121102834.png]]
    - **How they work:** These sensors output a voltage level that is directly proportional to the temperature.
        
    - **Interface:** You read the value using an **Analog Pin** (ADC) on the microcontroller.
        
    - **Wiring:** Typically requires 3 pins: Voltage In (2.7-5.5V), Ground, and Analog Voltage Out.
        
- **Digital Sensors (e.g., TC74):**
    ![[Pasted image 20260121102847.png]]
    - **How they work:** They convert the temperature internally and send digital data.
        
    - **Interface:** This specific model uses the **I2C** protocol (Serial Data SDA, Serial Clock SCL) to communicate with the microcontroller, rather than a simple voltage level.
        

#### Humidity and Pressure
- **Humidity Sensor (e.g., DHT11):**
    ![[Pasted image 20260121102901.png]]
    - **Function:** A very common digital sensor that measures both temperature and humidity.
        
- **Barometric Pressure (e.g., BMP085):**
    ![[Pasted image 20260121102914.png]]
    - **Function:** Measures atmospheric pressure. Since pressure changes with altitude, these are also frequently used as altimeters to determine height.
        

#### Sensors Detecting Chemicals (Air Quality)
- **Gas Sensors (e.g., MQ-2):**
    ![[Pasted image 20260121102930.png]]
    - **Function:** The MQ series (like the MQ-2) detects combustible gases and smoke.
        
    - **Mechanism:** They typically use a heating element inside; the resistance of the sensor changes when exposed to specific gases.
        
- **Dust Sensors (e.g., PM2.5):**
    ![[Pasted image 20260121102943.png]]
    - **Function:** Used to measure particulate matter (PM2.5) for air quality analysis.
### Sound sensors
**Sound Sensors** are primarily represented by microphones, which allow an embedded system to detect sound levels or capture audio.

#### Microphones
![[Pasted image 20260121102957.png]]
The document highlights **Electret Microphones** (specifically an amplifier breakout board from Adafruit) as the standard way to detect sound.

Example: Adafruit product 1713 is an electret microphone with an adjustable amplifier. The slides list these key specifications:

- **Adjustable Gain:** The sensitivity of the microphone can be changed. The example lists settings for **40dB, 50dB, and 60dB**.
    
- **Output:** It produces an analog signal with a maximum peak-to-peak voltage (**2Vpp max**). This means the wave fluctuates up and down within a 2-volt range.
    
- **DC Offset:** The signal is centered around **1.25V**.
    
    - _Why this matters:_ Microcontrollers (like Arduino) cannot read negative voltages. A raw audio wave goes positive and negative. The "DC Offset" lifts the entire wave up by 1.25V so that the negative parts of the sound wave are still positive voltages (0V to 2.5V) readable by the Analog Pin.

### Localization
**Localization** is divided into two main categories: **Global** (knowing where you are on Earth) and **Local** (knowing where you are relative to nearby objects or beacons).

#### Global Localization: GPS

The **Global Positioning System (GPS)** is the standard for outdoor navigation.

- **How it works:** It relies on a network of **32 satellites** managed by the USA. These satellites constantly broadcast their coordinates and the precise time.
    
- **The Principle:** It uses **trilateration**. By measuring the time it takes for signals to travel from the satellites to the receiver, the system calculates the distance.
    
- **Requirement:** To get a valid position fix, the receiver must "see" at least **4 satellites**.
    
- **Accuracy:** Typically in the range of **meters**. It is affected by weather conditions and the quality of the receiver.
    
- **Example Module:** The slides mention the **Parallax PAM-7Q** and **uBlox** modules.
    

#### Local Localization Technologies

For indoor environments or short-range interactions where GPS doesn't work well, other technologies are used:

- **RFID (Radio Frequency Identification):** Uses small tags that store data. A reader can read/write this data from a distance ranging from centimeters to meters.
    
    - _Example:_ AZDelivery Kit RFID RC522.
        
- **NFC (Near Field Communication):** Similar to RFID but designed for very short distances or contact (e.g., contactless payments).
    
- **iBeacon (Bluetooth Low Energy - BLE):** Devices that transmit a unique ID (UUID) using Bluetooth. A receiver estimates proximity based on the signal strength.
    
- **UWB (Ultra Wide Band):** A technology that offers very high accuracy (down to 10cm) but is significantly more expensive.
    

#### Cost vs. Accuracy Trade-off
- **High Cost / High Accuracy:** Camera-based systems, RTK GPS, and LIDAR.
    
- **Mid Cost / High Accuracy:** UWB (Pozyx).
    
- **Low Cost / Low Accuracy:** Standard GPS, WiFi, and BLE (Accuracy around 3-5 meters).

# Actuators
Before connecting an actuator, you must determine how much power it needs. The document distinguishes two main cases:

- **Low Power (Direct Drive):** If the device needs very little current (e.g., standard **LEDs** or **Piezo buzzers**), the microcontroller's GPIO pin can power it directly (usually 5V and around 20mA).
    
- **High Power (External Drive):** If the device needs more voltage or current than the microcontroller can provide (e.g., **Motors**, **High-power LEDs**, or **AC appliances**), you must use an external power circuit.
    
    - In this case, the GPIO pin acts only as a **signal** to open/close a switch (implemented via **Transistors** or **Relays**).

From an electronic perspective, it is critical to know what kind of "load" the actuator represents:

- **Resistive Loads:** Simple devices that resist current (e.g., heating elements, incandescent bulbs). They are easier to drive.
    
- **Inductive Loads:** Devices that use magnetic fields, like **motors** and **solenoids**.
    
    - **Warning:** These are dangerous for microcontrollers because when they stop, they generate an "inverse current" (back EMF). You **must** use a protection diode to prevent frying your board.
### Visual Output Actuators

- **LEDs (Light Emitting Diodes):** Efficient, reliable, and low power. They generate photons when current flows through a semiconductor.
    
- **LCD Displays:** The document highlights the **16x2 Parallel LCD** screen.
    
    - It uses liquid crystals trapped between glass surfaces.
        
    - It typically requires 6 digital pins to control (4 for data, 2 for control) plus power, though **I2C** versions exist to save pins.

## Electric motors
- Electric motors are electric machines that convert the input electric power into an output effect which is mechanical
- An electric motor has two main parts
	- stator -> fixed/static part
	- rotor -> moving part, including the shaft
- these two components generate a magnetic field that causes the rotor to move
- Types of electric motors used in embedded:
	1. **DC Motors:** Good for continuous rotation and high speed (e.g., robot wheels).![[Pasted image 20260121104727.png]]
	2. **Stepper Motors:** Rotation is divided into precise steps, allowing for accurate positioning without sensors (e.g., 3D printers).    ![[Pasted image 20260121104740.png]]
	3. **Servo Motors:** Allows precise control of the shaft angle, usually limited to 180° (e.g., robotic arms)![[Pasted image 20260121104755.png]]
###  Servo motors
Servo motors can be controlled by specifying the angle of the shaft – between -90° and 90° • 

On the device: 3 wires 
- 2 for power (nero => GND, rosso => +5V) 
- 1 for digital control 
- typically white or yellow 

How to control a servo motor 
- The control is done by sending a stream of digital pulses at a specified frequency • typically 50 pulsed per second (50 Hz) 
- the duration of the pulses determines the rotation angle • pulses typically go from 1 ms (-90) to 2 ms (+90) 
- the larger is the pulse, the larger is the angle • linear scale, so 1.5 ms pulse => 0 degree •
- actually the real duration can greatly vary from servo to servo (to be calibrated/checked)

![[Pasted image 20260121105009.png]]

```cpp
int servoPin = 2;
void pulseServo(int servoPin, int pulseLen)
{
 digitalWrite(servoPin, HIGH);
 delayMicroseconds(pulseLen);
 digitalWrite(servoPin,LOW);
 delay(15);
}
int c;
enum { MINUS_90, PLUS_90 } state;
void setup()
{
 pinMode(servoPin, OUTPUT);
 state = MINUS_90;
 Serial.begin(9600);
 c = 0;
}
void loop(){
 c++;
 switch (state) {
 case MINUS_90:
 pulseServo(servoPin, 250);
 if (c > 100){
 Serial.println("--> +90");
 state = PLUS_90;
 c = 0;
 }
 break;
 case PLUS_90:
 pulseServo(servoPin, 2250);
 if (c > 100){
 Serial.println("--> -90");
 state = MINUS_90;
 c = 0;
 }
 }
}
```

- `pulseServo` sends a pulse of the specified duration and then waits for 15 ms 
- actually, to have 50 Hz, the waiting time should be 1000/50 = 20ms 
- the difference is for taking into account the instruction execution time and the pulse time empirical value
- Empirical tests for this servo – pulse duration for -90 is about 250 us (0.25 ms), while +90 is 2250 us (2.25 ms)

![[Pasted image 20260121104655.png]]

**Working with actuators: servo motors**

- Key aspect
    - precise control of shaft position, between 0 and 180 degree
- Can be analog or digital
    - Analog one (in the kit): 3 terminals – 2 for power and one for control
    - Example in the kit: [microservo Hitech HS-53](https://www.servocity.com/hitec-hs-53-servo)
- Control through PWM
    - periodic digital signal with pulses between 1 ms and 2 ms, ~50 Hz frequency (~20 ms period)
    - duty cycle => angle
        - common behaviour: ~1ms pulse => servo at one extreme (0 degree, counterclockwise), 1.5ms pulse => 90 degree and 2ms pulse => 180 degree, clockwise
        - different servos => different behaviours – check with the datasheets and empirical tests,
- Physical properties
    - Power
        - voltage level: between 4,5V and 6V
            - better to use an external powerline, feasible to use the Arduino one
        - "impulse current" => the current needed depends on the load
            - about ~5-10mA with no load or small load
            - can raise up to ~1A for heavy loads or external forces that try to move the shaft
        - "stall current": current needed to keep the current position - proportional to the effort/force exerted to keep the position
    - Drive torque  
        = capability of the servo to move the shaft in spite of the resistance of the load/apparatus which is mechanically bound to it
        - Computed as radius * force - in Newton * meter (Nm) or Kg meter (Kgm), where 1 Kgm = 9.81 Nm - from 2° principle of dynamics
        - available value depends on the voltage level of the battery
            - the greater the voltage level is, the greater is the drive torque
        - when receiving a command to rotate of some angle X, the shaft will actually rotate only if drive torque is capable to overcome the resistance due to the load
    - Rotation speed (or, duration of transition time for reaching the position)
        - duration of rotation = time to reach the position.
        - check datasheets and manuals
            - info typically given considering no load: e.g. - 0,2 sec / 60°
            - with a resistance load => higher
# Examples


# Interfacing components using different voltage levels


# Useful links
- https://www.adafruit.com/category/35 -> market
- http://wiring.org.co/learning/libraries -> Wiring libraries
- https://create.arduino.cc/projecthub -> Arduino project hub









# SI - Measurement system used

## Basic units of measurement
![[Pasted image 20251021183519.png]]

## Derived units
![[Pasted image 20251021183543.png]]

## Prefixes
![[Pasted image 20251021183604.png]]

## Uncertainty

• Multiple measurements of the same parameter typically don’t produce the same values – various causes, related to the measurement tools, environments • To describe this dispersion of values, the concept of measurement uncertainty is introduced – can be interpreted as the level of confidence with respect to the result, – a physical quantity can be determined only at some specified finite level of uncertainty (i.e. the error) • uncertainty quantifies the imprecision or inaccuracy of the measurement • The measurement of some physical quantity: value ± uncertainty (error) and unit of measurement

## Measurement errors
• Systematic errors – an error is systematic if, once the experimental conditions are fixed, it systematically impacts in the same way (same quantity and sign) on the measurements • Random (accidental) errors – errors whose influence (quantity and sign) can change as the measurements are repeated – environment conditions can be the source of these errors • “Gross” errors – errors done by either the operator or some failure of the measuring instruments


• Static characteristics – about operating conditions in which the input varies slowly, measuring then the output • Accuracy, Precision, Linearity, Sensitivity, Resolution, Repeatability, Reproducibility, Stability,… • Dynamic characteristics – about operating conditions in which the input varies fast, to check possible output attenuations and delay • the dynamic characteristics of a sensor could be critical since it may constraint the whole bandwidth of the whole control system • response time

## Metrological Characteristics
Static characteristics – about operating cond
itions in which the input varies slowly, measuring then the output • Accuracy, Precision, Linearity, Sensitivity, Resolution, Repeatability, Reproducibility, Stability,… • Dynamic characteristics – about operating conditions in which the input varies fast, to check possible output attenuations and delay • the dynamic characteristics of a sensor could be critical since it may constraint the whole bandwidth of the whole control system • response time

## Accuracy and precision

## Calibration

## Dynamic behaviour