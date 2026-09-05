# PHASE 1.2 — NETWORK TRAFFIC & FLOW ANALYSIS

## Deep Learning Based Intrusion Detection for IoT Devices

---

# 1. Network Traffic

**Network traffic** is the collection of data exchanged between devices over a network.

Example:

```text
IoT Device
    ↓
Network Traffic
    ↓
Router
    ↓
Server
```

Every communication generates observable characteristics.

Examples:

* Packets
* Packet sizes
* Packet timing
* Source/Destination addresses
* Ports
* Protocols
* TCP flags

---

# 2. Why Network Traffic Matters for IDS

Our project is based on the idea that:

```text
Normal Activity
      ↓
Normal Traffic Pattern
```

while:

```text
Attack
      ↓
Changed Network Behavior
      ↓
Changed Traffic Pattern
```

Therefore:

```text
Network Traffic
      ↓
Traffic Characteristics
      ↓
Features
      ↓
Deep Learning Model
      ↓
Attack Detection
```

---

# 3. Packet-Level Data

At packet level, we examine individual packets.

Example:

```text
Packet 1
Packet 2
Packet 3
Packet 4
Packet 5
```

Each packet may contain information such as:

* Source IP
* Destination IP
* Source Port
* Destination Port
* Protocol
* Packet length
* TCP flags
* Timestamp

Packet-level analysis provides detailed information about individual communications.

---

# 4. Flow-Level Data

Instead of analyzing every packet individually, multiple packets can be grouped into a **network flow**.

A flow represents communication between endpoints over a period of time.

Conceptually:

```text
Source
  ↓
Multiple Packets
  ↓
Destination
```

Instead of storing every packet individually, the traffic can be summarized using statistical characteristics.

---

# 5. Five-Tuple

A common way of identifying a network flow is using a **5-tuple**:

```text
1. Source IP
2. Destination IP
3. Source Port
4. Destination Port
5. Protocol
```

Example:

```text
Source IP       = 192.168.1.10
Destination IP  = 192.168.1.50
Source Port     = 50000
Destination Port= 1883
Protocol        = TCP
```

Together:

```text
192.168.1.10
50000
192.168.1.50
1883
TCP
```

This can identify a communication flow.

---

# 6. Uni-Flow

A **uni-flow** represents traffic primarily in one direction.

```text
A ─────────────→ B
```

Example:

```text
IoT Device ─────────→ Server
```

The traffic from A to B is considered separately.

---

# 7. Bi-Flow

A **bi-flow** considers communication in both directions.

```text
A ─────────────→ B
A ←───────────── B
```

Example:

```text
IoT Device ─────────→ Server
IoT Device ←───────── Server
```

Both directions together provide a more complete view of communication.

---

# 8. Forward and Backward Traffic

Many datasets divide flow traffic into:

### Forward direction

Usually:

```text
Source → Destination
```

### Backward direction

Usually:

```text
Destination → Source
```

Example:

```text
IoT Device → Server
     Forward

Server → IoT Device
     Backward
```

This distinction is important because datasets may contain separate statistics for forward and backward traffic.

---

# 9. Flow Duration

**Flow Duration** represents how long a flow lasts.

```text
Start                         End
 |-----------------------------|
          Flow Duration
```

Example:

```text
Flow Duration = 2.5 seconds
```

Different applications and attacks can have different duration patterns.

---

# 10. Packet Count

The number of packets belonging to a flow.

Example:

```text
Flow
 ↓
Packet 1
Packet 2
Packet 3
Packet 4
Packet 5
```

Therefore:

```text
Packet Count = 5
```

Datasets may separate:

* Forward packet count
* Backward packet count
* Total packet count

---

# 11. Byte Count

Represents the total amount of data transferred.

Example:

```text
Packet 1 → 500 bytes
Packet 2 → 300 bytes
Packet 3 → 200 bytes
```

Total:

```text
500 + 300 + 200 = 1000 bytes
```

Datasets may contain:

* Total bytes
* Forward bytes
* Backward bytes

---

# 12. Packet Length

Packet length represents the amount of data in a packet.

Example:

```text
Packet 1 → 500 bytes
Packet 2 → 200 bytes
Packet 3 → 1000 bytes
```

Datasets may calculate:

* Minimum packet length
* Maximum packet length
* Mean packet length
* Standard deviation
* Total packet length

These statistics describe traffic behavior.

---

# 13. Packet Rate

Packet rate measures how quickly packets are transmitted.

Conceptually:

```text
Packet Rate =
Number of Packets / Time
```

Example:

```text
1000 packets
1 second

→ 1000 packets/second
```

High packet rates can be an important signal in flooding attacks.

---

# 14. Byte Rate

Similarly, byte rate measures the amount of data transferred per unit of time.

Conceptually:

```text
Byte Rate =
Total Bytes / Time
```

Example:

```text
10 MB
1 second

→ 10 MB/s
```

This can help identify high-volume traffic.

---

# 15. Inter-Arrival Time (IAT)

**Inter-Arrival Time** is the time difference between consecutive packets.

Example:

```text
Packet 1
   |
   | 10 ms
   ↓
Packet 2
   |
   | 20 ms
   ↓
Packet 3
```

Therefore:

```text
IAT = 10 ms, 20 ms
```

---

# 16. IAT Statistics

Instead of storing every IAT value, datasets may calculate statistics such as:

* Mean IAT
* Minimum IAT
* Maximum IAT
* Standard deviation of IAT

Example:

```text
IAT values:
10 ms
20 ms
15 ms
25 ms
```

The dataset can summarize their distribution.

Timing information can be useful for distinguishing different traffic behaviors.

---

# 17. TCP Flag Counts

A flow may contain multiple TCP packets with different flags.

A dataset may count:

* SYN flag
* ACK flag
* FIN flag
* RST flag
* PSH flag

Example:

```text
SYN → 10
ACK → 15
FIN → 1
RST → 2
```

These values summarize TCP connection behavior.

---

# 18. Why TCP Flags Are Useful for IDS

Consider normal TCP communication:

```text
SYN
SYN-ACK
ACK
Data
ACK
FIN
```

Now consider a possible SYN flood:

```text
SYN
SYN
SYN
SYN
SYN
SYN
...
```

The distribution of TCP flags can therefore differ.

The IDS may learn this difference.

---

# 19. Normal Traffic vs Attack Traffic

### Normal Traffic

```text
IoT Device
    ↓
Regular communication
    ↓
Expected packet rate
    ↓
Expected flow duration
    ↓
Expected packet sizes
```

### Attack Traffic

```text
Attacker / Compromised Device
    ↓
Malicious activity
    ↓
Changed communication pattern
    ↓
Changed packet rate
    ↓
Changed packet sizes
    ↓
Changed flow statistics
```

These differences can become useful features for machine learning.

---

# 20. Example — SYN Flood

Normal TCP:

```text
Client → SYN
Server → SYN-ACK
Client → ACK
```

Attack:

```text
Attacker → SYN
Attacker → SYN
Attacker → SYN
Attacker → SYN
Attacker → SYN
...
```

Potential observable characteristics:

* High SYN count
* High connection-attempt rate
* Many incomplete connections
* Abnormal packet rate
* Different flow behavior

These characteristics can appear as dataset features.

---

# 21. Example — UDP Flood

Attack:

```text
Attacker
   ↓↓↓↓↓↓↓↓↓↓↓↓↓
Target
```

Possible characteristics:

* High packet rate
* High byte rate
* Large traffic volume
* Large number of UDP packets

Again:

```text
Attack
  ↓
Traffic Changes
  ↓
Features Change
```

---

# 22. Example — Port Scanning

An attacker attempts to discover open services.

```text
Target
├── Port 22
├── Port 80
├── Port 443
├── Port 1883
├── Port 8883
└── ...
```

Possible traffic behavior:

* Many destination ports
* Many short connection attempts
* Repeated probes
* Communication with multiple services

This can produce a recognizable traffic pattern.

---

# 23. Example — Host Discovery

An attacker attempts to discover active devices.

Example:

```text
192.168.1.1 → Probe
192.168.1.2 → Probe
192.168.1.3 → Probe
192.168.1.4 → Probe
...
```

Potential characteristics:

* One source communicating with many destinations
* High number of short flows
* Repeated probing

---

# 24. Network Features

A **feature** is a measurable property used to represent network traffic.

Examples:

### Address Features

* Source IP
* Destination IP

### Port Features

* Source Port
* Destination Port

### Protocol Features

* Protocol type

### Flow Features

* Flow Duration
* Packet Count
* Byte Count

### Packet Features

* Packet Length
* Average Packet Length
* Minimum Packet Length
* Maximum Packet Length

### Timing Features

* IAT
* Mean IAT
* Minimum IAT
* Maximum IAT
* IAT Standard Deviation

### TCP Features

