# PHASE 3 — IoT SECURITY & ATTACK FUNDAMENTALS

## Deep Learning Based Intrusion Detection for IoT Devices

---

# Phase Overview

This phase focuses on understanding the **major attacks that affect IoT networks** and, more importantly, how those attacks change normal network behavior.

The goal is not to learn how to perform attacks, but to understand them from the **defensive/IDS perspective**:

```text
Attack
   ↓
How Attacker Behaves
   ↓
What Normal Behavior Becomes Abnormal
   ↓
Network Traffic Changes
   ↓
Observable Features
   ↓
Potential IDS Detection
```

We will study:

```text
DoS / DDoS
SYN Flood
UDP Flood
ICMP Flood
Port Scanning
Host Discovery / Ping Sweep
Brute Force
ARP Spoofing
DNS Spoofing
SQL Injection
XSS
Command Injection
IoT Botnets
Mirai
```

---

# 3.1 DoS — Denial of Service

## What is DoS?

A **Denial of Service (DoS)** attack attempts to make a device, service, or network resource unavailable to legitimate users.

Basic idea:

```text
Attacker
    ↓
Large / malicious traffic or requests
    ↓
Target
    ↓
Resources become exhausted
    ↓
Legitimate users are affected
```

The attacker may attempt to consume:

* Network bandwidth
* CPU
* Memory
* Connection capacity
* Application resources

---

## Normal Traffic

Normally:

```text
Few / expected clients
       ↓
Normal request rate
       ↓
Normal resource usage
       ↓
Service available
```

---

## During DoS

```text
Attacker
   ↓↓↓↓↓↓↓↓↓
Target
   ↓
Abnormally high traffic/request rate
   ↓
Resource exhaustion
   ↓
Service degradation/unavailability
```

---

## IDS-Relevant Features

Potential indicators:

* Packet count
* Packet rate
* Byte count
* Byte rate
* Flow count
* Flow duration
* Connection rate
* Request rate
* Source/destination distribution

Important idea:

```text
Normal traffic rate
        ↓
Sudden abnormal increase
        ↓
Possible DoS behavior
```

---

# 3.2 DDoS — Distributed Denial of Service

## What is DDoS?

DDoS is a DoS attack performed using **multiple compromised systems**.

```text
Compromised Device 1 ──┐
Compromised Device 2 ──┤
Compromised Device 3 ──┤
Compromised Device 4 ──┤
                       ↓
                     Target
```

Instead of one attacker generating traffic, many systems participate.

---

## Why DDoS is Important for IoT

IoT devices are attractive targets for compromise because large numbers of devices can potentially be coordinated into a botnet.

Therefore:

```text
IoT Devices
     ↓
Compromised
     ↓
Botnet
     ↓
Coordinated Traffic
     ↓
DDoS
```

---

## IDS-Relevant Features

Potential indicators:

* High packet rate
* High byte rate
* Large number of source IPs
* Many simultaneous flows
* High connection rate
* Abnormal traffic volume
* Sudden traffic spikes

Important distinction:

```text
DoS:
Usually one/fewer attacking sources

DDoS:
Many distributed sources
```

---

# 3.3 SYN Flood

## What is SYN Flood?

A SYN flood is a type of **TCP-based DoS attack** that abuses the TCP connection-establishment process.

Normal TCP connection establishment:

```text
Client                Server

  SYN  ───────────────→
       ←──────── SYN-ACK
  ACK  ───────────────→

      Connection Established
```

This is called the **TCP three-way handshake**.

---

## SYN Flood Behavior

The attacker sends many SYN requests without completing the connection establishment normally.

Conceptually:

```text
Attacker
  ↓ SYN
Server
  ↓ SYN-ACK
Attacker
  ✕ No normal completion
```

Repeated many times:

```text
SYN
SYN
SYN
SYN
SYN
SYN
...
```

The target may have many incomplete connection attempts.

---

## Normal vs Attack

Normal:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
Established connection
```

Attack:

```text
SYN
 ↓
SYN-ACK
 ↓
No expected completion
```

---

## IDS-Relevant Features

Potential indicators:

* SYN packet count
* SYN/ACK ratio
* Incomplete connections
* Connection attempt rate
* Flow count
* Packet rate
* Source IP distribution
* Destination port

Important concept:

```text
Large number of SYN attempts
        +
