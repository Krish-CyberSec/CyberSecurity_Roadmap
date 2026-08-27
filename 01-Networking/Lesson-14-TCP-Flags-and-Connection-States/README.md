# Lesson 14 - TCP Flags, Connection States & Packet Analysis

## Module Information

| Field          | Details                                        |
| -------------- | ---------------------------------------------- |
| Module         | Networking for Cybersecurity                   |
| Lesson         | 14                                             |
| Topic          | TCP Flags, Connection States & Packet Analysis |
| Difficulty     | Intermediate                                   |
| OSI Layer      | Layer 4 – Transport                            |
| Prerequisites  | Lessons 01–13                                  |
| TryHackMe Room | Wireshark 101                                  |
| Status         | Completed                                      |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Explain the purpose of important TCP flags.
* Understand the relationship between TCP flags and TCP connection states.
* Explain the TCP connection-establishment process.
* Understand the TCP connection-termination process.
* Distinguish graceful termination from a TCP reset.
* Understand important TCP states such as:

  * LISTEN
  * SYN-SENT
  * SYN-RECEIVED
  * ESTABLISHED
  * FIN-WAIT
  * CLOSE-WAIT
  * LAST-ACK
  * TIME-WAIT
  * CLOSED
* Understand how TCP flags appear during port scanning.
* Distinguish SYN scanning from SYN flooding.
* Interpret basic TCP packet sequences.
* Use packet sequences to understand the story of a TCP connection.
* Apply TCP knowledge to SOC investigations.
* Understand why TCP behavior can be relevant to GRC and security monitoring.

---

# Part 1 — TCP Flags

## 1. What Are TCP Flags?

TCP segments contain control information represented by flags.

These flags help endpoints communicate information about the state or handling of a TCP connection.

The important flags for this lesson are:

```text
SYN
ACK
FIN
RST
PSH
URG
```

The most important ones to understand first are:

```text
SYN
ACK
FIN
RST
```

---

# 2. SYN

`SYN` is associated with TCP connection establishment and sequence-number synchronization.

When a client wants to establish a TCP connection:

```text
Client
  |
  | SYN
  v
Server
```

Conceptually, the client is saying:

> "I want to establish a TCP connection."

---

# 3. ACK

`ACK` is used for acknowledgement-related signaling.

During the TCP three-way handshake:

```text
Client → SYN
Server → SYN-ACK
Client → ACK
```

The final ACK helps complete the connection establishment.

ACK is not limited to the handshake. TCP also uses acknowledgement mechanisms during normal data transfer.

---

# 4. SYN-ACK

A TCP segment can contain both:

```text
SYN
ACK
```

during connection establishment.

The server responds to the client's SYN with:

```text
SYN-ACK
```

Conceptually:

```text
Client
  |
  | SYN
  v
Server
  |
  | SYN-ACK
  v
Client
```

The server is acknowledging the received SYN while also participating in its own connection establishment.

---

# 5. FIN

`FIN` is associated with graceful TCP connection termination.

A system can send:

```text
FIN
```

when it has finished sending data.

Conceptually:

```text
Client
  |
  | FIN
  v
Server
```

FIN does not mean that the entire TCP connection disappears immediately.

Because TCP is full-duplex, one side can finish sending while the other side continues sending.

---

# 6. RST

`RST` means reset.

It is used to abruptly reset or reject a TCP connection.

For example:

```text
Client
  |
  | SYN
  v
Server
  |
  | RST
  v
Client
```

A common interpretation during port scanning is:

```text
SYN → RST
```

which can indicate that the host is reachable but the destination port is closed.

However, RST can occur for many reasons, so it must be interpreted in context.

---

# 7. FIN vs RST

These should not be confused.

### FIN

```text
"I am finished sending."
```

Represents graceful termination.

### RST

```text
"Reset/abort this connection."
```

Represents an abrupt reset.

Mental model:

```text
FIN → Graceful
RST → Abrupt
```

---

# 8. PSH

`PSH` stands for Push.

At a high level, PSH is associated with making received data available to the application without unnecessary delay.

You may see:

```text
PSH, ACK
```

during application data transfer.

For this lesson, remember:

> PSH is associated with pushing received data toward the application.

---

# 9. URG

`URG` indicates that urgent-pointer information is significant.

It is less important for our current learning stage than:

```text
SYN
ACK
FIN
RST
```

So the basic mental model is:

```text
SYN → Connection establishment

ACK → Acknowledgement

FIN → Graceful termination

RST → Reset

PSH → Push data toward application

URG → Urgent-pointer information
```

---

# Part 2 — TCP Connection States

# 10. TCP as a State Machine

TCP is not simply:

```text
OPEN
CLOSED
```

It uses connection states to keep track of what is happening during the lifecycle of a connection.

A simplified lifecycle is:

```text
Connection Start
      ↓
Connection Established
      ↓
Data Transfer
      ↓
Connection Termination
      ↓
Connection Closed
```

---

# 11. LISTEN

A server waiting for incoming TCP connections can be in:

```text
LISTEN
```

For example:

```text
10.10.10.50:22/TCP
```

with an SSH service waiting for connections.

Conceptually:

```text
Server
  |
  ▼
TCP 22
  |
  ▼
LISTEN
  |
  ▼
Waiting for connection
```

This connects directly to Lesson 13, where we learned about listening ports.

---

# 12. SYN-SENT

When a client sends a SYN:

```text
Client
  |
  | SYN
  v
Server
```

the client can enter:

```text
SYN-SENT
```

The client is waiting for the server's response.

---

# 13. SYN-RECEIVED

When the server receives the client's SYN and responds with SYN-ACK, the server can enter:

```text
SYN-RECEIVED
```

It is waiting for the client's final acknowledgement.

Conceptually:

```text
Client                         Server
  |                              |
  | SYN                          |
  |----------------------------->|
  |                              |
  |                         SYN-RECEIVED
  |                              |
  | SYN-ACK                      |
  |<-----------------------------|
```

---

# 14. ESTABLISHED

After the client receives SYN-ACK and sends ACK:

```text
Client
  |
  | ACK
  v
Server
```

the connection can enter:

```text
ESTABLISHED
```

This is the normal active state for TCP data transfer.

```text
Client
  |
  |====== Data ======>
  |
Server
```

---

# 15. The Establishment Sequence

The simplified sequence is:

```text
LISTEN
   ↓
SYN
   ↓
SYN-SENT / SYN-RECEIVED
   ↓
ACK
   ↓
ESTABLISHED
```

This also explains why the TCP handshake is relevant to port scanning.

---

# 16. FIN-WAIT-1

When an endpoint sends its FIN:

```text
FIN
```

it can enter:

```text
FIN-WAIT-1
```

The endpoint is waiting for the peer to acknowledge the FIN.

Conceptually:

```text
Application finishes
       ↓
FIN sent
       ↓
FIN-WAIT-1
```

---

# 17. FIN-WAIT-2

After the FIN is acknowledged, the endpoint may enter:

```text
FIN-WAIT-2
```

It has finished sending data but may still receive data from the peer.

This happens because TCP communication is full-duplex.

One direction can be closed while the other direction remains active.

---

# 18. CLOSE-WAIT

When one side receives a FIN from the peer, it can enter:

```text
CLOSE-WAIT
```

This means:

> The remote endpoint has finished sending, but the local application has not completely closed its own side.

Conceptually:

```text
Remote
  |
  | FIN
  v
Local
  |
  ▼
CLOSE-WAIT
```

---

# 19. LAST-ACK

After the local application finishes and sends its own FIN, the endpoint can enter:

```text
LAST-ACK
```

It waits for the final acknowledgement.

Conceptually:

```text
CLOSE-WAIT
      ↓
Local application finishes
      ↓
FIN
      ↓
LAST-ACK
      ↓
Final ACK
```

---

# 20. TIME-WAIT

After TCP termination, an endpoint may enter:

```text
TIME-WAIT
```

This is a normal TCP state that helps ensure delayed packets from the old connection do not interfere with future communication and supports safe connection termination.

For this lesson, remember:

> TIME-WAIT is a normal post-termination TCP state.

---

# 21. CLOSED

Eventually the connection reaches:

```text
CLOSED
```

The TCP connection no longer exists.

---

# 22. Simplified TCP Lifecycle

