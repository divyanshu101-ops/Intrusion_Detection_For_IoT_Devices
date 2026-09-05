# Phase 0 — IoT Fundamentals

## 1. IoT Fundamentals

### What is IoT?

* Internet of Things (IoT) = physical devices connected to a network that can sense, process, communicate data, and/or perform actions.
* Examples: smart cameras, smart bulbs, temperature sensors, smart locks, industrial sensors.

### Key Characteristics

* **Resource constrained:** Limited CPU, RAM, storage, and power.
* **Always/continuously connected:** Devices frequently communicate with gateways, servers, or cloud services.
* **Heterogeneous:** Different hardware, software, operating systems, and communication protocols.
* **Large attack surface:** Weak passwords, vulnerable services, outdated firmware, and insecure communication can expose devices.

---

# 2. Basic IoT Architecture

```text
IoT Devices
(Sensors / Actuators)
        ↓
Gateway / Router
        ↓
Internet / Network
        ↓
Server / Cloud
        ↓
Application
```

### Main Components

**IoT Device**

* Physical device performing a specific function.
* May contain sensors, actuators, processor, memory, communication interface, and firmware.

**Sensor**

* Collects information from the physical environment.
* Examples: temperature, humidity, motion, light.

**Actuator**

* Performs an action based on commands.
* Examples: motor, lock, valve, light.

**Processor / Microcontroller**

* Processes sensor data.
* Runs device logic.
* Handles communication.

**Firmware**

* Software running on the IoT device that controls its hardware and functionality.

**Gateway / Router**

* Connects IoT devices to other networks.
* Forwards packets and may provide network/security functions.

**Server / Cloud**

* Stores and processes IoT data.
* Provides applications and device-management services.

---

# 3. How an IoT Device Works

General flow:

```text
Sensor
  ↓
Data Collection
  ↓
Local Processing
  ↓
Message Creation
  ↓
Communication Protocol
  ↓
Network
  ↓
Gateway / Router
  ↓
Server / Cloud
```

### Example — Smart Temperature Sensor

```text
Temperature Sensor
        ↓
Measures 28°C
        ↓
Microcontroller processes data
        ↓
MQTT message
        ↓
Wi-Fi
        ↓
Router
        ↓
MQTT Broker / Server
        ↓
Application receives data
```

---

# 4. IoT Communication

IoT devices communicate using different network protocols.

### Important protocols for this project

**Application Layer**

* MQTT
* HTTP / HTTPS
* DNS

**Transport Layer**

* TCP
* UDP

**Internet / Network**

* IP
* ICMP

**Link / Local Network**

* ARP

These protocols will be studied in more detail in later phases.

---

# 5. IoT Network Traffic

Whenever an IoT device communicates, it generates **network traffic**.

Example:

```text
IoT Device
    ↓
MQTT / HTTP
    ↓
TCP / UDP
    ↓
IP Network
    ↓
Gateway
    ↓
Server
```

Network traffic can contain observable characteristics such as:

* Packet count
* Packet size
* Flow duration
* Inter-arrival time
* Source IP
* Destination IP
* Source port
* Destination port
* Protocol information

These characteristics can later become **features in an IDS dataset**.

---

# 6. Normal vs Attacked IoT Device

### Normal Device

```text
IoT Device
    ↓
Normal Activity
    ↓
Normal Communication
    ↓
Normal Network Traffic
```

### Compromised / Attacked Device

```text
IoT Device
    ↓
Malicious Activity
    ↓
Changed Communication Pattern
    ↓
Abnormal Network Traffic
```

An IDS attempts to learn the difference between these behaviors.

---

# 7. IoT Security Connection

IoT devices can be vulnerable because of:

* Limited resources
* Weak/default passwords
* Vulnerable services
* Outdated firmware
* Insecure communication
* Large numbers of connected devices
* Heterogeneous environments

A compromised IoT device can potentially:

* Generate malicious traffic
* Scan other devices
* Attempt brute force attacks
* Participate in DDoS
* Communicate with malicious infrastructure

---

# 8. IoT Ecosystem

IoT should be viewed as an ecosystem rather than a single device.

```text
                  Internet
                     ↓
                Cloud Server
                     ↓
                  Gateway
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       Camera     Smart Bulb   Sensor
          ↓          ↓          ↓
       Traffic    Traffic     Traffic
```

An attacker may target:

* Individual IoT devices
* Multiple IoT devices
* Gateway/router
* Server
* Communication between devices

---

# 9. Most Important Project Connection

```text
IoT Device
    ↓
Performs Function
    ↓
Communicates Over Network
    ↓
Generates Network Traffic
    ↓
Attack Changes Traffic Behavior
    ↓
Traffic Features
    ↓
IDS Dataset
    ↓
Deep Learning Model
    ↓
Intrusion Detection
```

### Core Idea

> **IoT devices generate network traffic while performing their functions. Attacks can change the characteristics of this traffic, and these changes can be learned by a Deep Learning-based IDS.**

---

# 10. What We Need to Know From Phase 0

Before moving forward, be able to explain:

* What IoT is
* What an IoT device is
* Role of sensors and actuators
* Role of processor/microcontroller
* Role of firmware
* Role of gateway/router
* Role of server/cloud
* How an IoT device works
* How IoT devices communicate
* Why IoT devices generate network traffic
* How attacks can change network behavior
* How IoT network traffic connects to IDS

---

# 🔥 PHASE 0 IN ONE LINE

**IoT Device → Performs Function → Communicates → Generates Traffic → Attack Changes Traffic → IDS Detects the Change**
