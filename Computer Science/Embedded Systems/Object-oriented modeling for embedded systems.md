- There are two main approaches:
	- **top-down approach** -> from the *domain* using high-level paradigms (like OO)
	- **bottom-up approach** -> from the *hardware* and related concepts

# Top down approach
- **modelling** -> representing relevant aspects of a system, that concerns the *structure*, *behaviour* and *interaction*
-  For modelling we can use:
	- **paradigms** -> OOP
	- **languages** -> UML
- In embedded systems we can consider two main categories:
	- **controller** -> encapsulating control/application logic, accessing/observing passive resources and devices
	- **controlled** **elements** -> modelling the resources and devices managed/used by the controller to do its job, encapsulates functionalities useful for the controller and passive components
- Metamodel:
	- **Controller** :
		- Integrating pro-active and re-active behaviour –
		- based on a control loop architecture -> super loop and  event-loop
	- Controlled elements:
		- well-defined interface 
		- observable state and events 
		- reusability, controllability 
		- OO model
Examples:
![[Pasted image 20251030092700.png]]
```cpp
– Button
	• isPressed(): boolean
– Led
	• switchOn()
	• switchOff()
	
lightOn := false
loop {
 /* first: sensing */
 bool isPressed = button.isPressed();

 /* processing and acting */
 if (!lightOn && isPressed){
	light.switchOn()
	lightOn := true
 } else if (lightOn && !isPressed){
	light.switchOff()
	lightOn := false
 }
}

// Light.h 
#pragma once
class Light {
public:
 virtual void switchOn() = 0;
 virtual void switchOff() = 0;
};

//Led.h
#include "Light.h"
class Led: public Light {
public:
 Led(int pin);
 void switchOn();
 void switchOff();
protected:
 int pin;
};

// Led.cpp
#include "Led.h"
#include "Arduino.h"
Led::Led(int pin){
 this->pin = pin;
 pinMode(pin,OUTPUT);
}
void Led::switchOn(){
 digitalWrite(pin,HIGH);
}
void Led::switchOff(){
 digitalWrite(pin,LOW);
};

// Button.h
#pragma once
class Button {
public:
 virtual bool isPressed() = 0;
};

// ButtonImpl.h
#pragma once
#include "Button.h"
class ButtonImpl: public Button {
public:
 ButtonImpl(int pin);
 bool isPressed();
protected:
 int pin;
};

//ButtonImpl.cpp
ButtonImpl::ButtonImpl(int pin){
 this->pin = pin;
 pinMode(pin, INPUT);
}

bool ButtonImpl::isPressed(){
 return digitalRead(pin) == HIGH;
}

//Main.cpp
#include "Led.h"
#include "ButtonImpl.h"
#define LED_PIN 13
#define BUTTON_PIN 2
Light* light;
Button* button;
boolean lightOn;
void setup(){
 light = new Led(LED_PIN);
 button = new ButtonImpl(BUTTON_PIN);
 lightOn = false;
}
void loop(){
 bool isPressed = button->isPressed();
 if (!lightOn && isPressed){
 light->switchOn();
 lightOn = true;
 } else if (lightOn && !isPressed){
 light->switchOff();
 lightOn = false;
 }
}
```
![[Pasted image 20251030094800.png]]
```cpp
//LightExt.h
#pragma once
class LightExt : public Light {
public:
 virtual void setIntensity(int) = 0;
};

//LedExt.h
#pragma once
#include "Led.h"
#include "LightExt.h"
class LedExt: public LightExt, public Led {
public:
 LedExt(int pin);
 LedExt(int pin, int intensity);
 void switchOn();
 void switchOff();
 void setIntensity(int v);
private:
 int currentIntensity;
 bool isOn;
};

//LedExt.cpp
#include "LedExt.h"
#include "Arduino.h"
LedExt::LedExt(int pin) : Led(pin) {
 currentIntensity = 128;
 isOn = false;
}
LedExt::LedExt(int pin, int intensity) : Led(pin) {
 isOn = false;
 currentIntensity = intensity;
}
void LedExt::switchOn(){
 analogWrite(pin,currentIntensity);
 isOn = true;
}
void LedExt::setIntensity(int value){
 currentIntensity = value;
 if (isOn){
 analogWrite(pin,currentIntensity);
 }
}
void LedExt::switchOff(){
 analogWrite(pin,0);
 isOn = false;
}

//Main.cpp
#include "LedExt.h"
// the pin must by a PWM one
#define LED_PIN 9
LightExt* led;
int brightness;
int fadeAmount;
void setup(){
 brightness = 0;
 fadeAmount = 5;
 led = new LedExt(LED_PIN,brightness);
 led->switchOn();
}
void loop(){
 led->setIntensity(brightness);
 brightness = brightness + fadeAmount;
 if (brightness == 0 || brightness == 255) {
 fadeAmount = -fadeAmount ;
 }
 delay(20);
}
```
Smart Led:
![[Pasted image 20251030095011.png]]
```cpp
lightOn := false
loop {
 detected := presDetector.detected()
 intens := lightSensor.getIntensity()
 isLowIntens := intens < LIGHT_THRESHOLD
 if (!lightOn && detected && isLowIntens){
	 light.switchOn()
	 lightOn := true
 } else if (lightOn && (!isLowIntens || !detected)){
	light.switchOff()
	lightOn := false
 }
}
```
- With OOP we can trace the state of LEDs, like pretending that we are seeing from an higher level point of view.
## Control loop and finite state machines
- Embedded systems has an *execution model* based on a **control loop**
- At each cycle the controller decides the proper action to perform, and the decision policy is based on the *current state* (changed by the action chosen)
- For this purpose we can use **Finite State Machines (FSM)** and they could be *synchronous* or *asynchronous*.
- This is a matter of *efficiency* and *reactivity* -> mainly implemented with patterns, for instance [[Observer pattern]]
- Problem with reactivity:
```cpp
/*
“Implement a controller for making a continuous fade in and fade out
of a led, with duration DT seconds, using a button to start and stop
asynchronously the process” 
*/
running := false
loop {
	 if (button.switchedOn() &&
	 !running){
	 running := true
	 light.switchOn()
	 } else if (button.switchedOff()
	 && running){
	 running := false
	 light.switchOff()
	 }
	 if (running){
	 fadeIn()
	 fadeOut()
	 }
}

fadeIn(){
 intensity := 0
 delta := 1/NUM_STEPS;
 for (i := 0; i < NUM_STEPS; i++){
 intensity := intensity + delta
 light.setIntensity(intensity)
 sleep(DeltaTime)
 }
}
fadeOut(){...}

/*
PROBLEM: if the button changes during the fade in/out, the controller does not detect
*/
```

### Controller model: Agents
- We will use the concept of “**agent**” to model controllers
- agents are first-class active *entities*, equipped with an autonomous logical *control* *flow*, designed to achieve or perform some *task*(s) that call for being reactive to data and events perceived from the environment (through sensors), and that do actions to have an effect on the environment though some actuators
![[Pasted image 20260122092350.png]]
- Complex jobs can be decomposed into tasks, that are executed by the control loop 
	- the agent carries on one or multiple tasks concurrently 
	- the behaviour of each task can be described by a state machine
- Complex embedded systems can be distributed and actually include a system of interacting/cooperating independent sub-systems 
- e.g. a smart home, a smart city or Internet of Things (IoT) perspective 
- In order to model these systems we can consider multiagent systems, as ensemble of agents that interact and cooperate