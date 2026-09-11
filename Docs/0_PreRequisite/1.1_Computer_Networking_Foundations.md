# PHASE 1.1 — COMPUTER NETWORKING FOUNDATIONS

## Deep Learning Based Intrusion Detection for IoT Devices

---

# 1. Introduction to Computer Networking

## 1.1 What is a Computer Network?

A **computer network** is a collection of interconnected devices that communicate and exchange data using predefined communication protocols.

Examples:

* IoT device ↔ Router
* IoT device ↔ Server
* Computer ↔ Web server
* Camera ↔ Cloud server

For our project:

> IoT devices communicate through networks, and this communication generates network traffic that can later be analyzed to detect attacks.

Basic communication:

```text
IoT Device
     ↓
Network
     ↓
Gateway / Router
     ↓
Server / Cloud
```

---

# 2. Basic Communication Model

Whenever two devices communicate, we need to understand four things:

```text
WHO → WHERE → HOW → WHAT
```

### WHO

Who is sending the data?

→ Source device

### WHERE

Where is the data going?

→ Destination device

### HOW

Which communication mechanism/protocol is being used?

→ TCP / UDP / IP / MQTT / HTTP etc.

### WHAT

What data is being communicated?

→ Application data

Example:

```text
IoT Device
192.168.1.10:50000
       ↓
       ↓ TCP
       ↓
Server
192.168.1.50:1883
```

Here:

```text
Source IP       = 192.168.1.10
Source Port     = 50000
Destination IP  = 192.168.1.50
Destination Port= 1883
Protocol        = TCP
```

---

# 3. Network Protocol

A **network protocol** is a set of rules that determines how devices communicate.

Different protocols perform different jobs.

For this project, important protocols include:

### Application

* MQTT
* HTTP
* HTTPS
* DNS

### Transport

* TCP
* UDP

### Internet

* IP
* ICMP

### Network Access / Link

* Ethernet
* Wi-Fi
* ARP

We will study the IoT-specific application protocols in Phase 2.

---

# 4. TCP/IP Model

The TCP/IP model provides a conceptual structure for understanding network communication.

For this project, use the simplified four-layer model:

```text
┌─────────────────────────────┐
│ Application Layer           │
│ MQTT, HTTP, HTTPS, DNS      │
├─────────────────────────────┤
│ Transport Layer             │
│ TCP, UDP                    │
├─────────────────────────────┤
│ Internet Layer              │
│ IP, ICMP                    │
├─────────────────────────────┤
│ Network Access Layer        │
│ Ethernet, Wi-Fi, ARP        │
└─────────────────────────────┘
```

---

# 5. Application Layer

The Application Layer contains protocols used by applications to communicate.

Important examples:

* MQTT
* HTTP
* HTTPS
* DNS

Example:

```text
Temperature Sensor
       ↓
MQTT
       ↓
MQTT Broker
```

The application protocol defines how application-level messages are exchanged.

---

# 6. Transport Layer

The Transport Layer provides communication between applications/processes running on different devices.

Main protocols:

```text
TCP
UDP
```

It is responsible for mechanisms such as:

* Transporting application data
* Using ports
* Managing communication
* Reliability in TCP
* Connection behavior

---

# 7. Internet Layer

The Internet Layer is responsible mainly for addressing and routing packets between networks.

Important protocol:

**IP — Internet Protocol**

Example:

```text
Source IP
192.168.1.10

        ↓

Destination IP
192.168.1.50
```

ICMP also operates around the Internet Layer and is used for network control/diagnostic messaging.

---

# 8. Network Access Layer

This layer deals with communication over the local physical/network medium.

Examples:

* Ethernet
* Wi-Fi

Important concepts:

* MAC addresses
* Frames
* ARP

For our project, this layer is especially relevant when understanding local-network attacks such as ARP spoofing.

---

# 9. Encapsulation

When data moves down the networking stack, each layer adds its own information.

Suppose an IoT application wants to send:

```text
temperature = 28°C
```

Conceptually:

```text
Application Layer
        ↓
[Application Data]

Transport Layer
        ↓
[TCP Header + Application Data]

Internet Layer
        ↓
[IP Header + TCP Header + Data]

Network Access Layer
        ↓
[Frame Header + IP Packet + Other Frame Information]
```

