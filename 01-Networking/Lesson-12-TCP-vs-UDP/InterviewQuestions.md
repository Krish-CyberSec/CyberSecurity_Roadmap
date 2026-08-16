# Lesson 12 - TCP vs UDP

# Interview Questions

## Beginner Level

### Q1. What is the Transport Layer?

**Answer:**

The Transport Layer is OSI Layer 4. It provides transport services between application endpoints and includes protocols such as TCP and UDP.

---

### Q2. Why isn't an IP address alone enough to deliver data to an application?

**Answer:**

An IP address identifies the destination host, but a host can run many applications and services simultaneously. Port numbers help identify the appropriate transport-layer service endpoint.

---

### Q3. Which OSI layer does TCP operate at?

**Answer:**

Layer 4 — Transport Layer.

---

### Q4. Which OSI layer does UDP operate at?

**Answer:**

Layer 4 — Transport Layer.

---

### Q5. What is TCP?

**Answer:**

TCP (Transmission Control Protocol) is a connection-oriented transport protocol that provides mechanisms for reliable, ordered communication.

---

### Q6. What is UDP?

**Answer:**

UDP (User Datagram Protocol) is a connectionless, datagram-oriented transport protocol that provides a simpler transport service with lower overhead than TCP.

---

# TCP vs UDP

## Q7. What is the main difference between TCP and UDP?

**Answer:**

TCP establishes a connection and provides mechanisms such as acknowledgements, retransmission, and ordered delivery.

UDP does not use TCP-style connection establishment or reliability mechanisms and instead provides a simpler datagram service.

---

### Q8. Why would a file transfer commonly use TCP?

**Answer:**

File transfers generally require complete and ordered data. TCP provides mechanisms for detecting lost data, retransmitting it, and presenting the data to the application in order.

---

### Q9. Why can real-time voice or video use UDP?

**Answer:**

Real-time applications may prioritize low latency and continuous delivery. Waiting for retransmission of every lost packet can introduce delays that are undesirable in real-time communication.

---

### Q10. Is TCP always slower than UDP?

**Answer:**

No.

That is an oversimplification. TCP generally has more transport mechanisms and overhead, while UDP provides a simpler transport service. Actual performance depends on the application, network, implementation, and protocol design.

---

### Q11. Is UDP unreliable?

**Answer:**

UDP does not provide TCP's built-in reliability mechanisms such as acknowledgements and retransmission. However, applications or higher-level protocols using UDP can implement their own reliability mechanisms.

---

# TCP Reliability

## Q12. Why does TCP use sequence information?

**Answer:**

Sequence information allows TCP to identify positions within the byte stream, support ordering, and help detect missing data.

---

### Q13. What is the purpose of TCP acknowledgements?

**Answer:**

Acknowledgements provide feedback to the sender about received data and contribute to TCP's reliability mechanisms.

---

### Q14. What is TCP retransmission?

**Answer:**

Retransmission is the process of sending data again when TCP determines that data needs to be recovered.

---

### Q15. Does TCP transmit files?

**Answer:**

TCP does not understand files such as PDFs or images. TCP transports a byte stream. The application determines what those bytes represent.

---

# TCP Three-Way Handshake

## Q16. What is the TCP three-way handshake?

**Answer:**

The TCP three-way handshake establishes a TCP connection using:

```text
SYN
SYN-ACK
ACK
```

---

### Q17. What is a SYN?

**Answer:**

SYN is a TCP flag associated with initiating a TCP connection and synchronizing sequence-number state.

---

### Q18. What is SYN-ACK?

**Answer:**

SYN-ACK combines acknowledgement of the received SYN with the server's participation in connection establishment.

---

### Q19. What is ACK?

**Answer:**

ACK is a TCP flag used for acknowledgement-related signaling.

---

### Q20. Why are three messages used in the TCP handshake?

**Answer:**

The handshake establishes communication state between the endpoints and synchronizes relevant TCP state, including sequence-number information.

---

# TCP Flags

## Q21. Name some important TCP flags.

**Answer:**

Important TCP flags include:

* SYN
* ACK
* FIN
* RST
* PSH
* URG

---

### Q22. What is the purpose of the FIN flag?

**Answer:**

FIN is associated with graceful TCP connection termination.

---

### Q23. What is the purpose of the RST flag?

**Answer:**

RST is used to reset a TCP connection.

---

# Ports

## Q24. What is a network port?

**Answer:**

A port is a 16-bit transport-layer identifier used to identify a service or application endpoint on a host.

---

### Q25. What is the valid TCP/UDP port range?

**Answer:**

```text
0–65535
```

---

### Q26. What is the difference between a source port and a destination port?

**Answer:**

The source port identifies the sending application's transport endpoint, while the destination port identifies the target service endpoint on the receiving host.

---

### Q27. What is an ephemeral port?

