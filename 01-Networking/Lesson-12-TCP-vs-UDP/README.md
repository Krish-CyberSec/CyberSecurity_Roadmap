# Lesson 12 - TCP vs UDP

## Module Information

| Field         | Details                      |
| ------------- | ---------------------------- |
| Module        | Networking for Cybersecurity |
| Lesson        | 12                           |
| Topic         | TCP, UDP, Ports & Sockets    |
| Difficulty    | Beginner → Intermediate      |
| OSI Layer     | Layer 4 – Transport          |
| Prerequisites | Lessons 01–11                |
| Status        | Completed                    |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Explain the purpose of the Transport Layer.
* Understand why IP addresses alone are not enough for application communication.
* Explain TCP and UDP.
* Understand why both TCP and UDP exist.
* Explain TCP's reliability mechanisms at a conceptual level.
* Understand TCP sequence numbers and acknowledgements.
* Explain the TCP three-way handshake.
* Understand important TCP flags at a basic level.
* Understand TCP's connection lifecycle at a high level.
* Explain ports and their purpose.
* Distinguish source and destination ports.
* Understand ephemeral ports.
* Explain what a socket represents.
* Understand the concept of a TCP four-tuple.
* Understand UDP datagrams.
* Identify real-world UDP use cases.
* Understand why DNS may use UDP and TCP.
* Understand the relationship between UDP, QUIC, and HTTP/3.
* Apply transport-layer knowledge to cybersecurity and GRC scenarios.
* Map TCP, UDP, ports, and sockets to the OSI model.

---

# Part 1 — TCP vs UDP Fundamentals

# 1. The Engineering Problem

In previous lessons, we learned how a packet can travel between hosts.

For example:

```text
Laptop
   ↓
Switch
   ↓
Router
   ↓
Internet
   ↓
Server
```

The Network Layer helps the packet reach the correct host.

But a server can run many applications at the same time:

```text
Web Server
SSH
DNS
Database
Mail Server
```

The operating system therefore needs transport-layer information to deliver incoming data to the correct application endpoint.

This is one of the major problems solved by the **Transport Layer**.

---

# 2. Layer 4 — Transport Layer

The Transport Layer is:

```text
OSI Layer 4
```

Our model now looks like:

```text
Layer 7 — Application
Layer 6 — Presentation
Layer 5 — Session
Layer 4 — Transport      ← CURRENT LESSON
Layer 3 — Network
Layer 2 — Data Link
Layer 1 — Physical
```

The two major transport protocols we are studying are:

```text
TCP
UDP
```

---

# 3. IP Address vs Port

A useful mental model is:

```text
IP Address → Which host?
Port       → Which service/application endpoint?
```

For example:

```text
10.10.10.50:443
```

means:

```text
Host:
10.10.10.50

Port:
443
```

Port `443` is commonly associated with HTTPS.

Another example:

```text
10.10.10.50:22
```

Port `22` is commonly associated with SSH.

The same host can therefore provide multiple services.

---

# 4. Why TCP and UDP Exist

Different applications have different communication requirements.

For example:

A file transfer generally benefits from:

* Reliable delivery
* Ordered delivery
* Recovery from lost data

A real-time application may prioritize:

* Low latency
* Low overhead
* Continuous delivery

TCP and UDP provide different transport behaviors for these requirements.

---

# 5. TCP

**TCP — Transmission Control Protocol**

TCP is a connection-oriented transport protocol that provides mechanisms for reliable, ordered communication.

TCP includes mechanisms for:

* Connection establishment
* Sequence numbers
* Acknowledgements
* Retransmission
* Ordered delivery
* Flow control
* Congestion control

TCP provides an **ordered byte stream** to the application.

---

# 6. UDP

**UDP — User Datagram Protocol**

UDP is a connectionless, datagram-oriented transport protocol.

It provides a simpler transport service than TCP.

UDP does not provide TCP's built-in mechanisms for:

* Connection establishment
* Acknowledgements
* Retransmission
* Ordered delivery
* TCP-style flow control

Applications can implement additional behavior themselves when necessary.

---

# 7. TCP vs UDP

