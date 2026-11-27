## Fundamental Communication Architectures
- Communication among devices is a crucial aspect of M2M (Machine-to-Machine) and IoT systems, that is the capability to exchange data/ information among devices and subsystems 
- Communication can be either **wired** or **wireless**. 
- In both cases devices are equipped with suitable communication modules
	- in the wired case, these are shields/boards with an Ethernet port and support for **802.11** protocol 
	- in the wireless case, they could be either shield or USB dongle or event SoC including a transmitter/receiver module and the antenna 
- The communication between the microcontroller and the communication modules typically occurs through the serial port

Embedded devices in IoT connect to the Internet in two primary ways:

- **Indirectly:** Through an intermediary node acting as a **gateway** (e.g., ZigBee or Bluetooth architectures).
    
- **Directly:** Using dedicated modules like **Wi-Fi** or **cellular (3G/4G/5G)**.
    
- **Reference Protocols:** Regardless of the connection method, the standard reference protocols remain **IP, TCP/IP, and UDP/IP**.
    
![[Pasted image 20260131154651.png]]
## 2. Wireless Technologies by Range

Wireless protocols are categorized based on their effective transmission distance:

- **Short Range (~10m):** Bluetooth (IEEE 802.15.1) and Visible Light Communication (IEEE 802.15.7).
    
- **Medium Range (~100m to 1 mile):** Wi-Fi (IEEE 802.11) and WPAN (IEEE 802.15.4).
    
- **Long Range (> 1 mile):** Cellular networks (2G–5G) and Low-Power Wide-Area (LPWA) technologies.


## 3. Key IoT Network Protocols

### IEEE 802.15.4 (LR-WPAN)

- **Purpose:** Standard for low-speed, low-cost, and low-power personal wireless networks.
    
- **Specs:** Operates at 2.4 GHz, 900 MHz, and 868 MHz with data rates up to 250 kbps.
    
- **Implementations:** Forms the base for **ZigBee**, **6LoWPAN**, **Thread**, and **WirelessHART**.

### Bluetooth and BLE

- **Classic Bluetooth:** Designed for short-range data/voice with a master-slave architecture (Piconets).
    
- **Bluetooth Low Energy (BLE/Smart):** Specifically designed for IoT with significantly lower power consumption (0.01 to 0.5 W vs 1 W for Classic) and much lower latency (6 ms vs 100 ms).
#### **Overview & Characteristics**

Bluetooth is a wireless technology standard (IEEE 802.15.1) designed for short-range communication, primarily within a **Wireless Personal Area Network (WPAN)**. It is widely used in consumer electronics like smartphones, laptops, and wearables.

+2

- **Frequency:** It operates in the free **2.4 GHz** bandwidth. To minimize interference, it uses a **frequency hopping** technique, dividing the bandwidth into 79 channels and switching between them 1,600 times per second.
    
    +1
    
- **Objectives:** The standard focuses on low power consumption, low production costs, and connecting devices over short distances (typically a few dozen meters).
    

#### **Device Classes (Power & Range)**

Bluetooth devices are categorized into four classes based on their transmission power and effective range:

|**Class**|**Power (mW)**|**Range (approx.)**|
|---|---|---|
|**1**|100 mW|~100 m|
|**2**|2.5 mW|~10 m|
|**3**|1 mW|~1 m|
|**4**|0.5 mW|~0.5 m|

#### **Network Topology**

Bluetooth networks rely on a **Master-Slave** architecture.

+1

- **Piconet:** The basic network unit is called a piconet, where one Master device can manage up to **7 Slave devices** simultaneously. Communication is synchronized to the Master's clock.
    
    +2
    
- **Scatternet:** Multiple piconets can be connected to form a scatternet. A device can act as a slave in multiple piconets, or a master in one and a slave in another.
    
    +1
    

#### **Protocol Stack and Profiles**

The Bluetooth stack is organized into layers similar to the OSI model.

- **Key Protocols:**
    
    - **RFCOMM:** Emulates an RS-232 serial port, allowing for simple data stream transmission and broad device compatibility.
        
    - **SDP (Service Discovery Protocol):** Allows devices to discover services available on other devices.
        
    - **L2CAP:** Manages data segmentation and protocol multiplexing.
        
- **Profiles:** To ensure compatibility for specific use cases, Bluetooth defines "profiles" such as the **Serial Port Profile (SPP)**, **Headset Profile (HSP)**, and **File Transfer Profile (FTP)**.
    
    +2
    

#### **Evolution: Bluetooth Low Energy (BLE) & Bluetooth 5**

As IoT evolved, Bluetooth split into different versions to address energy constraints.

- **Bluetooth 4.0 (Bluetooth Smart / BLE):** Introduced **Bluetooth Low Energy (BLE)**, specifically designed for IoT. It features significantly lower power consumption, higher reactivity, and easier device communication compared to "Classic" Bluetooth.
    
    +1
    
    - **Dual-mode devices:** Support both Classic and BLE functionalities.
        
    - **Single-mode devices:** Implement only the BLE stack.
        
- **Bluetooth 5 (2016):** A further evolution for IoT that offers **4x the range** and **2x the speed** (up to 2 Mbps) compared to previous low-power versions, without increasing power consumption.
![[Pasted image 20260131154909.png]]
### 5G in IoT

- **Disruptive Features:** High speed (1-10 Gbps), ultra-low latency (1-10 ms), and high reliability.
    
- **IoT Optimization:** Designed to support "Massive IoT," allowing 10-100x more devices and up to 10 years of battery life for M2M (machine to machine) sensors.
    
![[Pasted image 20260131155004.png]]

### Matter

- **Overview:** A recent (2022) open-source connectivity standard backed by Amazon, Apple, and Google.
    
- **Goal:** To provide an interoperable standard that replaces proprietary smart home protocols.
    

---

## 4. Application Level Protocols: MQTT vs. CoAP

These protocols are designed specifically for constrained nodes and networks.

| **Feature**       | **MQTT**                                              | **CoAP**                                                  |
| ----------------- | ----------------------------------------------------- | --------------------------------------------------------- |
| **Transport**     | TCP (more overhead)<br><br>+1                         | UDP (connectionless/fast)<br><br>+1                       |
| **Model**         | **Publish/Subscribe** via a Broker                    | **Request/Response** (RESTful -> client/server)<br><br>+1 |
| **Communication** | Many-to-many<br><br>+1                                | One-to-one or Multicast<br><br>+1                         |
| **Security**      | SSL/TLS<br><br>+1                                     | DTLS<br><br>+1                                            |
| **Best Use Case** | Robust communication via cellular networks.<br><br>+2 | Constrained, lossy networks (LLNs).<br><br>+1             |
![[Pasted image 20260131154252.png]]