**Answer:**

An ephemeral port is a temporary port commonly selected by the client operating system for outgoing connections.

---

### Q28. What is port 443 commonly associated with?

**Answer:**

TCP port 443 is commonly associated with HTTPS.

However, a port number alone does not prove which application is running.

---

### Q29. What is port 22 commonly associated with?

**Answer:**

TCP port 22 is commonly associated with SSH.

---

### Q30. What is port 53 commonly associated with?

**Answer:**

Port 53 is commonly associated with DNS and may be used over both UDP and TCP depending on the DNS operation.

---

# Sockets

## Q31. What is a socket?

**Answer:**

A socket is a network communication endpoint. A simplified representation is:

```text
IP Address + Port + Transport Protocol
```

---

### Q32. What is a TCP four-tuple?

**Answer:**

A TCP flow can be identified using:

```text
Source IP
Source Port
Destination IP
Destination Port
```

---

### Q33. Why are source ports important?

**Answer:**

Source ports help the operating system distinguish different communication flows originating from the same host.

---

# UDP in Practice

## Q34. What is a UDP datagram?

**Answer:**

A UDP datagram is an individual message transported by UDP. Unlike TCP's byte stream, UDP presents data as separate datagrams.

---

### Q35. Why can DNS use UDP?

**Answer:**

DNS queries are often small and can benefit from UDP's lower transport overhead. However, DNS can also use TCP and other transport mechanisms.

---

### Q36. Why can QUIC use UDP?

**Answer:**

QUIC uses UDP as its underlying transport and implements additional functionality such as reliable delivery, congestion control, encryption, connection management, and stream multiplexing above UDP.

---

### Q37. Is UDP 443 necessarily suspicious?

**Answer:**

No. UDP port 443 can legitimately be used by QUIC and HTTP/3.

---

# Security Questions

## Q38. Why is TCP knowledge important for a SOC analyst?

**Answer:**

SOC analysts investigate network flows, firewall logs, IDS alerts, and packet captures. Understanding TCP allows them to interpret connection establishment, flags, ports, retransmissions, and unusual traffic patterns.

---

### Q39. A workstation sends thousands of TCP SYN packets to many hosts and ports. What might you investigate?

**Answer:**

Possible explanations include:

* Port scanning
* Security scanning
* Misconfiguration
* Malware
* Other unusual activity

The analyst should investigate the source process, targets, ports, timing, network context, and other evidence before determining whether the activity is malicious.

---

### Q40. Why is a port number not proof of a service?

**Answer:**

Services can be configured to listen on non-standard ports, and a common port number can be used by another application. Therefore, port numbers are indicators rather than definitive proof.

---

# GRC Questions

## Q41. Why should a GRC analyst understand TCP and UDP?

**Answer:**

GRC analysts may review firewall rules, network architecture, security controls, and approved services. Understanding TCP, UDP, ports, and IP addressing helps them evaluate whether technical controls align with security requirements.

---

### Q42. Interpret this firewall rule:

```text
Source: 10.10.0.0/24
Destination: 10.20.0.0/24
Protocol: TCP
Destination Port: 443
Action: ALLOW
```

**Answer:**

The rule permits TCP traffic from the `10.10.0.0/24` network to the `10.20.0.0/24` network when the destination port is `443`.

---

# Scenario Questions

## Q43.

A client sends:

```text
192.168.1.20:51544
        ↓
10.20.10.50:443
```

Identify:

```text
Source IP:
Source Port:
Destination IP:
Destination Port:
```

**Answer:**

```text
Source IP:        192.168.1.20
Source Port:      51544
Destination IP:   10.20.10.50
Destination Port: 443
```

---

### Q44.

A firewall detects:

```text
UDP 443
```

Should the security team immediately block it?

**Answer:**

No.

UDP 443 can legitimately be used by QUIC and HTTP/3. The team should investigate the source, destination, application behavior, and context before deciding whether the traffic is malicious.

---

### Q45.

A file transfer experiences packet loss. Why is TCP generally appropriate?

**Answer:**

TCP provides mechanisms such as sequence information, acknowledgements, and retransmission that allow lost data to be recovered and delivered in the proper order.

---

# Final Interview Challenge

## Q46.

Explain the complete journey of:

```text
192.168.1.20:51544
        ↓
10.20.10.50:443
```

from the application to the network.

**Expected reasoning:**

```text
Application
    ↓
Application data
    ↓
TCP
    ↓
Source port 51544
Destination port 443
    ↓
IP
    ↓
Source IP 192.168.1.20
Destination IP 10.20.10.50
    ↓
Data Link
    ↓
Local MAC addressing
    ↓
Ethernet / Wi-Fi
    ↓
Physical transmission
```

The exact encapsulation and headers depend on the networking environment, but this provides the conceptual layer-by-layer model.
