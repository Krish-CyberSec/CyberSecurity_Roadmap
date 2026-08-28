# Lesson 14 - TCP Flags & Connection States

# Interview Questions

---

# Beginner Level

## Q1. What are TCP flags?

**Answer:**

TCP flags are control bits in the TCP header used to communicate information about connection establishment, acknowledgement, termination, and other TCP behaviors.

Important flags include:

```text
SYN
ACK
FIN
RST
PSH
URG
```

---

## Q2. What is the purpose of the SYN flag?

**Answer:**

SYN is associated with TCP connection establishment and synchronization of sequence-number state.

---

## Q3. What is the purpose of the ACK flag?

**Answer:**

ACK is used for acknowledgement-related signaling. It is used during the TCP three-way handshake and during normal data transfer.

---

## Q4. What is the purpose of the FIN flag?

**Answer:**

FIN is associated with graceful TCP connection termination.

It indicates that an endpoint has finished sending data in that direction.

---

## Q5. What is the purpose of the RST flag?

**Answer:**

RST is used to reset or abruptly terminate a TCP connection.

---

## Q6. What is the difference between FIN and RST?

**Answer:**

```text
FIN → Graceful termination

RST → Reset / abrupt termination
```

FIN is used for an orderly shutdown, while RST is used to reset the connection.

---

## Q7. What does PSH mean?

**Answer:**

PSH stands for Push. At a high level, it indicates that the receiving TCP stack should make the relevant data available to the application without unnecessary delay.

---

## Q8. What does URG mean?

**Answer:**

URG indicates that urgent-pointer information is significant.

---

# TCP Connection States

## Q9. What is a TCP connection state?

**Answer:**

A TCP connection state indicates the current stage of a TCP connection's lifecycle.

TCP uses states to keep track of connection establishment, active communication, and termination.

---

## Q10. What does LISTEN mean?

**Answer:**

LISTEN means a TCP endpoint is waiting for an incoming connection.

For example:

```text
10.10.10.50:22
```

may be in a listening state when an SSH service is waiting for clients.

---

## Q11. What is SYN-SENT?

**Answer:**

SYN-SENT is a client-side TCP state entered after sending a SYN and waiting for a response.

---

## Q12. What is SYN-RECEIVED?

**Answer:**

SYN-RECEIVED is a state in which the server has received a SYN, responded with SYN-ACK, and is waiting for the next step in connection establishment.

---

## Q13. What is ESTABLISHED?

**Answer:**

ESTABLISHED indicates that the TCP connection has been successfully established and can be used for normal data transfer.

---

## Q14. What is FIN-WAIT?

**Answer:**

FIN-WAIT represents a state during TCP connection termination where an endpoint has initiated shutdown of its sending side and is waiting for the appropriate responses from the peer.

---

## Q15. What is CLOSE-WAIT?

**Answer:**

CLOSE-WAIT means the remote endpoint has closed its sending direction, but the local application has not yet completely closed its own side.

---

## Q16. What is LAST-ACK?

**Answer:**

LAST-ACK is a TCP termination state in which an endpoint has sent its FIN and is waiting for the peer's final acknowledgement.

---

## Q17. What is TIME-WAIT?

**Answer:**

TIME-WAIT is a normal TCP state after connection termination. It helps TCP safely complete the old connection and prevents delayed packets from interfering with a future connection.

---

## Q18. What does CLOSED mean?

**Answer:**

CLOSED means the TCP connection no longer exists.

---

# TCP Three-Way Handshake

## Q19. What is the TCP three-way handshake?

**Answer:**

The TCP three-way handshake is the connection-establishment process:

```text
SYN
SYN-ACK
ACK
```

---

## Q20. Why is the TCP handshake called a three-way handshake?

**Answer:**

Because three TCP messages are exchanged to establish the connection and synchronize relevant state between the endpoints.

---

## Q21. Explain the TCP three-way handshake.

**Answer:**

```text
Client → SYN → Server

Server → SYN-ACK → Client

Client → ACK → Server
```

After this exchange, the connection can enter the ESTABLISHED state.

---

## Q22. Does ACK only occur during the TCP handshake?