This process is called:

**Encapsulation**

---

# 10. Decapsulation

At the receiving device, the reverse process occurs.

```text
Frame
  ↓
IP Packet
  ↓
TCP/UDP Segment or Datagram
  ↓
Application Data
```

This is called:

**Decapsulation**

---

# 11. Why Encapsulation Matters for IDS

Different parts of network communication expose different information.

A network-monitoring system may observe information such as:

* Source IP
* Destination IP
* Source Port
* Destination Port
* Protocol
* Packet Length
* TCP Flags
* Timing information

These observable characteristics can later become **features used by an IDS dataset**.

---

# 12. IP Address

An **IP address** identifies a network interface in an IP network.

Example:

```text
IoT Device
192.168.1.10

Server
192.168.1.50
```

Communication:

```text
192.168.1.10
      ↓
192.168.1.50
```

### Source IP

The IP address of the sender.

### Destination IP

The IP address of the receiver.

---

# 13. Private IP Addresses

Devices inside a local network commonly use private IP addresses.

Example:

```text
Router
192.168.1.1
   │
   ├── Camera → 192.168.1.10
   ├── Bulb   → 192.168.1.11
   └── Sensor → 192.168.1.12
```

These addresses are used inside the local network and are commonly seen in IoT environments.

---

# 14. MAC Address

A **MAC address** identifies a network interface at the link layer.

Example:

```text
AA:BB:CC:11:22:33
```

An IoT device can have both:

```text
IP Address:
192.168.1.10

MAC Address:
AA:BB:CC:11:22:33
```

Important distinction:

```text
IP
↓
Network-level addressing

MAC
↓
Local/link-level addressing
```

MAC addresses become particularly important when studying ARP and ARP spoofing.

---

# 15. Port

A **port** identifies a network service/application endpoint on a host.

A single server can run many services:

```text
Server
│
├── Web Server
├── MQTT Broker
├── SSH
└── DNS Service
```

Ports allow traffic to be directed toward the appropriate service.

Example:

```text
192.168.1.50:1883
```

Here:

```text
192.168.1.50 → IP address
1883         → Port
```

---

# 16. Common Ports Relevant to IoT/IDS

```text
SSH        → 22
HTTP       → 80
HTTPS      → 443
DNS        → 53
MQTT       → 1883
MQTT over TLS → 8883
```

Ports are useful for:

* Identifying services
* Detecting scanning
* Detecting unauthorized connections
* Understanding application traffic
* Analyzing attack behavior

---

# 17. Socket

A socket can be thought of as a communication endpoint.

A simplified representation is:

```text
IP Address + Port
```

Example:

```text
192.168.1.10:50000
```

Communication:

```text
192.168.1.10:50000
          ↓
192.168.1.50:1883
```

This allows us to identify:

```text
Source IP
Source Port
Destination IP
Destination Port
```

---

# 18. Packet

A **packet** is a unit of data transmitted through an IP network.

Simplified structure:

```text
┌─────────────────────────┐
│ IP Header               │
├─────────────────────────┤
│ Transport Header        │
├─────────────────────────┤
│ Application Data        │
└─────────────────────────┘
```

Depending on the protocol, a packet may contain information such as:

* Source IP
* Destination IP
* Protocol
* Packet length
* TTL
* Transport information

---

# 19. Frame

A **frame** is a data-link-layer unit used for local network communication.

Simplified:

```text
Frame
├── Link-layer header
├── IP packet
└── Link-layer information
```

Important distinction:

```text
Frame
↓
Link Layer

Packet
↓
Network/IP Layer
```

---

# 20. Segment and Datagram

Transport-layer data is commonly referred to as:

```text
TCP → Segment
UDP → Datagram
```

Conceptually:

```text
Application Data
       ↓
TCP → Segment
UDP → Datagram
       ↓
IP → Packet
       ↓
Network → Frame
```

Different tools and datasets may use these terms.

---

# 21. TCP

**TCP = Transmission Control Protocol**

TCP is a:

* Connection-oriented
* Reliable
* Ordered

