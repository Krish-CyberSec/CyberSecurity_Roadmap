# Lesson 14 - TCP Flags & Connection States

# Labs

---

# Lab 01 — Identify TCP Flags

## Objective

Understand what the major TCP flags indicate.

Match each flag with its basic purpose.

```text
SYN
ACK
FIN
RST
PSH
URG
```

Complete:

| Flag | Purpose |
| ---- | ------- |
| SYN  |         |
| ACK  |         |
| FIN  |         |
| RST  |         |
| PSH  |         |
| URG  |         |

---

# Lab 02 — TCP Three-Way Handshake

## Objective

Recognize normal TCP connection establishment.

Complete the sequence:

```text
Client
   |
   | __________
   v
Server
   |
   | __________
   v
Client
   |
   | __________
   v
Server
```

Then answer:

1. What state does the client enter after sending the SYN?
2. What state does the server enter after receiving the SYN and sending SYN-ACK?
3. What state does the connection reach after the final ACK?

---

# Lab 03 — TCP Connection States

Match each state to its description.

### States

```text
LISTEN
SYN-SENT
SYN-RECEIVED
ESTABLISHED
CLOSE-WAIT
FIN-WAIT
LAST-ACK
TIME-WAIT
CLOSED
```

### Descriptions

```text
A. Waiting for an incoming connection
B. Client has sent SYN and is waiting for response
C. Server has received SYN and sent SYN-ACK
D. Active connection
E. Remote side has closed its sending direction
F. Local side has initiated termination
G. Waiting for final ACK after sending FIN
H. Post-termination state
I. Connection no longer exists
```

Create the correct mapping.

---

# Lab 04 — Read the TCP Story

Analyze:

```text
SYN
SYN-ACK
ACK
PSH-ACK
ACK
PSH-ACK
ACK
FIN
ACK
FIN
ACK
```

### Questions

1. What happened first?
2. When was the connection established?
3. When was application data exchanged?
4. How was the connection closed?
5. Was the termination graceful?

---

# Lab 05 — Closed Port

Analyze:

```text
Scanner → SYN → Target
Target  → RST → Scanner
```

### Questions

1. What does the RST suggest?
2. Is the host likely reachable?
3. Is the port likely open or closed?
4. Could RST by itself prove malicious activity?

---

# Lab 06 — SYN Scan

Analyze:

```text
Scanner → SYN
Target  → SYN-ACK
Scanner → RST
```

### Questions

1. What type of scan does this resemble?
2. Why doesn't the scanner send the final ACK?
3. What can the scanner learn?
4. Why is understanding TCP flags useful here?

---

# Lab 07 — SYN Flood Investigation

## Scenario

A web server suddenly receives:

```text
SYN
SYN
SYN
SYN
SYN
SYN
...
```

Thousands of times per second.

Many connections remain incomplete.

### Questions

1. What attack could this pattern represent?
2. What resource could be affected?
3. What other explanations should you rule out?
4. What network telemetry would you investigate?
5. Which security controls might be relevant?

---

# Lab 08 — RST Investigation

## Scenario

A server shows:

```text
SYN
SYN-ACK
ACK
Data
RST
```

### Questions

1. What does the RST tell you?
2. Does it identify the cause?
3. What host/application evidence would you investigate?
4. Could a firewall/security device have caused it?
5. Could an application have caused it?

---

# Lab 09 — FIN Investigation

Analyze:

```text
FIN
ACK
FIN
ACK
```

### Questions

1. What does FIN indicate?
2. Is this graceful or abrupt termination?
3. Why are two FINs involved?
4. Why doesn't the first FIN necessarily terminate both directions immediately?

---

# Lab 10 — TIME-WAIT Investigation

## Scenario

A Linux server currently has:

```text
8,500 TIME-WAIT sockets
```

### Questions

1. Is TIME-WAIT automatically malicious?
2. What does TIME-WAIT indicate?
3. What could cause a large number of TIME-WAIT connections?
4. When might the condition become operationally important?

---

# Lab 11 — CLOSE-WAIT Investigation

A server shows:

```text
3,000 CLOSE-WAIT connections
```

### Questions

1. What does CLOSE-WAIT mean?
2. Which side has already closed its sending direction?
3. Why might a large number of CLOSE-WAIT sockets indicate an application problem?
4. What process/application information would you inspect?

---

# Lab 12 — Packet Direction Analysis

Analyze:

```text
Client                          Server

SYN
------------------------------>

             SYN-ACK
<------------------------------

ACK
------------------------------>

PSH-ACK
------------------------------>

             ACK
<------------------------------
```

### Questions

1. Which host initiated the connection?
2. Which host responded?
3. Which packet indicates the server accepted the connection request?
4. Which packet carries application data?
5. Which flag combination would you expect to see during normal data transfer?

---

# Lab 13 — Wireshark Practice

## Objective

Use packet captures to identify TCP connection stages.

In your authorized lab capture:

Find one TCP conversation containing:

```text
SYN
SYN-ACK
ACK
```

Record:

```text
Source IP:
Source Port:
Destination IP:
Destination Port:
```

Then find:

```text
FIN
```

or:

```text
RST
```

and determine how the connection ended.

---

# Lab 14 — Wireshark TCP Flags

For one TCP conversation, identify packets containing:

```text
SYN
ACK
PSH
FIN
RST
```

Complete:

| Packet | Flags | Meaning |
| ------ | ----- | ------- |
| 1      |       |         |
| 2      |       |         |
| 3      |       |         |
| 4      |       |         |
| 5      |       |         |

Then write a short explanation of the connection's story.

---

# Lab 15 — Port Scan Detection

## Scenario

A workstation sends:

```text
10.10.10.20:22   SYN
10.10.10.20:80   SYN
10.10.10.20:443  SYN
10.10.10.20:445  SYN

10.10.10.21:22   SYN
10.10.10.21:80   SYN
10.10.10.21:443  SYN
10.10.10.21:445  SYN

10.10.10.22:22   SYN
...
```

### Questions

1. What behavior does this resemble?
2. What makes it suspicious?
3. Could the behavior be legitimate?
4. What would you check to confirm that?
5. Which host/process created the traffic?

---

# Lab 16 — SYN Scan vs SYN Flood

Create a comparison:

| Feature                 | SYN Scan | SYN Flood |
| ----------------------- | -------- | --------- |
| Primary goal            |          |           |
| Typical traffic pattern |          |           |
| Connection completion   |          |           |
| Security impact         |          |           |
| Possible legitimate use |          |           |

---

# Lab 17 — Security Investigation

## Scenario

A SOC alert says:

> "A workstation generated an unusual amount of TCP SYN traffic."

### Investigation

Follow:

```text
Source IP
   ↓
Source Process
   ↓
Destination IPs
   ↓
Destination Ports
   ↓
TCP Flags
   ↓
Connection States
   ↓
Traffic Volume
   ↓
Time Window
   ↓
Expected Behavior
```

### Questions

1. What process generated the traffic?
2. Were destinations internal or external?
3. Were many ports targeted?
4. Were connections completed?
5. Is the source an approved security scanner?
6. Is there evidence of malicious behavior?

---

# Lab 18 — GRC Investigation

## Scenario

An incident report states:

> "The production environment received thousands of incomplete TCP connections."

### Questions

As a GRC analyst:

1. Was this activity authorized?
2. Was a security scanner involved?
3. Which control detected it?
4. Did availability suffer?
5. What response procedure was followed?
6. Was the incident documented?
7. Are preventive controls adequate?
8. Are improvements required?

---

# Lab 19 — TCP Flag Storytelling

For each sequence, explain the network story.

### Sequence A

```text
SYN
SYN-ACK
ACK
```

### Sequence B

```text
SYN
RST
```

### Sequence C

```text
SYN
SYN-ACK
RST
```

### Sequence D

```text
FIN
ACK
FIN
ACK
```

### Sequence E

```text
Data
RST
```

Write:

```text
Sequence:
Interpretation:
Confidence:
Additional evidence required:
```

---

# Lab 20 — Final Integrated Scenario

You are investigating a server:

```text
Server:
10.10.10.50
```

Traffic observed:

```text
Client A → SYN → 10.10.10.50:443
Server   → SYN-ACK
Client A → ACK
Client A → PSH-ACK
Server   → ACK
Client A → FIN
Server   → ACK
Server   → FIN
Client A → ACK
```

At the same time:

```text
Client B → SYN → 10.10.10.50:22
Server   → RST

Client C → SYN → 10.10.10.50:443
Server   → SYN-ACK
Client C → RST
```

### Task

Explain each conversation.

### Conversation A

```text
Client A → 443
```

Determine:

```text
Connection state:
Data transfer:
Termination:
```

### Conversation B

```text
Client B → 22
```

Determine:

```text
Likely port state:
Reason:
```

### Conversation C

```text
Client C → 443
```

Determine:

```text
Likely activity:
Why?
```

---

# Lab 21 — Final Packet Analysis Challenge

Without looking at your notes, analyze:

```text
1. SYN
2. SYN-ACK
3. ACK
4. PSH-ACK
5. ACK
6. PSH-ACK
7. ACK
8. RST
```

Answer:

1. Was the connection established?
2. Was data exchanged?
3. Was the connection gracefully terminated?
4. How did the connection end?
5. What additional evidence would you want to understand why?

---

# Lab Completion Checklist

* [ ] I can identify SYN.
* [ ] I can identify ACK.
* [ ] I can identify FIN.
* [ ] I can identify RST.
* [ ] I understand PSH.
* [ ] I understand URG.
* [ ] I understand LISTEN.
* [ ] I understand SYN-SENT.
* [ ] I understand SYN-RECEIVED.
* [ ] I understand ESTABLISHED.
* [ ] I understand FIN-WAIT.
* [ ] I understand CLOSE-WAIT.
* [ ] I understand LAST-ACK.
* [ ] I understand TIME-WAIT.
* [ ] I understand CLOSED.
* [ ] I can recognize the TCP three-way handshake.
* [ ] I can recognize graceful TCP termination.
* [ ] I can recognize reset behavior.
* [ ] I understand SYN scanning.
* [ ] I understand SYN flooding.
* [ ] I can read a basic TCP packet sequence.
* [ ] I practiced TCP analysis in Wireshark.
* [ ] I can investigate suspicious TCP behavior as a SOC analyst.
* [ ] I can explain TCP findings from a GRC perspective.

---

# Final Reflection

Write your own answer:

> **If you saw a TCP conversation in Wireshark containing SYN, SYN-ACK, ACK, data packets, and FIN/ACK packets, how would you explain the complete story of that connection?**

Do not copy the README.

The goal is to explain the network behavior in your own words.
