# PHASE 2 — IoT COMMUNICATION PROTOCOLS

## Deep Learning Based Intrusion Detection for IoT Devices

---

# 2.1 MQTT — Message Queuing Telemetry Transport

## What is MQTT?

MQTT is a lightweight **application-layer messaging protocol** designed especially for environments such as IoT where:

* Devices may have limited resources
* Network bandwidth may be limited
* Efficient communication is required
* Many devices need to communicate through a central service

MQTT follows a **Publish/Subscribe architecture**.

Basic model:

```text
IoT Device
    ↓
  MQTT
    ↓
MQTT Broker
    ↓
Other IoT Devices / Applications
```

---

# MQTT Architecture

Three important components:

```text
Publisher
Subscriber
Broker
```

## Publisher

The device/application that publishes a message.

Example:

```text
Temperature Sensor
        ↓
     Publisher
        ↓
temperature = 28°C
```

## Subscriber

The device/application interested in receiving messages.

```text
Mobile Application
        ↑
     Subscriber
```

## Broker

The central MQTT server that receives published messages and distributes them to subscribers.

```text
Publisher
    ↓
 Broker
    ↓
Subscriber
```

The publisher and subscriber normally communicate through the broker rather than directly.

---

# MQTT Example

```text
Temperature Sensor
        ↓
     Publisher
        ↓
      Broker
        ↓
   Mobile App
     Subscriber
```

Sensor publishes:

```text
Topic:
home/temperature

Message:
28°C
```

Mobile application subscribes to:

```text
home/temperature
```

The broker receives the message and forwards it to subscribers.

---

# MQTT Topic

A **topic** is a named channel used to organize MQTT messages.

Examples:

```text
home/temperature
home/humidity
home/livingroom/light
factory/machine1/status
```

Publisher:

```text
Topic:
home/temperature

Message:
28°C
```

Subscriber subscribes to the topic.

---

# MQTT Message Flow

```text
Temperature Sensor
       |
       | PUBLISH
       ↓
MQTT Broker
       |
       | delivers message
       ↓
Mobile App
```

Basic model:

```text
Publisher → Broker → Subscriber
```

---

# MQTT Ports

Standard MQTT:

```text
1883
```

MQTT over TLS:

```text
8883
```

Therefore:

```text
Destination Port = 1883
```

may indicate MQTT traffic.

However:

> A port number alone does not prove the actual application protocol.

---

# MQTT QoS — Quality of Service

MQTT provides three QoS levels.

## QoS 0 — At Most Once

```text
Message
   ↓
Send once
```

No guarantee that the message arrives.

## QoS 1 — At Least Once

The message is delivered at least once.

Duplicates can occur.

## QoS 2 — Exactly Once

Provides the strongest delivery assurance among MQTT QoS levels, with additional protocol overhead.

General idea:

```text
QoS 0 → Lowest overhead
QoS 1 → Higher reliability
QoS 2 → Highest delivery assurance / overhead
```

QoS can influence MQTT traffic behavior.

---

# MQTT CONNECT

A client establishes a session with the broker.

```text
IoT Device
    |
    | CONNECT
    ↓
MQTT Broker
    |
    | CONNACK
    ↓
IoT Device
```

---

# MQTT PUBLISH

A publisher sends a message to a topic.

```text
Client
   |
   | PUBLISH
   ↓
Broker
```

Example:

```text
Topic:
sensor/temp

Payload:
28
```

---

# MQTT SUBSCRIBE

A client requests messages from a topic.

```text
Client
   |
   | SUBSCRIBE
   ↓
Broker
```

The broker sends an acknowledgement.

---

# MQTT Communication Sequence

A simplified MQTT sequence can be:

```text
Client → CONNECT → Broker
Client ← CONNACK ← Broker

Client → SUBSCRIBE → Broker
Client ← SUBACK ← Broker

Publisher → PUBLISH → Broker

Broker → PUBLISH → Subscriber
```

---

# MQTT Control Packets

Important MQTT control packets:

```text
CONNECT
CONNACK

PUBLISH

SUBSCRIBE
SUBACK

UNSUBSCRIBE
UNSUBACK

PINGREQ
PINGRESP

DISCONNECT
```

For this project, especially understand:

```text
CONNECT
PUBLISH
SUBSCRIBE
DISCONNECT
```

---

# MQTT Keep Alive

MQTT clients can maintain communication with a broker using a keep-alive mechanism.

Client sends:

```text
PINGREQ
```

Broker responds:

```text
PINGRESP
```

This helps maintain/check whether the connection is still active.

---

# MQTT Security

Basic MQTT communication on port 1883 does **not inherently provide encryption**.

MQTT can be secured using TLS:

```text
MQTT
  ↓
TLS
  ↓
Encrypted Communication
```

Common secure MQTT port:

```text
8883
```