Low connection completion
        ↓
Possible SYN Flood
```

---

# 3.4 UDP Flood

## What is UDP Flood?

A UDP flood attempts to overwhelm a target by generating a large amount of UDP traffic.

UDP does not establish a connection like TCP.

Conceptually:

```text
Attacker
   ↓
UDP packets
↓↓↓↓↓↓↓↓↓↓
Target
```

The target may need to process a large number of packets.

---

## Normal Traffic

```text
Normal UDP communication
        ↓
Expected packet rate
        ↓
Expected traffic volume
```

---

## Attack Behavior

```text
Very high UDP packet rate
        ↓
High bandwidth/resource consumption
        ↓
Service degradation
```

---

## IDS-Relevant Features

Potential indicators:

* UDP packet count
* UDP packet rate
* Byte rate
* Flow count
* Source/destination ports
* Flow duration
* Traffic volume

---

# 3.5 ICMP Flood

## What is ICMP Flood?

An ICMP flood sends a large volume of ICMP packets toward a target.

A common example involves repeated ICMP Echo Requests.

```text
Attacker
   ↓↓↓↓↓↓↓↓↓↓↓↓↓
ICMP packets
   ↓↓↓↓↓↓↓↓↓↓↓↓↓
Target
```

---

## Normal ICMP

Usually:

```text
Occasional ping/diagnostic traffic
```

---

## Attack Behavior

```text
Normal:
Few ICMP packets

Attack:
Huge number of ICMP packets
```

---

## IDS-Relevant Features

Potential indicators:

* ICMP packet count
* ICMP packet rate
* Byte rate
* Flow count
* Source IP count
* Packet size
* Traffic volume

---

# 3.6 Port Scanning

## What is Port Scanning?

Port scanning is used to discover which network ports/services are accessible on a target.

Conceptually:

```text
Attacker
   ↓
Target
   ├── Port 21
   ├── Port 22
   ├── Port 23
   ├── Port 80
   ├── Port 443
   └── Port 1883
```

The attacker probes multiple ports.

---

## Why Attackers Scan Ports

Port scanning can help identify:

* Open ports
* Running services
* Potential attack surfaces
* Network configuration

It is often part of the **reconnaissance phase** of an attack.

---

## Normal Traffic

A normal IoT device might communicate with a relatively limited set of expected services.

Example:

```text
Device
   ↓
443
1883
53
```

---

## Scanning Behavior

```text
Attacker
   ↓
Port 21
Port 22
Port 23
Port 25
Port 53
Port 80
Port 443
Port 1883
...
```

A large number of ports may be probed within a short period.

---

## IDS-Relevant Features

Potential indicators:

* Number of destination ports
* Number of unique ports
* Connection attempts
* Destination IPs
* Failed connections
* Packet rate
* Flow count
* Port diversity

Important pattern:

```text
One source
   ↓
Many destination ports
   ↓
Short time period
   ↓
Possible Port Scan
```

---

# 3.7 Host Discovery / Ping Sweep

## What is Host Discovery?

Host discovery attempts to determine which devices are active/reachable on a network.

A common technique is a **ping sweep**.

Example:

```text
Attacker
   ↓
192.168.1.1
192.168.1.2
192.168.1.3
192.168.1.4
192.168.1.5
...
```

The attacker checks which hosts respond.

---

## Ping Sweep

Conceptually:

```text
ICMP Echo Request
        ↓
Multiple IP addresses
        ↓
Responses identify reachable hosts
```

---

## Why It Matters for IoT

An IoT network may contain:

```text
Router
Camera
Smart TV
Sensor
Smart bulb
Gateway
Server
```

An attacker first needs to understand what devices exist.

Therefore:

```text
Host Discovery
      ↓
Identify Devices
      ↓
Port Scanning
      ↓
Identify Services
      ↓
Attack
```

---

## IDS-Relevant Features

Potential indicators:

* Number of destination IPs
* Number of unique hosts
* ICMP packet count
* Destination diversity
* Packet rate
* Connection attempts
* Source-to-destination pattern

Important pattern:

```text
One source
   ↓
Many destination IPs
   ↓