| Feature                    | TCP                     | UDP                             |
| -------------------------- | ----------------------- | ------------------------------- |
| Connection establishment   | Yes                     | No TCP-style handshake          |
| Data model                 | Ordered byte stream     | Datagram                        |
| TCP-style acknowledgements | Yes                     | No                              |
| TCP-style retransmission   | Yes                     | No                              |
| TCP-style ordering         | Yes                     | No                              |
| TCP-style flow control     | Yes                     | No                              |
| TCP congestion control     | Yes                     | No TCP-style mechanism          |
| Transport overhead         | Higher                  | Lower                           |
| Common examples            | Web, SSH, file transfer | DNS, real-time traffic, QUIC    |
| Reliability                | Built-in mechanisms     | Application may provide its own |

A common misconception is:

```text
TCP = slow
UDP = fast
```

This is too simplistic.

A better model is:

> TCP provides more transport mechanisms and reliability features, while UDP provides a simpler datagram transport with lower transport overhead.

Actual performance depends on the application, network conditions, implementation, and protocol design.

---

# Part 2 — TCP Reliability and Three-Way Handshake

# 8. Why Does TCP Need Reliability?

Suppose an application sends data that is conceptually divided into several pieces:

```text
Data 1
Data 2
Data 3
Data 4
Data 5
```

The network might experience loss:

```text
Data 1   ✓
Data 2   ✓
Data 3   ✗
Data 4   ✓
Data 5   ✓
```

The receiver needs a way to detect missing data and reconstruct the stream correctly.

TCP provides mechanisms to help solve this problem.

---

# 9. Sequence Numbers

TCP uses sequence information to identify positions in the byte stream.

Conceptually:

```text
Segment A → Sequence 1000
Segment B → Sequence 1500
Segment C → Sequence 2000
```

This helps the receiver determine ordering and identify gaps.

The application doesn't see TCP segments as files or images.

TCP transports bytes.

The application determines what those bytes represent.

---

# 10. Acknowledgements

TCP uses acknowledgement mechanisms to provide feedback to the sender about received data.

Conceptually:

```text
Sender
   |
   | Data
   v
Receiver
   |
   | ACK
   v
Sender
```

This feedback is one component of TCP's reliability system.

---

# 11. Retransmission

If data is lost, TCP can retransmit missing data.

Conceptually:

```text
Sender
  |
  | Segment 1
  | Segment 2
  | Segment 3   X Lost
  | Segment 4
  v
Receiver
  |
  | Missing data detected
  v
Sender retransmits
```

This is important for applications where complete and ordered data delivery matters.

---

# 12. TCP Provides an Ordered Byte Stream

Suppose data is sent as:

```text
A
B
C
D
```

Network behavior may cause data to arrive out of sequence.

TCP uses sequence information and its reliability mechanisms to present the application with the data in the correct order.

The application sees:

```text
A B C D
```

rather than having to handle arbitrary network reordering itself.

---

# 13. TCP Three-Way Handshake

Before normal TCP data transfer, the endpoints establish a TCP connection.

The basic handshake is:

```text
SYN
SYN-ACK
ACK
```

---

## Step 1 — SYN

The client sends:

```text
SYN
```

Conceptually:

```text
Client
  |
  | SYN
  v
Server
```

This indicates a request to establish a TCP connection.

---

## Step 2 — SYN-ACK

The server responds:

```text
SYN + ACK
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

The server acknowledges the request and participates in connection establishment.

---

## Step 3 — ACK

The client sends:

```text
ACK
```

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
  |
  | ACK
  v
Server
```

The connection can now proceed to normal data transfer.

---

# 14. Why Three Messages?

The handshake establishes communication state between the two endpoints and synchronizes relevant TCP state, including sequence-number information.

A useful simplified interpretation is:

```text
Client → "Can we communicate?"

Server → "I received you and I'm ready."

Client → "Confirmed."
```

---

# 15. TCP Flags

Important TCP flags include:

* SYN
* ACK
* FIN
* RST
* PSH
* URG

At this stage:

### SYN

Associated with connection establishment.

### ACK

Acknowledgement-related signaling.

### FIN

Used when gracefully closing a TCP connection.

### RST

Used to reset a connection.

The complete TCP state machine will be studied later.

---

# 16. TCP Connection Lifecycle

A simplified view:

```text
Connection Start
      |
      ▼
    SYN
      |
      ▼
  SYN-ACK
      |
      ▼
     ACK
      |
      ▼
 ESTABLISHED
      |
      |
 Data Transfer
      |
      ▼
 Connection Closing
      |
      ▼
     FIN
```