**Answer:**

No.

ACK is also used during normal TCP communication as part of TCP's acknowledgement mechanisms.

---

# TCP Connection Termination

## Q23. Why doesn't a FIN immediately close the entire TCP connection?

**Answer:**

TCP is full-duplex.

Each endpoint has an independent sending direction, so one side can finish sending while the other side still has data to send.

---

## Q24. What is the conceptual TCP termination sequence?

**Answer:**

A simplified termination sequence is:

```text
FIN
ACK
FIN
ACK
```

This is often described as a four-step TCP connection termination process.

---

## Q25. What happens after a host sends FIN?

**Answer:**

The endpoint enters a termination-related state such as FIN-WAIT and waits for the peer's response while the TCP connection is being closed.

---

## Q26. Why can a server remain in CLOSE-WAIT?

**Answer:**

A server can remain in CLOSE-WAIT because the remote side has already closed its sending direction while the local application has not yet finished closing its own side.

A large or persistent number of CLOSE-WAIT sockets can indicate application or resource-handling problems and may warrant investigation.

---

## Q27. Why do TCP connections enter TIME-WAIT?

**Answer:**

TIME-WAIT helps TCP safely complete connection termination and allows delayed packets from the old connection to expire before the same connection information can be reused.

---

# TCP Scanning

## Q28. How can TCP flags help with port scanning?

**Answer:**

TCP connection behavior provides responses that can help determine whether a port is open, closed, or filtered.

For example:

```text
SYN → SYN-ACK
```

can indicate a reachable listening service.

While:

```text
SYN → RST
```

can indicate that the TCP port is closed.

---

## Q29. What is a TCP SYN scan?

**Answer:**

A TCP SYN scan sends SYN packets and examines responses without completing the normal TCP connection.

A simplified sequence is:

```text
SYN
SYN-ACK
RST
```

---

## Q30. What is a TCP Connect Scan?

**Answer:**

A TCP Connect Scan attempts to complete the normal TCP three-way handshake.

Conceptually:

```text
SYN
SYN-ACK
ACK
```

---

## Q31. What is the difference between a SYN scan and a SYN flood?

**Answer:**

A SYN scan is a reconnaissance technique used to discover reachable TCP services.

A SYN flood is a denial-of-service technique that attempts to create large numbers of incomplete TCP connection states and consume resources.

---

# Security Analysis

## Q32. Why are TCP flags useful to a SOC analyst?

**Answer:**

TCP flags help an analyst understand what is happening during a network connection.

They can provide context about:

* Connection attempts
* Established connections
* Connection termination
* Resets
* Scanning behavior
* Other unusual traffic patterns

---

## Q33. A workstation sends SYN packets to hundreds of hosts across many ports. What might this indicate?

**Answer:**

Possible explanations include:

* Port scanning
* Authorized security scanning
* Network inventory
* Misconfiguration
* Malware
* Other unusual activity

The analyst should investigate the source process, destination hosts, ports, timing, and whether the activity is authorized.

---

## Q34. Does a large number of SYN packets automatically mean an attack?

**Answer:**

No.

Legitimate vulnerability scanners, monitoring systems, and network-management tools can also generate large amounts of SYN traffic.

Context is required.

---

## Q35. What could a large number of SYN packets with no completed connections indicate?

**Answer:**

Possible explanations include:

* Port scanning
* SYN flooding
* Network connectivity problems
* Firewall filtering
* Application issues

The analyst should examine traffic patterns and surrounding evidence.

---

## Q36. Does an RST packet mean an attack is occurring?

**Answer:**

No.

RST can be caused by:

* Closed ports
* Application behavior
* Firewall/security devices
* Invalid connection states
* Network conditions
* Malicious activity

RST is evidence that a connection was reset, not proof of why it happened.

---

## Q37. What does this sequence suggest?

```text
SYN
SYN-ACK
RST
```

**Answer:**

It is consistent with a TCP SYN scan pattern in which a scanner probes an open TCP port and then resets the connection instead of completing the handshake.

---

## Q38. What does this sequence suggest?

```text
SYN
SYN-ACK
ACK
```