transport protocol.

TCP provides mechanisms for:

* Establishing connections
* Reliable delivery
* Ordering data
* Acknowledgements
* Error handling
* Flow control

---

# 22. TCP Connection

TCP normally establishes a connection before transferring application data.

This is done using the:

**Three-Way Handshake**

```text
Client                     Server

  | -------- SYN --------> |
  | <----- SYN + ACK ----- |
  | -------- ACK --------> |
```

After this:

```text
Connection Established
        ↓
Data Transfer
```

---

# 23. TCP Flags

TCP contains control flags.

Important flags for this project:

### SYN

Used to initiate a TCP connection.

```text
SYN
↓
Request to establish connection
```

### ACK

Acknowledges received data/segments.

```text
ACK
↓
Acknowledgement
```

### FIN

Used for graceful connection termination.

```text
FIN
↓
Request to close connection
```

### RST

Resets/aborts a connection.

```text
RST
↓
Reset connection
```

### PSH

Indicates data should be pushed toward the receiving application.

---

# 24. Why TCP Flags Matter for IDS

TCP flags reveal information about connection behavior.

Example:

```text
SYN
SYN
SYN
SYN
SYN
SYN
...
```

A large number of SYN attempts can indicate suspicious behavior such as a possible **SYN Flood**.

Therefore:

```text
TCP Flags
    ↓
Connection Behavior
    ↓
Potential Attack Signal
```

---

# 25. UDP

**UDP = User Datagram Protocol**

UDP is:

* Connectionless
* Lightweight
* Lower overhead
* Without guaranteed delivery
* Without guaranteed ordering

Unlike TCP, UDP does not use a TCP-style three-way handshake.

Basic communication:

```text
Client
  |
  | UDP
  ↓
Server
```

UDP is important in IoT because many IoT applications can use lightweight communication, and UDP traffic is also relevant to flooding attacks.

---

# 26. TCP vs UDP

| Feature       | TCP                        | UDP                        |
| ------------- | --------------------------- | --------------------------- |
| Connection    | Connection-oriented        | Connectionless             |
| Reliability   | Reliable                   | No delivery guarantee      |
| Ordering      | Ordered                    | No ordering guarantee      |
| Handshake     | Yes                        | No TCP handshake           |
| Overhead      | Higher                     | Lower                      |
| TCP Flags     | Yes                        | No TCP flags               |
| IDS relevance | Connection behavior, flags | Traffic rate, volume, etc. |

The important project-level point:

> TCP and UDP generate different traffic patterns, and attacks against them can produce different observable characteristics.

---

# 27. Routing

When two devices are on different networks, packets may pass through multiple routers.

Example:

```text
IoT Device
     ↓
Router 1
     ↓
Router 2
     ↓
Router 3
     ↓
Server
```

Routers forward packets toward their destination.

IP addressing and routing therefore work together.

---

# 28. Complete Networking Picture

An IoT communication can be represented as:

```text
IoT Application
      ↓
MQTT / HTTP / DNS
      ↓
TCP / UDP
      ↓
IP
      ↓
Wi-Fi / Ethernet
      ↓
Router / Gateway
      ↓
Network
      ↓
Server
```

The resulting communication generates observable network traffic.

---

# 29. Networking Concepts Relevant to Our IDS

The most important networking concepts for this project are:

```text
IP Address
MAC Address
Port
Socket
Packet
Frame
TCP
UDP
TCP Handshake
TCP Flags
Routing
Protocols
```

These concepts provide the foundation for understanding network traffic and IDS datasets.

---

# 30. Phase 1.1 Core Connection

```text
IoT Device
     ↓
Application
     ↓
MQTT / HTTP / DNS
     ↓
TCP / UDP
     ↓
IP
     ↓
Wi-Fi / Ethernet
     ↓
Packets / Frames
     ↓
Network
```

This is the networking foundation required before analyzing traffic.

---

# 31. Phase 1.1 Goal

After 1.1, you should understand:

> **How information generated by an IoT application gets converted into network communication and travels between devices through different networking layers.**

The next part focuses on the **actual traffic generated by this communication**.

---

# 1.1 END