Real TCP state handling is more detailed than this simplified diagram.

---

# Part 3 — Ports and Sockets

# 17. Why Ports Exist

A server can run many services at the same time.

For example:

```text
Server
├── Web
├── SSH
├── DNS
└── Database
```

The IP address identifies the host.

The port identifies the transport-layer service endpoint.

Therefore:

```text
IP Address → Host
Port       → Service/Application endpoint
```

---

# 18. Port Number Range

TCP and UDP use 16-bit port numbers.

Therefore:

```text
0 – 65535
```

Broad categories are:

```text
0–1023
Well-known ports

1024–49151
Registered ports

49152–65535
Dynamic / private / ephemeral ports
```

These categories describe common conventions.

---

# 19. Common Ports

|  Port | Common Association |
| ----: | ------------------ |
| 20/21 | FTP                |
|    22 | SSH                |
|    23 | Telnet             |
|    25 | SMTP               |
|    53 | DNS                |
|    80 | HTTP               |
|   110 | POP3               |
|   143 | IMAP               |
|   443 | HTTPS              |
|   445 | SMB                |
|  3389 | RDP                |

These are common associations, not guarantees.

A service can be configured to listen on a non-standard port.

Therefore:

> **A port number is evidence of a possible service, not proof of that service.**

---

# 20. Source Port and Destination Port

Consider:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

This contains:

```text
Source IP:
192.168.1.20

Source Port:
51544

Destination IP:
10.10.10.50

Destination Port:
443
```

The destination port commonly identifies the target service endpoint.

The source port is typically an ephemeral client-side port.

---

# 21. Why Source Ports Matter

Suppose a client establishes several connections:

```text
192.168.1.20:51001 → Server A:443

192.168.1.20:51002 → Server B:443

192.168.1.20:51003 → Server C:443
```

Different source ports help the operating system distinguish different communication flows.

---

# 22. What Is a Socket?

A socket represents a network communication endpoint.

A simplified mental model is:

```text
IP Address
+
Port
+
Transport Protocol
```

Example:

```text
192.168.1.20:51544/TCP
```

or:

```text
10.10.10.50:443/TCP
```

---

# 23. TCP Four-Tuple

A TCP flow can be distinguished using:

```text
Source IP
Source Port
Destination IP
Destination Port
```

Example:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

This information appears in:

* Firewall logs
* Flow records
* SIEM data
* Wireshark
* SOC investigations

---

# 24. TCP Ports and UDP Ports Are Separate

TCP and UDP have separate transport namespaces.

Therefore:

```text
TCP 53
```

and:

```text
UDP 53
```

are different transport endpoints.

The same numerical port can exist under both TCP and UDP.

---

# Part 4 — UDP in Practice

# 25. Why Use UDP?

UDP can be useful when an application values:

* Lower transport overhead
* Low latency
* Freshness of information
* Application-specific reliability
* Datagram-based communication

Examples include:

* DNS
* Voice communication
* Real-time media
* Gaming
* Some streaming systems
* QUIC

---

# 26. UDP and Packet Loss

Consider a live voice call.

If a small audio packet is delayed or lost:

### Option A

Wait for recovery:

```text
Packet lost
    ↓
Recovery
    ↓
Delay
```

### Option B

Continue with a small gap:

```text
Packet lost
    ↓
Keep going
```

For some real-time applications, minimizing delay can be more important than retransmitting every lost packet.

This is why UDP can be useful.

---

# 27. UDP Does Not Mean "No Reliability"

UDP itself does not provide TCP's built-in reliability mechanisms.

However, an application or higher-level protocol can implement its own:

* Sequence numbers
* Acknowledgements
* Retransmission
* Error handling

Conceptually:

```text
Application
  ├── Reliability logic
  ├── Sequence numbers
  ├── Retransmission
  └── Error handling
         |
         v
        UDP
```

Therefore:

> **UDP does not provide TCP-style reliability; applications using UDP can implement their own reliability when required.**

---

# 28. DNS

DNS commonly uses:

```text
UDP
Port 53
```

For example:

```text
Client
192.168.1.20:53000
      |
      | DNS Query
      v
DNS Server
8.8.8.8:53
```