Other security mechanisms include:

* Authentication
* Authorization
* Access control
* Certificates
* TLS encryption

---

# MQTT and IDS

MQTT generates traffic such as:

```text
CONNECT
PUBLISH
SUBSCRIBE
PING
DISCONNECT
```

Normal behavior might look like:

```text
Few connections
Normal publish rate
Normal message frequency
Normal traffic volume
```

Attack behavior may look like:

```text
Many connections
Very high publish rate
Abnormal packet frequency
Unusual traffic volume
```

Therefore:

```text
MQTT Activity
      ↓
Network Traffic
      ↓
Traffic Features
      ↓
IDS
```

---

# MQTT Attacks to Know Later

These will be studied properly in the attack phase:

* MQTT brute-force attacks
* MQTT flooding
* MQTT connection flooding
* Unauthorized publishing
* Unauthorized subscription
* Topic abuse
* MQTT-based DoS
* Malicious MQTT traffic

Important concept:

```text
Attack
   ↓
Changed communication behavior
   ↓
Changed traffic pattern
   ↓
Changed network features
```

---

# 2.2 HTTP — HyperText Transfer Protocol

HTTP is an **application-layer protocol** commonly used for communication between clients and servers.

Basic model:

```text
Client
   |
   | HTTP Request
   ↓
Server
   |
   | HTTP Response
   ↓
Client
```

---

# HTTP Request

The client sends a request.

Example:

```text
GET /temperature
```

The server processes the request.

---

# HTTP Response

The server sends a response.

Conceptually:

```text
HTTP Response
     ↓
Status
Headers
Data
```

Example:

```text
200 OK
temperature = 28
```

---

# HTTP Ports

Standard HTTP:

```text
80
```

HTTPS:

```text
443
```

---

# HTTP in IoT

An IoT device may communicate with a server through an API.

```text
IoT Device
    ↓
HTTP POST
    ↓
Cloud API
```

Example:

```text
POST /sensor-data

temperature = 28
humidity = 70
```

---

# HTTP and IDS

HTTP traffic can provide observable characteristics such as:

* Request rate
* Response rate
* Destination port
* Flow duration
* Packet sizes
* Connection patterns
* Traffic volume

Potential attacks include:

* HTTP flooding
* Web-based attacks
* Brute-force attempts
* Scanning
* Suspicious request behavior

These will be studied later.

---

# 2.3 HTTPS — HTTP Secure

HTTPS is HTTP transmitted through a secure **TLS** connection.

Conceptually:

```text
HTTP
  ↓
TLS
  ↓
Encrypted Communication
```

Usually:

```text
Port = 443
```

Normal HTTP:

```text
IoT Device
    ↓
HTTP
    ↓
Server
```

HTTPS:

```text
IoT Device
    ↓
TLS-encrypted traffic
    ↓
Server
```

---

# HTTPS and IDS

Encryption limits visibility into application-layer content.

However, network-level information may still be observable, such as:

* IP addresses
* Ports
* Packet sizes
* Timing
* Flow duration
* Traffic volume
* Protocol information

Therefore:

```text
Encrypted Payload
       ≠
No Network Information
```

An IDS can still analyze metadata and traffic behavior.

---

# 2.4 DNS — Domain Name System

DNS translates domain names into IP addresses.

Instead of using:

```text
142.x.x.x
```

applications can use:

```text
example.com
```

DNS resolves:

```text
Domain Name
     ↓
DNS
     ↓
IP Address
```

---

# DNS Communication

```text
IoT Device
    |
    | DNS Query
    ↓
DNS Server
    |
    | DNS Response
    ↓
IoT Device
```

Example:

```text
"What is the IP address of example.com?"
```

DNS server returns the corresponding IP information.

---

# DNS Ports

DNS commonly uses:

```text
UDP 53
```

TCP can also be used for DNS in certain situations.

Important:

> DNS does not always use UDP.

---

# DNS in IoT

An IoT device may need DNS to discover the IP address of a cloud service.

```text
IoT Device
     ↓
DNS Query
     ↓
DNS Server
     ↓
Cloud Server IP
     ↓
IoT Device connects
```

---

# DNS and IDS

DNS traffic can provide useful signals.

Normal:

```text
Device
   ↓
Expected domains
```

Suspicious:

```text
Device
   ↓
Many unusual domains
```

Potentially useful characteristics:

* Number of DNS queries
* Query frequency
* Destination DNS server
* Query/response behavior
* Domain-related patterns
* Traffic volume

DNS can therefore be relevant when analyzing compromised IoT devices and malware-related behavior.

---

# 2.5 ICMP — Internet Control Message Protocol

ICMP is used mainly for:

* Network diagnostics
* Network control
* Error reporting

A common example is:

```text
ping
```

---

# ICMP Echo Request / Reply

