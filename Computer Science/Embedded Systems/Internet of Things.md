The **Internet of Things (IoT)** is defined as the connection of physical objects—which are not operated by humans—to the Internet.

- **Core Components:** It consists of physical objects, controllers/sensors/actuators, and Internet connectivity.
    
- **The "Things":** These are typically **embedded systems** equipped with sensors to digitize the physical world in real-time, avoiding manual, error-prone data entry.
    
- **Key Traits:** IoT functions as a "System-of-Systems" that requires interoperability, security (authentication and authorization), and strict data privacy .
    

---

## Why IoT is Important Today

IoT has moved beyond technical novelty to become a pillar of the **fourth industrial revolution (Industry 4.0)**.

- **Societal Impact:** It digitizes the world by connecting people, processes, data, and things.
    
- **Business Transformation:** It enables "Precision" services in medicine and agriculture by integrating IoT with AI and Machine Learning to learn from collected data.

- **Economic Impact:** It drives efficiency in supply chains and business processes through real-time data collection and big-stream technologies.


---

## Evolution of IoT

The evolution is categorized into five distinct stages of product development:

1. **Product Stage:** A basic mechanical item (e.g., an air conditioner).
    
2. **Smart Product:** The item becomes programmable.
    
3. **Smart Connected Product:** The device is accessible via the Internet for remote control and predictive maintenance .
    
4. **Product Systems:** Different devices (thermostats, windows, floors) talk to each other through interoperability standards.
    
5. **System of Systems:** Large-scale integration where home appliances, cars, and smart hospitals communicate in a unified ecosystem.
    

---

## Data Flow and Implementation (I-IoT)

In Industrial IoT (I-IoT), data moves through a specific functional stack from the factory floor to the cloud:

- **The Field Level:** Sensors and **RTUs** (Remote Terminal Units) collect data and convert it into digital signals.
    
- **The Controller Level:** **PLCs** (Programmable Logic Controllers) supervise data collection and control sensors using specific languages like Ladder Diagrams or Structured Text .
    
- **The Edge:** An **Edge Device** acts as a middleman, linked to industrial sources via **OPC Servers** and to the cloud via **IoT Gateways**.
    
- **Supervision:** **SCADA** (Supervisory Control and Data Acquisition) systems monitor large-scale industrial processes across multiple nodes.


---

## Impact on Cloud Systems

IoT and the Cloud are deeply interdependent, as the volume and velocity of sensor data (Big Data) often cannot be stored locally.

- **Cloud Services:** Primarily used for data streaming, device management, and offline/online analysis.
    
- **Service Levels:** Resources are provisioned as **IaaS** (virtualized hardware), **PaaS** (execution runtimes), or **SaaS** (end-user applications) .
    
- **Universal Access:** The cloud enables ubiquitous, on-demand network access to shared computing resources.
    

---

## The Web of Things (WoT)

The **Web of Things** is the application-level layer of IoT.

- **Integration:** It brings IoT devices into the World Wide Web by giving each "thing" a **RESTful API**.
    
- **Interoperability:** It allows developers to interact with physical objects using standard web technologies (HTTP, JSON, WebSockets) regardless of the underlying low-level protocol (Bluetooth, ZigBee, etc.).
    
- **Simplicity:** It makes it easier to program and maintain large systems by treating "things" like any other web resource.