However, DNS is not limited to UDP.

DNS can also use:

* TCP
* DNS over TLS
* DNS over HTTPS
* Other transport mechanisms

Therefore:

> Do not treat "DNS = UDP 53" as an absolute rule.

---

# 29. Real-Time Applications

UDP can be useful for:

```text
VoIP
Video conferencing
Live media
Online gaming
```

because these applications may prioritize:

```text
Low latency
+
Continuous communication
```

over waiting for every missing packet.

The specific protocol behavior depends on the application.

---

# 30. QUIC

A modern example is **QUIC**.

Conceptually:

```text
HTTP/3
   |
  QUIC
   |
  UDP
   |
  IP
```

QUIC uses UDP underneath but provides sophisticated capabilities such as:

* Reliable delivery
* Stream multiplexing
* Connection management
* Encryption
* Congestion control

This demonstrates why:

> "UDP is unreliable"

is an incomplete description.

UDP provides the underlying datagram service; higher-level protocols can add additional functionality.

---

# 31. UDP 443

Seeing:

```text
UDP 443
```

does not automatically mean malicious traffic.

It may represent legitimate QUIC / HTTP/3 traffic.

Likewise:

```text
TCP 443
```

does not guarantee that the traffic is HTTPS.

Therefore, security analysts should investigate:

```text
Port
+
Protocol
+
Application behavior
+
Source
+
Destination
+
Context
```

rather than relying on a port number alone.

---

# Part 5 — Complete Data Journey and Security Context

# 32. Complete Communication Stack

Our networking model is now:

```text
Application
    |
    | Data
    v
TCP / UDP
    |
    | Transport
    v
IP
    |
    | Network
    v
MAC
    |
    | Local delivery
    v
Ethernet / Wi-Fi
    |
    v
Physical network
```

Each layer answers a different question:

```text
Application → What data?

TCP / UDP   → How should it be transported?

Port        → Which service endpoint?

IP          → Which host/network?

MAC         → Which local interface?

Physical    → How do the bits travel?
```

---

# 33. Example — HTTPS

Suppose a browser requests:

```text
https://example.com
```

A simplified conceptual stack is:

```text
Application
HTTP request

        ↓

Transport
TCP
Destination Port 443

        ↓

Network
Destination IP

        ↓

Data Link
Destination MAC

        ↓

Physical
Bits/signals
```

This ties together the topics we've learned across the networking module.

---

# 34. Security Perspective

Transport-layer information is extremely valuable in cybersecurity.

A flow record may contain:

```text
Source IP
Destination IP
Source Port
Destination Port
Protocol
```

A SOC analyst can use this information to investigate:

* Port scanning
* Unexpected services
* Suspicious TCP connections
* Unusual UDP activity
* Firewall violations
* Possible command-and-control traffic

---

# 35. Port Scanning

An attacker may probe:

```text
22
23
80
443
445
3389
```

across a target host or network.

The purpose may be to discover reachable services.

This is the basic concept behind port scanning and service enumeration.

We will study this more deeply later during the security tooling portion of the roadmap.

---

# 36. Firewall Example

Consider:

```text
ALLOW TCP 443
DENY TCP 23
```

This can be interpreted as:

```text
TCP traffic → destination port 443 → ALLOW

TCP traffic → destination port 23 → DENY
```

Firewall rules can also consider:

* Source IP/network
* Destination IP/network
* Protocol
* Source port
* Destination port
* Direction

---

# 37. SOC Scenario

Suppose a workstation suddenly generates:

```text
Thousands of TCP SYN packets
```

toward:

```text
Many hosts
Many destination ports
```

A SOC analyst may investigate whether this represents:

* Port scanning
* Misconfiguration
* Security tooling
* Malware
* Other unusual activity

The analyst should use context rather than automatically labeling it malicious.

---

# 38. GRC Scenario

A security policy states:

> "Only approved services may communicate with the production environment."

A technical control might be:

```text
Source Network:
10.10.0.0/24

Destination Network:
10.20.0.0/24

Protocol:
TCP

Destination Port:
443

Action:
ALLOW
```

A GRC analyst can now understand what the technical rule is actually permitting and compare it against the organization's policy.

---

# 39. Why Transport Knowledge Matters Across Cybersecurity

### SOC

You need to understand:

```text
IPs
Ports
Protocols
TCP flags
Flows
```