```text
Device A
   |
   | ICMP Echo Request
   ↓
Device B
   |
   | ICMP Echo Reply
   ↓
Device A
```

This is the basic mechanism behind common ping operations.

---

# ICMP and Network Discovery

Attackers can use ICMP messages to discover active hosts.

Example:

```text
192.168.1.1 → Probe
192.168.1.2 → Probe
192.168.1.3 → Probe
192.168.1.4 → Probe
...
```

This can help identify reachable devices.

---

# ICMP Flood

An attacker can generate a large volume of ICMP traffic.

```text
Attacker
  ↓↓↓↓↓↓↓↓↓↓↓
Target
```

Potential characteristics:

* High packet rate
* High traffic volume
* Large number of ICMP packets

These characteristics may become IDS features.

---

# 2.6 ARP — Address Resolution Protocol

ARP is used on IPv4 local networks to map:

```text
IP Address
     ↓
MAC Address
```

Example:

```text
192.168.1.20
      ↓
AA:BB:CC:11:22:33
```

---

# ARP Request

Suppose Device A wants to communicate with:

```text
192.168.1.20
```

but does not know its MAC address.

It sends an ARP request:

```text
"Who has 192.168.1.20?"
```

The request is broadcast on the local network.

---

# ARP Reply

The device owning the IP responds:

```text
192.168.1.20
      ↓
AA:BB:CC:11:22:33
```

Therefore:

```text
IP Address
    ↓
ARP
    ↓
MAC Address
```

---

# ARP Table

Devices maintain mappings such as:

```text
IP Address       MAC Address

192.168.1.1      AA:AA:AA:AA:AA:AA
192.168.1.20     BB:BB:BB:BB:BB:BB
192.168.1.30     CC:CC:CC:CC:CC:CC
```

---

# ARP Spoofing

ARP has important security implications.

In ARP spoofing, an attacker sends misleading ARP information.

Conceptually:

```text
Victim
   ↓
Believes:

Gateway IP → Attacker MAC
```

instead of:

```text
Gateway IP → Real Gateway MAC
```

The attacker may then position themselves between communicating devices.

This can enable:

**Man-in-the-Middle (MITM)** behavior.

---

# ARP and IoT IDS

IoT devices commonly operate inside local networks.

Therefore abnormal ARP activity can be relevant.

Possible indicators:

* Unexpected ARP responses
* Frequent ARP changes
* Conflicting IP/MAC mappings
* Abnormal ARP traffic

---

# 2.7 All Protocols Together

```text
                    IoT Application
                           ↓
              ┌────────────┼────────────┐
              ↓            ↓            ↓
            MQTT          HTTP         DNS
              ↓            ↓            ↓
              └────────────┼────────────┘
                           ↓
                      TCP / UDP
                           ↓
                      IP / ICMP
                           ↓
                    ARP / Wi-Fi
                           ↓
                        Network
```

Each protocol has a different purpose.

---

# 2.8 MQTT vs HTTP

| Feature             | MQTT              | HTTP                  |
| ------------------- | ----------------- | --------------------- |
| Layer               | Application       | Application           |
| Communication model | Publish/Subscribe | Request/Response      |
| Main use            | IoT messaging     | Web/API communication |
| Typical port        | 1883              | 80                    |
| Secure version      | MQTT over TLS     | HTTPS                 |
| IoT relevance       | Very High         | High                  |
| Central component   | Broker             | Server                |

Core difference:

```text
MQTT:
Publisher → Broker → Subscriber

HTTP:
Client → Request → Server → Response
```

---

# 2.9 Protocol → Transport Mapping

Simplified mapping:

```text
MQTT
  ↓
Usually TCP

HTTP
  ↓
TCP

HTTPS
  ↓
TCP + TLS

DNS
  ↓
Usually UDP
Can also use TCP

ICMP
  ↓
IP-based control/diagnostic protocol

ARP
  ↓
Local link/network access
```

---

# 2.10 Why Protocol Identification Matters

Suppose a dataset contains:

```text
Protocol = TCP
Destination Port = 1883
```

Possible interpretation:

```text
MQTT traffic
```

Suppose:

```text
Protocol = TCP
Destination Port = 443
```

Possible interpretation:

```text
HTTPS / TLS traffic
```

Suppose:

```text
Protocol = UDP
Destination Port = 53
```

Possible interpretation:

```text
DNS traffic
```

Important:

> Port + protocol provide clues, but they do not always prove the application protocol.

---

# 2.11 Protocol Behavior → Traffic Behavior

Different protocols naturally produce different traffic patterns.

MQTT:

```text
CONNECT / PUBLISH / SUBSCRIBE
        ↓
IoT messaging traffic
```

HTTP:

```text
Request / Response
        ↓
Web/API traffic
```

DNS:

```text
Query / Response
        ↓
Name-resolution traffic
```

ICMP:

```text
Echo / Control Messages
        ↓
Diagnostic / Discovery traffic
```

ARP:

```text
IP ↔ MAC resolution
        ↓
Local-network traffic
```

Therefore:

```text
Protocol
   ↓
Communication Behavior
   ↓
Traffic Pattern
   ↓
Features
   ↓
IDS Dataset
```

---

# 2.12 Why Protocol Knowledge Matters for Dataset Research

When researching IoT IDS datasets, different datasets may contain different protocols.

Example:

```text
Dataset A:
MQTT
HTTP
DNS
TCP
UDP
ICMP
ARP
```

Another:

```text
Dataset B:
Wi-Fi
TCP
UDP
HTTP
```

Another:

```text
Dataset C:
MQTT
CoAP
DNS
TCP
UDP
```

Therefore, dataset selection should not depend only on:

* Number of rows
* Number of features
* Dataset size

You should also investigate:

```text
Which IoT devices?
Which protocols?
Which attacks?
Which traffic?
How was traffic captured?
How were features extracted?
How were labels created?
```

---

# 2.13 Protocol → Attack → Feature Connection

This is one of the most important concepts in the project.

## MQTT

```text
MQTT
  ↓
MQTT Flooding
  ↓
Abnormal message/connection rate
  ↓
Packet Rate
Flow Duration
Packet Count
Byte Rate
...
```

## TCP

```text
TCP
  ↓
SYN Flood
  ↓
Large number of SYN attempts
  ↓
SYN Count
Packet Rate
Flow Statistics
...
```

## ARP

```text
ARP
  ↓
ARP Spoofing
  ↓
Abnormal IP-MAC mappings
  ↓
ARP traffic patterns
...
```

## ICMP

```text
ICMP
  ↓
ICMP Flood
  ↓
Large amount of ICMP traffic
  ↓
Packet Rate
Byte Rate
Packet Count
...
```

The overall relationship is:

```text
Protocol
   ↓
Normal Communication
   ↓
Attack
   ↓
Abnormal Communication
   ↓
Changed Traffic Features
   ↓
IDS Dataset
   ↓
Deep Learning
```

---

# 2.14 Phase 2 — What You Must Know

## MQTT

Understand:

* What MQTT is
* Publisher
* Subscriber
* Broker
* Topic
* PUBLISH
* SUBSCRIBE
* CONNECT
* DISCONNECT
* QoS
* Keep Alive
* MQTT ports
* MQTT security
* MQTT traffic patterns

## HTTP / HTTPS

Understand:

* Request/response model
* HTTP
* HTTPS
* Ports 80/443
* IoT API communication
* TLS encryption
* Effect of encryption on traffic visibility

## DNS

Understand:

* Domain → IP resolution
* DNS queries
* DNS responses
* UDP/TCP
* Port 53
* DNS in IoT
* Suspicious DNS behavior

## ICMP

Understand:

* Echo Request
* Echo Reply
* Ping
* Network discovery
* ICMP flooding

## ARP

Understand:

* IP → MAC resolution
* ARP request
* ARP reply
* ARP table
* ARP spoofing
* MITM connection

---

# 🔥 PHASE 2 FINAL MENTAL MODEL

```text
                 IoT DEVICE
                     ↓
              IoT Application
                     ↓
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      MQTT         HTTP          DNS
        ↓            ↓            ↓
        └────────────┼────────────┘
                     ↓
                  TCP/UDP
                     ↓
                  IP/ICMP
                     ↓
                ARP / Wi-Fi
                     ↓
                  Packets
                     ↓
                   Flows
                     ↓
                  Features
                     ↓
                IDS Dataset
                     ↓
             Deep Learning Model
                     ↓
             Benign / Attack
```

---

# 🔥 MOST IMPORTANT CONNECTION

```text
PROTOCOL
   ↓
NORMAL COMMUNICATION
   ↓
ATTACK
   ↓
ABNORMAL COMMUNICATION
   ↓
CHANGED TRAFFIC FEATURES
   ↓
DATASET
   ↓
DEEP LEARNING
   ↓
INTRUSION DETECTION
```

---

# PHASE 2 CHECKPOINT

Before moving to Phase 3, you should be able to look at network traffic and understand:

```text
What protocol is involved?
        ↓
What is this protocol normally used for?
        ↓
How does the communication work?
        ↓
What kind of traffic does it generate?
        ↓
How could an attack change that behavior?
        ↓
Which network features could change?
```

Once you can make this connection, **Phase 2 is complete.**

Next:

```text
PHASE 3
IoT Attack Fundamentals & Attack Taxonomy
```

where we will study the attacks themselves and connect:

```text
Attack
  ↓
Attacker Behavior
  ↓
Protocol Used
  ↓
Traffic Pattern
  ↓
Observable Features
  ↓
Potential IDS Detection
```
