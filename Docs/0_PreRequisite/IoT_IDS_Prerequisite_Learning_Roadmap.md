# 🔐 Deep Learning Based Intrusion Detection for IoT Devices

## Prerequisite Learning Roadmap

---

## Phase 0 — IoT Fundamentals

* IoT ecosystem and architecture
* IoT devices, sensors, actuators, gateways and servers
* How IoT devices collect, process and transmit data
* Basic IoT communication flow
* How normal IoT network traffic is generated

↓

## Phase 1 — Computer Networking

* Network fundamentals
* IP & MAC addresses
* Ports and sockets
* Packets, frames and network flows
* Uni-flow vs Bi-flow
* TCP/IP model
* TCP and UDP
* TCP 3-way handshake
* TCP flags
* Basic routing and communication
* Network traffic characteristics and flow features

↓

## Phase 2 — IoT Communication Protocols

Focus only on protocols relevant to the project:

* MQTT
* HTTP / HTTPS
* DNS
* TCP / UDP
* IP
* ICMP
* ARP

Understand how each protocol works and what normal traffic looks like.

↓

## Phase 3 — IoT Security & Attack Fundamentals

Study attacks relevant to IoT network intrusion detection:

* DoS / DDoS
* SYN Flood
* UDP Flood
* ICMP Flood
* Port Scanning
* Host Discovery / Ping Sweep
* Brute Force
* ARP Spoofing
* DNS Spoofing
* SQL Injection
* XSS
* Command Injection
* IoT Botnets
* Mirai

For every attack understand:

* How the attack works
* What the attacker does
* How normal traffic changes
* What network behavior/features can reveal the attack

↓

## Phase 4 — Intrusion Detection Fundamentals

* IDS concept
* Network IDS (NIDS)
* Signature-based detection
* Anomaly-based detection
* ML-based IDS
* DL-based IDS
* Binary classification
* Multi-class classification
* TP, TN, FP, FN
* Accuracy
* Precision
* Recall
* F1-score
* Macro F1
* Confusion Matrix

↓

## Phase 5 — Connect Everything

```text
IoT Device
    ↓
IoT Application
    ↓
Communication Protocol
    ↓
TCP / UDP
    ↓
IP Network
    ↓
Packets / Flows
    ↓
Network Features
    ↓
Normal or Attack Traffic
    ↓
IDS Dataset
    ↓
ML / Deep Learning
    ↓
Intrusion Detection
```

↓

## Phase 6 — IoT IDS Dataset Research

Only after completing the above:

* Research existing IoT IDS datasets
* Study dataset documentation
* Read original dataset papers
* Understand devices and protocols
* Understand attack categories
* Understand dataset size
* Understand features
* Understand traffic granularity
* Understand labels
* Analyze class imbalance
* Identify dataset limitations
* Compare datasets
* Select the most suitable datasets for the project

↓

## Phase 7 — Start the Main Project

```text
Dataset Selection
      ↓
Dataset Understanding
      ↓
Preprocessing
      ↓
EDA
      ↓
Traditional ML Baselines
      ↓
MLP
      ↓
CNN
      ↓
LSTM
      ↓
GRU
      ↓
CNN-LSTM
      ↓
CNN-BiLSTM
      ↓
CNN-BiLSTM-Attention
      ↓
Hyperparameter Optimization
      ↓
Feature Selection
      ↓
Class Imbalance Experiments
      ↓
Ablation Study
      ↓
Generalization / Unseen Attack Testing
      ↓
Explainable AI
      ↓
Final Model Comparison
      ↓
🏆 Best Model Selection
      ↓
Final Research Analysis
```

---

## 🎯 Learning Goal Before Dataset Research

```text
Understand IoT Devices
        +
Understand How They Communicate
        +
Understand Computer Networking
        +
Understand IoT Protocols
        +
Understand IoT Attacks
        +
Understand IDS
        ↓
Understand How Attacks Change Network Traffic
        ↓
Ready for Dataset Research
```

**Rule:** Learn only the concepts that help us understand the chain:

`IoT Device → Network → Traffic → Attack → Dataset → Deep Learning → IDS`