### Security Engineering

You need to understand:

```text
Firewall rules
Allowed protocols
Service exposure
Traffic paths
```

### Red Team

You need to understand:

```text
Open ports
Services
Scanning
Network communication
```

### GRC

You need to understand:

```text
Approved services
Network controls
Firewall policies
Security architecture
Change impact
```

So Layer 4 is not just an academic networking topic.

---

# 40. OSI Model Mapping

| OSI Layer               | Topics                                      |
| ----------------------- | ------------------------------------------- |
| Layer 7 – Application   | Browser, HTTP, DNS, SSH concepts            |
| Layer 6 – Presentation  | Not studied yet                             |
| Layer 5 – Session       | Not studied yet                             |
| **Layer 4 – Transport** | **TCP, UDP, Ports, Sockets, TCP Handshake** |
| Layer 3 – Network       | IP, Router, Routing, Subnetting, CIDR, VLSM |
| Layer 2 – Data Link     | MAC, ARP, Switch                            |
| Layer 1 – Physical      | Ethernet, Wi-Fi                             |

---

# 41. TCP vs UDP — Final Mental Model

```text
                     TRANSPORT LAYER
                           |
              ┌────────────┴────────────┐
              |                         |
             TCP                       UDP
              |                         |
      Connection-oriented         Connectionless
              |                         |
      Ordered byte stream            Datagrams
              |                         |
      Acknowledgements           No TCP-style ACK
              |                         |
      Retransmission             No TCP-style
                                  retransmission
              |                         |
      Flow control               Lower overhead
              |                         |
   Congestion-control             Application may
      mechanisms                  add its own logic
```

---

# Key Takeaways

* The Transport Layer is OSI Layer 4.
* TCP and UDP provide different transport behaviors.
* TCP establishes connections and provides mechanisms for reliable, ordered communication.
* UDP provides a simpler datagram-based transport.
* TCP uses sequence information and acknowledgements.
* TCP uses retransmission mechanisms when data needs to be recovered.
* TCP begins communication using the SYN, SYN-ACK, ACK handshake.
* Ports help identify application/service endpoints.
* Source ports are typically ephemeral client-side ports.
* Destination ports commonly identify target service endpoints.
* A socket represents a network communication endpoint.
* A TCP flow can be identified using source IP, source port, destination IP, and destination port.
* TCP and UDP have separate port namespaces.
* UDP can be useful for low-overhead and latency-sensitive applications.
* DNS commonly uses UDP 53 but can also use TCP and other transports.
* QUIC uses UDP as its underlying transport while providing advanced transport functionality.
* Port numbers alone do not prove what service is running.
* Transport-layer information is important for SOC, Security Engineering, Red Teaming, and GRC.

---

# Before You Move On

You should be able to explain without looking at your notes:

* [ ] Why the Transport Layer exists.
* [ ] TCP vs UDP.
* [ ] Why TCP uses a three-way handshake.
* [ ] What SYN, SYN-ACK, and ACK represent at a basic level.
* [ ] Why TCP uses sequence information.
* [ ] What acknowledgements accomplish.
* [ ] Why retransmission is useful.
* [ ] What a port identifies.
* [ ] Source vs destination port.
* [ ] What an ephemeral port is.
* [ ] What a socket represents.
* [ ] What a four-tuple represents.
* [ ] Why UDP can be useful for real-time applications.
* [ ] Why UDP 443 can be legitimate.
* [ ] How transport information helps a SOC analyst.
* [ ] How transport concepts appear in firewall policies.
* [ ] Why GRC professionals need to understand TCP, UDP, and ports.

---

# Summary

Lesson 12 introduced the Transport Layer and established the foundation for understanding how applications communicate over networks.

TCP provides connection establishment and mechanisms for reliable, ordered transport. UDP provides a simpler datagram service with less transport overhead and allows applications or higher-level protocols to implement additional functionality when necessary.

Ports identify service endpoints, while sockets represent communication endpoints. Together with IP addresses and transport protocols, they provide the information necessary for operating systems, firewalls, monitoring platforms, and security analysts to understand network flows.

These concepts form the foundation for the next stage of networking and cybersecurity topics, including deeper TCP behavior, service enumeration, firewall analysis, Wireshark, Nmap, and network-based security investigations.