* SYN Count
* ACK Count
* FIN Count
* RST Count
* PSH Count

---

# 25. Why Features Matter

Suppose a dataset contains:

```text
Flow Duration
Packet Count
Byte Count
Mean Packet Length
Mean IAT
SYN Count
ACK Count
Protocol
Label
```

These are measurements of network behavior.

The Deep Learning model receives these numerical/categorical representations and attempts to learn relationships between them and the attack labels.

Conceptually:

```text
Network Traffic
       ↓
Feature Extraction
       ↓
Dataset Row
       ↓
Deep Learning Model
       ↓
Prediction
```

---

# 26. Packet-Level vs Flow-Level

## Packet-Level

```text
Packet 1
Packet 2
Packet 3
Packet 4
...
```

More detailed.

## Flow-Level

```text
Flow
├── Duration
├── Packet Count
├── Byte Count
├── Packet Rate
├── Mean Packet Length
├── IAT statistics
└── TCP flag statistics
```

More summarized.

Many modern IDS datasets use **flow-level features**, which is why understanding flows is essential.

---

# 27. How Raw Traffic Becomes a Dataset

A simplified pipeline:

```text
Network
   ↓
Packets
   ↓
Flow Construction
   ↓
Feature Extraction
   ↓
Feature Table
   ↓
Labels
   ↓
IDS Dataset
```

Example:

```text
Raw Traffic
     ↓
Flow
     ↓
Duration = 2.3 sec
Packets = 50
Bytes = 12000
Mean IAT = 15 ms
SYN Count = 1
     ↓
Dataset Row
```

---

# 28. Dataset Row Mental Model

A dataset row can represent:

> **One network flow described using multiple measurable characteristics.**

Example:

```text
Src IP       = 192.168.1.10
Dst IP       = 192.168.1.50
Src Port     = 50000
Dst Port     = 1883
Protocol     = TCP
Duration     = 2.3
Packets      = 50
Bytes        = 12000
Mean IAT     = 15 ms
SYN Count    = 1
Label        = Benign
```

Another row might represent an attack:

```text
Src IP       = 10.0.0.5
Dst IP       = 192.168.1.50
Src Port     = ...
Dst Port     = ...
Protocol     = TCP
Duration     = ...
Packets      = ...
Bytes        = ...
Mean IAT     = ...
SYN Count    = ...
Label        = SYN Flood
```

The model learns patterns across many such examples.

---

# 29. Important Idea: Features Are Not Automatically "Good"

A feature may be:

* Useful
* Redundant
* Irrelevant
* Highly correlated with another feature
* Dataset-specific
* Potentially misleading

Therefore, later we will perform:

* Exploratory Data Analysis
* Feature analysis
* Feature selection
* Correlation analysis
* Model-based feature importance

We should not assume every dataset column is equally useful.

---

# 30. Important Idea: Traffic Behavior Depends on Context

The same feature value isn't necessarily malicious by itself.

For example:

```text
High Packet Rate
```

could be:

* Normal for a particular application
* Abnormal for another application
* Attack traffic under certain circumstances

Therefore, IDS models need to learn **patterns and relationships between multiple features**, rather than relying on a single number.

This is one reason Deep Learning can be useful.

---

# 31. Tools Used to Observe Network Traffic

One useful tool we may encounter during this project is:

**Wireshark**

It allows us to inspect captured network traffic.

Conceptually:

```text
Network
   ↓
Packet Capture
   ↓
Wireshark
   ↓
Inspect Packets
```

You don't need to become a Wireshark expert.

For this project, the useful concepts are:

* Captured packets
* Source/Destination
* Protocol
* Ports
* Packet length
* TCP flags
* Timing
* Conversations/flows

---

# 32. From Wireshark to IDS Dataset

Conceptually:

```text
Network
   ↓
Packet Capture
   ↓
Packet Analysis
   ↓
Flow Construction
   ↓
Feature Extraction
   ↓
Dataset
```

Many IDS datasets are generated through similar traffic-capture and feature-extraction processes, although the exact methodology differs by dataset.

This is why dataset documentation will matter later.

---

# 33. The Complete Traffic Pipeline

```text
IoT Device
     ↓
Application
     ↓
Protocol
     ↓
TCP / UDP
     ↓
IP
     ↓
Packets
     ↓
Network Flow
     ↓
Flow Features
     ↓
Dataset
     ↓
Machine Learning / Deep Learning
     ↓
Prediction
```

---

# 34. Attack Detection Pipeline