Short period
   ↓
Possible Host Discovery
```

---

# 3.8 Brute Force

## What is Brute Force?

A brute-force attack attempts to discover valid credentials by repeatedly trying authentication combinations.

Conceptually:

```text
Attacker
   ↓
Username + Password attempt
   ↓
Server
   ↓
Failure

Repeat...
```

Eventually:

```text
Possible valid credentials
```

---

## IoT Brute Force

IoT devices may expose:

* Web interfaces
* SSH
* Telnet
* MQTT services
* Administrative interfaces

An attacker may repeatedly attempt authentication.

---

## Normal Behavior

```text
Few authentication attempts
        ↓
Successful/legitimate login
```

---

## Attack Behavior

```text
Many authentication attempts
        ↓
Repeated failures
        ↓
Possible credential attack
```

---

## IDS-Relevant Features

Potential indicators:

* Authentication attempt count
* Failed connection count
* Request rate
* Connection rate
* Source IP
* Destination port
* Session frequency
* Flow duration

Important pattern:

```text
Same source
   ↓
Repeated connection/authentication attempts
   ↓
High failure rate
   ↓
Possible Brute Force
```

---

# 3.9 ARP Spoofing

## What is ARP Spoofing?

ARP spoofing involves sending false ARP information to manipulate IP-to-MAC mappings on a local network.

Normal:

```text
Gateway IP → Real Gateway MAC
```

Attack:

```text
Gateway IP → Attacker MAC
```

The victim may then send traffic to the attacker instead of the legitimate gateway.

---

## Normal ARP

```text
Victim
   |
   | ARP Request
   ↓
Network
   |
   | Correct ARP Reply
   ↓
Victim
```

---

## ARP Spoofing

```text
Attacker
   ↓
False ARP information
   ↓
Victim
```

The attacker may attempt to position themselves between:

```text
Victim ↔ Gateway
```

This can enable **Man-in-the-Middle (MITM)** behavior.

---

## IDS-Relevant Features

Potential indicators:

* ARP request count
* ARP response count
* IP-MAC mapping changes
* Duplicate/conflicting mappings
* Unexpected ARP responses
* ARP packet frequency
* MAC address changes

Important pattern:

```text
One IP
   ↓
Unexpected MAC change
   ↓
Conflicting ARP information
   ↓
Possible ARP Spoofing
```

---

# 3.10 DNS Spoofing

## What is DNS Spoofing?

DNS spoofing involves providing false DNS information so that a domain name resolves to an incorrect IP address.

Normal:

```text
example.com
     ↓
DNS
     ↓
Legitimate IP
```

Spoofed:

```text
example.com
     ↓
False DNS Response
     ↓
Attacker-controlled / incorrect IP
```

---

## Possible Goal

The attacker may attempt to redirect a victim toward:

* A malicious server
* A phishing page
* An attacker-controlled service

---

## IDS-Relevant Features

Potential indicators can include:

* Unexpected DNS responses
* DNS response inconsistencies
* Abnormal DNS server behavior
* Unusual query/response patterns
* Unexpected destination IPs
* High DNS query rate
* DNS traffic anomalies

Important:

> Detecting DNS spoofing from flow-level data alone can be difficult because the actual DNS content may not always be available.

---

# 3.11 SQL Injection

## What is SQL Injection?

SQL Injection is an application-layer attack in which malicious input is inserted into an application's database query processing.

Basic conceptual flow:

```text
User Input
    ↓
Web Application
    ↓
Database Query
    ↓
Database
```

If input is handled insecurely:

```text
Malicious Input
      ↓
Application
      ↓
Unexpected Database Query
```

---

## Why It Matters to IoT

IoT systems may have:

```text
IoT Device
    ↓
Backend API
    ↓
Web Application
    ↓
Database
```

Therefore, the IoT device ecosystem can contain web/database components that may be vulnerable.

---

## Normal Behavior

```text
Normal request
      ↓
Expected application processing
      ↓
Expected database operation
```

---

## Attack Behavior

```text
Suspicious input
      ↓
Abnormal application/database behavior
      ↓