**Answer:**

It indicates normal TCP connection establishment.

---

## Q39. What does this sequence suggest?

```text
FIN
ACK
FIN
ACK
```

**Answer:**

It is consistent with graceful TCP connection termination.

---

# Packet Analysis

## Q40. What should you consider when interpreting a TCP flag?

**Answer:**

Do not interpret a flag in isolation.

Consider:

```text
Source
Destination
Direction
Previous packet
Current packet
Next packet
Connection state
Traffic pattern
Application context
```

---

## Q41. Why is packet analysis more than reading individual packets?

**Answer:**

A sequence of packets provides a story about what the connection is doing.

For example:

```text
SYN
SYN-ACK
ACK
Data
FIN
ACK
FIN
ACK
```

tells a much more complete story than any single packet.

---

## Q42. How would you describe packet analysis to a beginner?

**Answer:**

Packet analysis is the process of examining network packets and their sequence to understand how communication is occurring between systems and identify unusual or suspicious behavior.

---

# GRC Questions

## Q43. Why should a GRC professional understand TCP states?

**Answer:**

GRC professionals may review:

* Security incidents
* Network-control assessments
* IDS/IPS controls
* Availability controls
* Firewall behavior
* Network monitoring

Understanding TCP states helps them understand technical findings and assess whether appropriate controls and procedures are in place.

---

## Q44. An incident report says:

> "The production server received a large number of incomplete TCP connection attempts."

What questions should a GRC analyst ask?

**Answer:**

The analyst could ask:

* Was the activity authorized?
* Was it a vulnerability scan?
* Was the source known?
* Was availability affected?
* Which control detected it?
* Was the event investigated?
* Were preventive controls available?
* Was the incident documented correctly?

---

# Scenario-Based Questions

## Q45.

You observe:

```text
Client → SYN
Server → SYN-ACK
Client → ACK
Client → PSH-ACK
Server → ACK
```

What is happening?

**Answer:**

A TCP connection has been established and application data is being exchanged.

---

## Q46.

You observe:

```text
Client → SYN
Server → RST
```

What does this commonly indicate?

**Answer:**

The target host is reachable, but the TCP port is likely closed because no service is listening there.

---

## Q47.

You observe:

```text
Client → FIN
Server → ACK
Server → FIN
Client → ACK
```

What does this indicate?

**Answer:**

It is a simplified example of graceful TCP connection termination.

---

## Q48.

A server has thousands of connections in TIME-WAIT. Is this automatically malicious?

**Answer:**

No.

TIME-WAIT is a normal TCP state. A large number may simply indicate that the system has recently handled many short-lived TCP connections.

It may become relevant if it contributes to resource or performance problems.

---

## Q49.

A server has a very large number of CLOSE-WAIT connections. What could you investigate?

**Answer:**

Investigate whether the application is properly closing sockets.

Possible causes include:

* Application bugs
* Resource leaks
* Poor connection handling
* Unexpected application behavior

---

# Final Interview Challenge

## Q50.

Explain this TCP conversation:

```text
Client                         Server

SYN
----------------------------->

              SYN-ACK
<-----------------------------

ACK
----------------------------->

PSH-ACK
----------------------------->

              ACK
<-----------------------------

FIN
----------------------------->

              ACK
<-----------------------------

              FIN
<-----------------------------

ACK
----------------------------->
```

**Expected Answer:**

The conversation shows:

```text
TCP connection establishment
        ↓
SYN
SYN-ACK
ACK

Data exchange
        ↓
PSH-ACK / ACK

Graceful termination
        ↓
FIN
ACK
FIN
ACK
```

The overall story is:

> A TCP connection was established, application data was exchanged, and the connection was then closed gracefully.

---

# Key Interview Mental Model

```text
SYN
↓
Connection Establishment

ACK
↓
Acknowledgement

ESTABLISHED
↓
Data Transfer

FIN
↓
Graceful Termination

TIME-WAIT
↓
Safe completion

CLOSED
↓
Connection finished

RST
↓
Reset / abort
```

The most important interview skill is not memorizing state names.

It is being able to explain:

> **What is happening in the connection and why?**