```text
                    SERVER
                     LISTEN
                       |
                       |
                 SYN / SYN-ACK
                       |
                       ▼
                 ESTABLISHED
                       |
                       |
                  Data Transfer
                       |
                       ▼
                 Graceful Close
                       |
              ┌────────┴────────┐
              ▼                 ▼
          FIN-WAIT          CLOSE-WAIT
              |                 |
              └────────┬────────┘
                       ▼
                   LAST-ACK
                       |
                       ▼
                   TIME-WAIT
                       |
                       ▼
                    CLOSED
```

The real TCP state machine contains more transitions and depends on which endpoint initiates or responds to the close.

---

# Part 3 — TCP Connection Termination

# 23. Why Doesn't FIN Immediately Close Everything?

TCP is full-duplex.

That means:

```text
Client → Server
```

and:

```text
Server → Client
```

are logically independent directions.

The client can say:

> "I'm finished sending."

while the server still has data to send.

Therefore:

```text
FIN
≠
Entire connection instantly disappears
```

---

# 24. Graceful Termination

A simplified graceful shutdown looks like:

```text
Client                         Server
  |                              |
  | FIN                          |
  |----------------------------->|
  |                              |
  | ACK                          |
  |<-----------------------------|
  |                              |
  |          Data                |
  |<-----------------------------|
  |                              |
  | FIN                          |
  |<-----------------------------|
  |                              |
  | ACK                          |
  |----------------------------->|
```

This is often described conceptually as a four-way close:

```text
FIN
ACK
FIN
ACK
```

Some packets may combine flags depending on timing and implementation.

---

# 25. FIN-WAIT Process

The endpoint sending the first FIN can move through:

```text
FIN-WAIT-1
      ↓
FIN-WAIT-2
```

while waiting for the other side to complete its side of the shutdown.

---

# 26. CLOSE-WAIT Process

The endpoint receiving the first FIN may enter:

```text
CLOSE-WAIT
```

It has acknowledged the remote endpoint's shutdown, but its own application has not yet finished.

After its application finishes:

```text
CLOSE-WAIT
      ↓
FIN
      ↓
LAST-ACK
```

---

# 27. TIME-WAIT and Operational Behavior

A system can have many TCP connections in:

```text
TIME-WAIT
```

This is not automatically malicious.

A large number can simply indicate that the system recently handled many short-lived TCP connections.

In some environments, a very large TIME-WAIT population may become relevant to resource or performance analysis.

---

# 28. RST vs Graceful Close

A graceful close uses:

```text
FIN
ACK
FIN
ACK
```

A reset may simply use:

```text
RST
```

RST is appropriate for situations where the connection needs to be aborted or rejected rather than gracefully shut down.

---

# 29. RST During Port Scanning

A scanner sends:

```text
SYN
```

and the target responds:

```text
RST
```

A common interpretation is:

```text
Host reachable
+
No listening TCP service
=
Port likely closed
```

This is why RST is important when understanding Nmap.

---

# Part 4 — TCP Flags in Security & Packet Analysis

# 30. Normal TCP Connection

A normal sequence can look like:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
Data
 ↓
FIN
 ↓
ACK
 ↓
FIN
 ↓
ACK
```

The story is:

```text
Connection established
      ↓
Data exchanged
      ↓
Connection gracefully closed
```

---

# 31. SYN Scanning

A SYN scan can produce:

```text
SYN
 ↓
SYN-ACK
 ↓
RST
```

The scanner is probing the target without completing the normal TCP connection.

This can help determine whether a TCP service is reachable.

---

# 32. SYN Flood

A SYN flood has a different purpose.

Conceptually:

```text
SYN
SYN
SYN
SYN
SYN
...
```

The server responds with:

```text
SYN-ACK
SYN-ACK
SYN-ACK
...
```

but many connections never complete normally.

At scale, this can consume server resources and affect availability.

The important distinction is:

```text
SYN Scan
→ Reconnaissance

SYN Flood
→ Availability attack
```

---

# 33. Why Patterns Matter

A single:

```text
SYN
```

doesn't tell you much.

But:

```text
SYN
SYN
SYN
SYN
...
```

to many destinations and ports tells a much more interesting story.

Security analysts consider:

* Source
* Destination
* Port
* Frequency
* Timing
* State
* Previous traffic
* Later traffic
* Host context

---

# 34. TCP Flags in Firewall Logs

A firewall or security tool may record:

```text
SRC=192.168.1.45
DST=172.217.22.14
SPORT=51432
DPORT=443
PROTO=TCP
FLAGS=SYN
```

Interpretation:

```text
Source IP:
192.168.1.45