Possible unauthorized database operation
```

---

## IDS-Relevant Features

If payload/application data is available:

* Suspicious request patterns
* Request size
* HTTP method
* Request frequency
* Response behavior
* Error responses
* Repeated abnormal requests

With encrypted traffic or flow-only datasets:

```text
Payload visibility ↓
        ↓
Direct SQLi detection becomes harder
```

Therefore SQL Injection may require **application-layer data** rather than only network-flow statistics.

---

# 3.12 XSS — Cross-Site Scripting

## What is XSS?

XSS is a web application vulnerability where attacker-controlled script content is injected into a web page and executed in a victim's browser context.

Conceptually:

```text
Attacker Input
      ↓
Web Application
      ↓
Stored / Reflected Content
      ↓
Victim's Browser
      ↓
Script executes
```

---

## IoT Relevance

IoT devices may provide web-based management interfaces.

Example:

```text
IoT Device
    ↓
Web Management Interface
    ↓
Browser
```

If the interface is vulnerable, XSS may become relevant.

---

## IDS-Relevant Features

If application-layer content is visible:

* Suspicious HTTP requests
* Abnormal parameters
* Request size
* Request frequency
* Response patterns
* Repeated suspicious requests

Again:

> Flow-level datasets may not contain enough information to reliably identify XSS directly.

---

# 3.13 Command Injection

## What is Command Injection?

Command Injection occurs when attacker-controlled input is improperly passed to operating-system command execution.

Conceptually:

```text
User Input
    ↓
Application
    ↓
Operating System Command
```

If input is insecurely handled:

```text
Malicious Input
      ↓
Application
      ↓
Unexpected OS command execution
```

---

## IoT Relevance

This attack can be particularly important for IoT because many IoT devices run operating systems and expose management interfaces.

Potential path:

```text
Attacker
   ↓
Web/API Interface
   ↓
IoT Application
   ↓
Operating System
   ↓
Command Execution
```

---

## IDS-Relevant Features

If application content is visible:

* Suspicious requests
* Unusual parameters
* Request size
* Request frequency
* Response anomalies
* Repeated requests

Network-flow-only datasets may not provide enough information to directly identify command injection.

---

# 3.14 IoT Botnets

## What is a Botnet?

A botnet is a collection of compromised devices controlled by an attacker.

```text
             Attacker
                 ↓
          Command / Control
                 ↓
       ┌─────────┼─────────┐
       ↓         ↓         ↓
     IoT 1     IoT 2     IoT 3
       ↓         ↓         ↓
       └─────────┼─────────┘
                 ↓
              Target
```

The compromised IoT devices are often called **bots**.

---

## Botnet Lifecycle

A simplified lifecycle:

```text
Discovery
    ↓
Exploitation
    ↓
Device Compromise
    ↓
Malware Installation
    ↓
Command & Control
    ↓
Botnet
    ↓
Attack / Malicious Activity
```

---

## IoT Botnet Traffic

Compromised devices may communicate with:

* Command-and-control infrastructure
* Other malicious infrastructure
* Attack targets

This can produce abnormal network behavior.

---

## IDS-Relevant Features

Potential indicators:

* Unusual outbound connections
* Repeated connections to suspicious destinations
* Periodic communication
* Abnormal traffic volume
* Unexpected protocols/ports
* Increased scanning
* Sudden behavioral changes

Important concept:

```text
Normal IoT Device
       ↓
Predictable communication

Compromised IoT Device
       ↓
New / abnormal communication
       ↓
Possible Botnet Activity
```

---

# 3.15 Mirai

## What is Mirai?

**Mirai** is a well-known IoT malware/botnet family that demonstrated how vulnerable IoT devices could be compromised and coordinated into large-scale attacks.

Its importance for this project is that it provides a real-world example of:

```text
IoT Vulnerability
       ↓
Device Compromise
       ↓
Botnet Formation
       ↓
Large-Scale Attack
```

---

# Simplified Mirai Attack Lifecycle

Conceptually:

```text
Internet
   ↓
Scan for vulnerable IoT devices
   ↓
Attempt compromise
   ↓
Infected IoT device
   ↓
Becomes bot
   ↓
Receives commands
   ↓
Participates in attacks
```

Mirai is strongly associated with IoT botnet-driven DDoS activity.

---

# Mirai and IDS

A compromised Mirai-like device may exhibit:

```text
Normal:
Expected IoT communication