When an attack occurs:

```text
Attack
   ↓
Changes Device / Network Behavior
   ↓
Changes Traffic
   ↓
Changes Flow Characteristics
   ↓
Changes Features
   ↓
IDS Model Detects Pattern
```

Example:

```text
SYN Flood
    ↓
Many SYN packets
    ↓
Abnormal connection behavior
    ↓
High SYN count / rate
    ↓
Model learns pattern
    ↓
Attack Detection
```

---

# 35. Critical Dataset Terms You Should Recognize

When you later open an IoT IDS dataset, don't be confused by terms such as:

```text
Src IP
Dst IP
Src Port
Dst Port
Protocol
Flow Duration
Fwd Packets
Bwd Packets
Total Packets
Fwd Bytes
Bwd Bytes
Packet Length
Flow IAT
SYN Count
ACK Count
FIN Count
RST Count
Label
```

They generally describe:

```text
Who communicated?
Where?
Using what protocol/service?
For how long?
How much traffic?
How frequently?
What connection behavior?
What was the class/label?
```

---

# 36. The Most Important Mental Model

Always think:

```text
DEVICE
   ↓
COMMUNICATION
   ↓
PACKETS
   ↓
FLOW
   ↓
FEATURES
   ↓
DATASET
   ↓
MODEL
   ↓
PREDICTION
```

And for attacks:

```text
ATTACK
   ↓
ABNORMAL BEHAVIOR
   ↓
ABNORMAL TRAFFIC
   ↓
ABNORMAL FEATURES
   ↓
MODEL
   ↓
ATTACK DETECTED
```

---

# 37. Phase 1 Complete Connection

The complete networking foundation is:

```text
                    IoT Device
                        ↓
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
                      Packets
                        ↓
                       Flows
                        ↓
             ┌──────────┴──────────┐
             ↓                     ↓
       Normal Traffic         Attack Traffic
             ↓                     ↓
             └──────────┬──────────┘
                        ↓
                  Flow Features
                        ↓
                   IDS Dataset
                        ↓
                 Deep Learning
                        ↓
                Intrusion Detection
```

---

# 38. Phase 1 — What You Must Be Able to Understand

By the end of Phase 1, you should understand:

### Networking Foundation

* Network
* Protocol
* TCP/IP model
* Encapsulation
* Decapsulation
* IP address
* MAC address
* Port
* Socket
* Packet
* Frame
* Segment
* Datagram
* Routing

### Transport

* TCP
* TCP connection
* Three-way handshake
* SYN
* ACK
* FIN
* RST
* PSH
* UDP
* TCP vs UDP

### Traffic

* Network traffic
* Packet-level data
* Flow-level data
* Five-tuple
* Uni-flow
* Bi-flow
* Forward traffic
* Backward traffic

### Flow Features

* Flow duration
* Packet count
* Byte count
* Packet length
* Packet rate
* Byte rate
* Inter-arrival time
* IAT statistics
* TCP flag counts

### IDS Connection

* Normal traffic vs attack traffic
* How attacks change traffic
* How traffic becomes features
* How features become dataset rows
* How an IDS model learns traffic patterns

---

# 39. Final Project Connection

The entire Phase 1 can be summarized as:

```text
IoT Device
      ↓
Generates Data
      ↓
Application Protocol
      ↓
TCP / UDP
      ↓
IP Network
      ↓
Packets
      ↓
Network Flow
      ↓
Flow Features
      ↓
IDS Dataset
      ↓
Deep Learning Model
      ↓
Benign / Attack
```

The key idea is:

> **We are not studying networking just for networking. We are studying it so that when we encounter network-traffic features inside IoT IDS datasets, we understand what those features represent, how they are generated, and why an attack might change them.**

---

# 🔥 PHASE 1 FINAL CHECKPOINT

Before moving to Phase 2, you should be able to look at something like:

```text
Flow Duration
Tot Fwd Pkts
Tot Bwd Pkts
TotLen Fwd Pkts
TotLen Bwd Pkts
Flow IAT Mean
Flow IAT Std
SYN Flag Count
ACK Flag Count
Dst Port
Protocol
Label
```

and explain the meaning of every concept without treating them as random dataset columns.

Once that is comfortable, **Phase 1 is complete**.

Next:

```text
PHASE 2
IoT Communication Protocols

MQTT
HTTP / HTTPS
DNS
ICMP
ARP
```

and we'll study them specifically from the perspective of **IoT communication + network traffic + intrusion detection**.