Source Port:
51432

Destination:
172.217.22.14:443

Protocol:
TCP

Flag:
SYN
```

This indicates a TCP connection attempt toward the destination.

---

# 35. Packet Analysis Is About the Story

Consider:

```text
Packet 1 → SYN
Packet 2 → SYN-ACK
Packet 3 → ACK
Packet 4 → PSH-ACK
Packet 5 → PSH-ACK
Packet 6 → ACK
Packet 7 → FIN
Packet 8 → ACK
Packet 9 → FIN
Packet 10 → ACK
```

Don't memorize ten packets.

Read the story:

```text
Connection established
        ↓
Application data exchanged
        ↓
Connection gracefully terminated
```

---

# 36. Suspicious Scanning Pattern

Suppose a workstation sends:

```text
Host A:22   SYN
Host A:80   SYN
Host A:443  SYN
Host A:445  SYN

Host B:22   SYN
Host B:80   SYN
Host B:443  SYN
Host B:445  SYN

Host C:22   SYN
...
```

This could represent:

```text
Port scanning / reconnaissance
```

But it could also be:

```text
Authorized vulnerability scanning
Monitoring
Security tooling
```

Therefore:

> **Traffic pattern is evidence, not an automatic verdict.**

---

# 37. RST Patterns

You may see:

```text
Data
Data
RST
```

This tells you the connection was reset.

Possible reasons include:

* Application behavior
* Service failure
* Firewall/security device
* Network problems
* Invalid connection state
* Port/service behavior
* Malicious activity

You need additional context.

---

# 38. Why TCP Flags Matter to SOC

A SOC analyst may use TCP flags to understand:

```text
Connection attempts
Connection establishment
Data exchange
Connection termination
Scanning
Resets
Potential denial-of-service activity
```

This is why Layer 4 knowledge becomes useful during:

* Alert triage
* Incident response
* Threat hunting
* Network detection
* Packet analysis

---

# 39. Why TCP Flags Matter to Red Team

During authorized security testing, understanding TCP behavior helps with:

```text
Port scanning
Firewall behavior
Service discovery
Network reconnaissance
Filtering analysis
```

The important idea is:

> TCP responses provide information about how a target behaves.

---

# 40. GRC Perspective

A GRC analyst may encounter technical reports containing:

> "The production server received a high rate of incomplete TCP connection attempts."

Understanding TCP states allows the analyst to ask better questions:

```text
Was the source authorized?
Was this a vulnerability scan?
Was the event detected?
Which control detected it?
Was availability affected?
Was the incident investigated?
```

Technical knowledge improves governance questions.

---

# 41. TryHackMe Practice

## Room

**Wireshark 101**

Focus on:

```text
TCP Traffic
```

and packet dissection.

Look for:

```text
SYN
SYN-ACK
ACK
PSH-ACK
FIN
RST
```

Try to identify:

```text
Connection establishment
Data transfer
Connection termination
Reset behavior
```

The purpose is not to memorize packet numbers.

The purpose is to recognize:

> **What is happening to this TCP connection?**

---

# Part 5 — Final Assessment

# 42. Scenario A — Normal Connection

You observe:

```text
1. Client → Server    SYN
2. Server → Client    SYN-ACK
3. Client → Server    ACK
4. Client → Server    PSH-ACK
5. Server → Client    ACK
6. Server → Client    PSH-ACK
7. Client → Server    ACK
8. Client → Server    FIN
9. Server → Client    ACK
10. Server → Client   FIN
11. Client → Server   ACK
```

Interpret the sequence.

### Expected reasoning

```text
SYN
 ↓
Connection establishment

SYN-ACK
 ↓
Server response

ACK
 ↓
Connection established

PSH-ACK / ACK
 ↓
Application data

FIN / ACK / FIN / ACK
 ↓