After compromise:
Scanning
      ↓
Unexpected connections
      ↓
Command/control communication
      ↓
Attack traffic
```

Potential IDS-relevant characteristics:

* Scanning behavior
* Number of destination IPs
* Destination port diversity
* Connection rate
* Packet rate
* Byte rate
* Outbound traffic changes
* Repeated communication patterns
* Sudden behavioral changes

---

# 3.16 Attack Classification

It is useful to organize the attacks into categories.

## DoS / Availability Attacks

```text
DoS
DDoS
SYN Flood
UDP Flood
ICMP Flood
```

Main goal:

```text
Reduce availability
        ↓
Exhaust resources
```

---

## Reconnaissance Attacks

```text
Port Scanning
Host Discovery
Ping Sweep
```

Main goal:

```text
Discover:
Hosts
Ports
Services
Network structure
```

---

## Credential Attacks

```text
Brute Force
```

Main goal:

```text
Obtain valid credentials
```

---

## Network Manipulation / Spoofing

```text
ARP Spoofing
DNS Spoofing
```

Main goal:

```text
Manipulate communication
or redirect traffic
```

---

## Web/Application Attacks

```text
SQL Injection
XSS
Command Injection
```

Main goal:

```text
Exploit vulnerable applications
```

---

## Malware / Botnet Activity

```text
IoT Botnets
Mirai
```

Main goal:

```text
Compromise devices
Control devices
Perform coordinated malicious activity
```

---

# 3.17 Attack → Network Behavior

This is one of the most important tables for the entire project.

| Attack            | Typical Abnormal Behavior            |
| ----------------- | ------------------------------------ |
| DoS               | High traffic/resource consumption    |
| DDoS               | High traffic from many sources       |
| SYN Flood         | Many SYNs, incomplete connections    |
| UDP Flood         | High UDP packet/byte rate            |
| ICMP Flood        | High ICMP packet rate                |
| Port Scanning     | Many destination ports               |
| Host Discovery    | Many destination IPs                 |
| Brute Force       | Repeated authentication attempts     |
| ARP Spoofing      | Abnormal IP-MAC mappings             |
| DNS Spoofing      | Suspicious/inconsistent DNS behavior |
| SQL Injection     | Suspicious application requests      |
| XSS               | Malicious web input                  |
| Command Injection | Suspicious command-related input     |
| IoT Botnet        | Abnormal outbound/control traffic    |
| Mirai             | Scanning + C2 + malicious traffic    |

---

# 3.18 Attack → Features Connection

Different attacks can affect different measurable network features.

Important feature categories:

## Volume Features

```text
Packet Count
Byte Count
Packet Rate
Byte Rate
```

Useful for:

```text
DoS
DDoS
UDP Flood
ICMP Flood
```

---

## Connection Features

```text
Connection Count
Flow Count
Connection Rate
Flow Duration
Incomplete Connections
```

Useful for:

```text
SYN Flood
Brute Force
Scanning
Botnet Activity
```

---

## Diversity Features

```text
Unique Source IPs
Unique Destination IPs
Unique Destination Ports
Port Diversity
```

Useful for:

```text
DDoS
Port Scanning
Host Discovery
Botnet Scanning
```

---

## Protocol Features

```text
TCP
UDP
ICMP
ARP
DNS
MQTT
HTTP
```

Useful for identifying changes in protocol behavior.

---

## Timing Features

```text
Inter-arrival Time
Connection Frequency
Burst Behavior
Flow Duration
```

Useful for detecting abnormal communication patterns.

---

# 3.19 The Most Important IDS Concept

Do not think:

```text
Attack
   ↓
One specific feature
```

Instead think:

```text
Attack
   ↓
Change in behavior
   ↓
Multiple traffic characteristics change
   ↓
Feature combination
   ↓
IDS model
```

For example:

```text
SYN Flood
   ↓
High SYN count
   +
High connection attempts
   +
Low completion rate
   +
Abnormal packet rate
   ↓
Potential SYN Flood pattern
```

Similarly:

```text
Port Scan
   ↓
Many destination ports
   +
Many short flows
   +
High connection attempts
   +
Low successful connections
   ↓