Graceful termination
```

Overall:

> Normal TCP connection → data exchange → graceful close.

---

# 43. Scenario B — Closed Port

```text
Scanner → SYN
Target  → RST
```

Likely interpretation:

```text
Host reachable
+
No listening TCP service
=
Port likely closed
```

---

# 44. Scenario C — SYN Scan

```text
Scanner → SYN
Target  → SYN-ACK
Scanner → RST
```

Likely interpretation:

> The scanner is probing for a TCP service without completing the connection.

This is consistent with a SYN scan.

---

# 45. Scenario D — SYN Flood

```text
SYN
SYN
SYN
SYN
SYN
...
```

to a server, with many partially established connections.

Possible interpretation:

> Potential SYN flood or other large-scale incomplete connection activity.

Further investigation is required.

---

# 46. Scenario E — Graceful Termination

```text
FIN
ACK
FIN
ACK
```

Likely interpretation:

> An orderly TCP connection close.

---

# 47. Scenario F — RST

```text
Data
Data
Data
RST
```

What can you safely conclude?

> The TCP connection was reset or aborted.

You cannot conclude from RST alone why it happened.

---

# 48. Scenario G — Scanning

A workstation sends SYN packets to hundreds of hosts across ports:

```text
22
80
443
445
3389
```

Possible interpretation:

> Network reconnaissance or port scanning.

Before treating it as malicious, investigate whether the source is an authorized scanner or security tool.

---


# 50. OSI Model Mapping

Lesson 14 remains at:

```text
Layer 4 — Transport
    |
    ├── TCP
    ├── UDP
    ├── Ports
    ├── Sockets
    ├── TCP Flags
    └── TCP States
```

The lower layers carry the TCP information:

```text
TCP Segment
    ↓
IP Packet
    ↓
Ethernet Frame
    ↓
Physical Transmission
```

---

# Key Takeaways

* TCP uses flags to communicate connection-control information.
* SYN is associated with connection establishment.
* ACK is used for acknowledgement-related signaling.
* FIN is associated with graceful termination.
* RST resets or aborts a connection.
* PSH is associated with pushing data toward the application.
* URG indicates urgent-pointer significance.
* TCP uses a state machine to manage connection lifecycle.
* LISTEN indicates a service is waiting for incoming TCP connections.
* SYN-SENT and SYN-RECEIVED are connection-establishment states.
* ESTABLISHED represents normal active TCP communication.
* FIN-WAIT and CLOSE-WAIT occur during connection termination.
* LAST-ACK and TIME-WAIT are normal termination states.
* RST and FIN have very different meanings.
* SYN scans use TCP connection-establishment behavior for reconnaissance.
* SYN floods exploit incomplete TCP connection establishment to affect availability.
* TCP flags should always be interpreted in context.
* Packet analysis is about understanding the sequence and story of communication.
* TCP behavior is useful to SOC analysts, Red Teamers, Security Engineers, and GRC professionals.

---

# Lesson 14 Completion Checklist

Before moving on, you should be able to:

* [ ] Explain SYN.
* [ ] Explain ACK.
* [ ] Explain FIN.
* [ ] Explain RST.
* [ ] Explain PSH at a basic level.
* [ ] Explain URG at a basic level.
* [ ] Explain LISTEN.
* [ ] Explain SYN-SENT.
* [ ] Explain SYN-RECEIVED.
* [ ] Explain ESTABLISHED.
* [ ] Explain FIN-WAIT.
* [ ] Explain CLOSE-WAIT.
* [ ] Explain LAST-ACK.
* [ ] Explain TIME-WAIT.
* [ ] Explain CLOSED.
* [ ] Explain graceful TCP termination.
* [ ] Explain why RST differs from FIN.
* [ ] Understand SYN scanning.
* [ ] Understand SYN flooding.
* [ ] Read a basic TCP packet sequence.
* [ ] Explain what the sequence says about the connection.
* [ ] Connect TCP behavior to SOC investigations.
* [ ] Connect TCP behavior to GRC/security monitoring.

---

# Summary

TCP is a stateful transport protocol whose behavior can be understood through its flags and connection states.

The connection begins with:

```text
SYN
SYN-ACK
ACK
```

and can proceed through:

```text
ESTABLISHED
```

before eventually closing using FIN-based termination or being reset with RST.

Understanding these flags and states makes it possible to interpret packet captures, recognize normal connection behavior, understand port-scanning techniques, investigate suspicious connection patterns, and communicate more effectively with SOC, security engineering, and GRC teams.

The key skill from this lesson is not memorizing every TCP state.

It is being able to look at a sequence such as:

```text
SYN
SYN-ACK
ACK
PSH-ACK
FIN
ACK
FIN
ACK
```

and explain the **story of the TCP connection** from beginning to end.