Potential scanning behavior
```

---

# 3.20 Important Difference: Network vs Application Attacks

Not every attack can be detected equally well from the same type of dataset.

## Network/Transport Attacks

Examples:

```text
SYN Flood
UDP Flood
ICMP Flood
Port Scanning
Host Discovery
```

These often produce strong **network-level traffic patterns**.

---

## Application Attacks

Examples:

```text
SQL Injection
XSS
Command Injection
```

These may depend heavily on:

```text
HTTP request
Payload
URL
Parameters
Application logs
Server responses
```

If a dataset contains only flow statistics:

```text
Source IP
Destination IP
Port
Protocol
Packet Count
Byte Count
Duration
...
```

then direct detection of application attacks may be much harder.

This distinction will become **very important when selecting and comparing datasets later.**

---

# 3.21 Attack Detection Mental Model

For every attack, ask these four questions:

```text
1. What does the attacker do?
          ↓
2. How does normal communication change?
          ↓
3. What network behavior becomes abnormal?
          ↓
4. Which measurable features can reveal that change?
```

Example:

```text
SYN Flood
    ↓
Attacker sends many SYNs
    ↓
Many incomplete TCP connections
    ↓
High SYN count + abnormal connection rate
    ↓
IDS features
```

---

# 3.22 Complete Attack → IDS Pipeline

```text
                ATTACK
                   ↓
          Attacker Behavior
                   ↓
        Change in Communication
                   ↓
          Abnormal Network Traffic
                   ↓
             Packet / Flow Data
                   ↓
              Extract Features
                   ↓
        ┌──────────┴──────────┐
        ↓                     ↓
   Statistical             Temporal
    Features                Features
        ↓                     ↓
        └──────────┬──────────┘
                   ↓
             IDS Dataset
                   ↓
          Machine Learning /
           Deep Learning
                   ↓
          Attack Classification
```

---

# 3.23 Phase 3 — What You Must Know

For every attack, you should be able to explain:

```text
What is the attack?
        ↓
What is the attacker's objective?
        ↓
How does the attack work conceptually?
        ↓
What protocol/application is involved?
        ↓
What changes compared with normal traffic?
        ↓
Which network features may reveal it?
```

You should specifically understand:

### Availability

```text
DoS
DDoS
SYN Flood
UDP Flood
ICMP Flood
```

### Reconnaissance

```text
Port Scanning
Host Discovery
Ping Sweep
```

### Authentication

```text
Brute Force
```

### Spoofing / Network Manipulation

```text
ARP Spoofing
DNS Spoofing
```

### Application Attacks

```text
SQL Injection
XSS
Command Injection
```

### Malware / Botnets

```text
IoT Botnets
Mirai
```

---

# 🔥 PHASE 3 FINAL MENTAL MODEL

```text
                    IoT ATTACK
                        ↓
              Attacker's Objective
                        ↓
                Attack Technique
                        ↓
              Protocol / Application
                        ↓
              Abnormal Communication
                        ↓
              Network Traffic Changes
                        ↓
        ┌───────────────┼───────────────┐
        ↓               ↓               ↓
     Volume          Connection       Diversity
     Features          Features        Features
        ↓               ↓               ↓
        └───────────────┼───────────────┘
                        ↓
                 IDS Features
                        ↓
                    Dataset
                        ↓
               Deep Learning Model
                        ↓
              Attack Classification
```

---

# 🔥 MOST IMPORTANT CONNECTION OF PHASE 3

```text
NORMAL DEVICE BEHAVIOR
          ↓
      ATTACK OCCURS
          ↓
   BEHAVIOR CHANGES
          ↓
   TRAFFIC CHANGES
          ↓
   FEATURES CHANGE
          ↓
     DATASET CAPTURES
       THOSE CHANGES
          ↓
    MODEL LEARNS PATTERNS
          ↓
     IDS DETECTS ATTACK
```

The ultimate objective is **not simply to memorize attack names**.

The real objective is to understand:

> **What makes malicious IoT traffic different from normal IoT traffic, and how those differences can become measurable features for an IDS.**

That understanding will directly prepare you for the next major stage: **researching, comparing, and selecting IoT intrusion-detection datasets based on devices, protocols, attacks, traffic capture methodology, labels, features, and suitability for Deep Learning.**
