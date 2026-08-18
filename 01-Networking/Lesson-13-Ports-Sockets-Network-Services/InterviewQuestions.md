# Lesson 13 - Ports, Sockets & Network Services

# Interview Questions

---

# Beginner Level

## Q1. What is a network port?

**Answer:**

A port is a transport-layer endpoint used to identify a service or application endpoint on a host.

A useful mental model is:

```text
IP Address → Which host?
Port       → Which service endpoint?
```

---

## Q2. Why are ports needed if we already have IP addresses?

**Answer:**

An IP address identifies the destination host, but a host can run multiple services simultaneously. Ports help the operating system identify which application or service should receive the traffic.

---

## Q3. What does it mean when a service is "listening" on a port?

**Answer:**

It means a process on the host has created a network endpoint and is waiting for incoming traffic on that port and transport protocol.

For example:

```text
10.10.10.50:22/TCP
```

could have an SSH service listening on it.

---

## Q4. Does a listening port mean the machine is compromised?

**Answer:**

No.

A listening port only indicates that a service is waiting for network communication. It does not by itself indicate that the system is compromised or vulnerable.

---

## Q5. What is the difference between a port and a service?

**Answer:**

A port is the transport-layer endpoint number, while a service is the application or network service operating behind that endpoint.

For example:

```text
22/TCP
  ↓
Commonly associated with SSH
```

---

# Port States

## Q6. What does an "open" port mean?

**Answer:**

From a scanner's perspective, an open port means that the scanner can determine that a service is reachable and accepting communication on that port.

---

## Q7. What does a "closed" port mean?

**Answer:**

A closed port generally means the host is reachable, but no service is currently listening on that port.

---

## Q8. What does a "filtered" port mean?

**Answer:**

A filtered port means that some filtering mechanism, such as a firewall or packet filter, prevents the scanner from determining whether the port is open or closed.

---

## Q9. What is the difference between LISTENING and OPEN?

**Answer:**

`LISTENING` is primarily a local host observation showing that a process is waiting for incoming connections.

`OPEN` is a scanner's observation that the port is reachable/open from the scanner's location.

Example:

```text
Local host:
TCP 8080 → LISTENING

Remote scanner:
TCP 8080 → FILTERED
```

Both can be true at the same time.

---

## Q10. Can a service be listening on a port but appear filtered to Nmap?

**Answer:**

Yes.

A firewall or other network control can block remote access even though the service is listening locally.

---

# Sockets

## Q11. What is a socket?

**Answer:**

A socket represents a network communication endpoint.

A simplified model is:

```text
IP Address
+
Port
+
Transport Protocol
```

For example:

```text
192.168.1.20:51544/TCP
```

---

## Q12. What is a TCP four-tuple?

**Answer:**

A TCP flow can be distinguished using:

```text
Source IP
Source Port
Destination IP
Destination Port
```

For example:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

---

## Q13. What is the difference between a source port and a destination port?

**Answer:**

The source port identifies the sending application's transport endpoint, while the destination port identifies the target service endpoint on the receiving host.

---

## Q14. What is an ephemeral port?

**Answer:**

An ephemeral port is a temporary port commonly selected by the client operating system for outgoing network connections.

For example:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

`51544` is likely an ephemeral client-side port.

---

## Q15. Why do clients need source ports?

**Answer:**

Source ports help the operating system distinguish multiple simultaneous communication flows originating from the same host.

---

# Binding and Exposure

## Q16. What does `127.0.0.1:8080` mean?

**Answer:**

It means a service is bound to the local loopback interface on port 8080.

It is generally intended to be accessible only from the local machine.

---

## Q17. What does `0.0.0.0:8080` generally mean?

**Answer:**

It generally means the service is listening on all available IPv4 interfaces on port 8080, subject to firewall and network controls.

---

## Q18. Why is `0.0.0.0:8080` potentially more exposed than `127.0.0.1:8080`?

**Answer:**

`127.0.0.1` refers to the local loopback interface, whereas `0.0.0.0` generally causes the service to listen across available IPv4 interfaces.

Therefore, other systems may potentially reach a service bound to `0.0.0.0`, depending on routing and firewall controls.

---

## Q19. Does listening on `0.0.0.0` automatically mean the service is accessible from the Internet?

**Answer:**

No.

External reachability also depends on:

* Routing
* Firewall rules
* ACLs
* Security groups
* Network topology
* Other filtering controls

---

# TCP Scanning

## Q20. What is a TCP Connect Scan?

**Answer:**

A TCP Connect Scan attempts to complete the normal TCP three-way handshake with the target.

Nmap syntax:

```bash
nmap -sT TARGET
```

Conceptually:

```text
SYN
SYN-ACK
ACK
```

---

## Q21. What is a TCP SYN Scan?

**Answer:**

A TCP SYN Scan sends a SYN and examines the response without completing the normal TCP connection.

A simplified interaction is:

```text
SYN
SYN-ACK
RST
```

Nmap syntax:

```bash
sudo nmap -sS TARGET
```

---

## Q22. What is the difference between a TCP Connect Scan and a SYN Scan?

**Answer:**

A TCP Connect Scan completes the TCP handshake.

A SYN Scan does not complete the full connection and instead uses the response to the SYN to infer the port state.

---

## Q23. Why is understanding TCP flags useful for port scanning?

**Answer:**

TCP flags such as SYN, SYN-ACK, ACK, and RST are involved in connection establishment and termination. Their behavior provides information that scanners can use to determine port states.

---

# UDP Scanning

## Q24. Why is UDP scanning different from TCP scanning?

**Answer:**

UDP does not use a TCP-style connection handshake.

A UDP scanner sends datagrams and must interpret responses, ICMP messages, or lack of response to determine the likely state of the port.

---

## Q25. Why can UDP scanning be harder than TCP scanning?

**Answer:**

An open UDP service may not respond to an unexpected probe.

Therefore, no response may indicate either:

```text
Open
```

or:

```text
Filtered
```

which makes the result more ambiguous.

---

## Q26. What might indicate a closed UDP port?

**Answer:**

An ICMP Destination Unreachable / Port Unreachable response can provide evidence that a UDP port is closed.

---

# Service Discovery and Enumeration

## Q27. What is service discovery?

**Answer:**

Service discovery is the process of determining what service or application is operating behind an open network endpoint.

---

## Q28. What is service enumeration?

**Answer:**

Service enumeration goes beyond simply identifying that a port is open. It attempts to gather more information about the service, such as its type, implementation, version, and behavior.

---

## Q29. What Nmap option can be used for service/version detection?

**Answer:**

```bash
nmap -sV TARGET
```

This asks Nmap to use probes to identify likely services and versions.

---

## Q30. Why doesn't Nmap simply assume port 80 means HTTP?

**Answer:**

Because port numbers are conventions rather than guarantees.

A service can be configured to use a non-standard port, so Nmap can use probes and response behavior to obtain additional evidence about the actual service.

---

## Q31. What is a service banner?

**Answer:**

A service banner is information returned by a network service that can reveal details such as the protocol, software, or version.

Example:

```text
SSH-2.0-OpenSSH_9.x
```

---

## Q32. Are service banners always reliable?

**Answer:**

No.

Banners may be modified, hidden, misleading, or generated by a proxy. Therefore, enumeration results should be treated as evidence rather than absolute truth.

---

## Q33. What is the difference between port scanning and service enumeration?

**Answer:**

Port scanning asks:

> Which ports are reachable?

Service enumeration asks:

> What service or software is running behind those ports?

---

## Q34. What is the difference between service enumeration and vulnerability assessment?

**Answer:**

Service enumeration identifies information about the service.

Vulnerability assessment determines whether the identified service contains a known or otherwise validated security weakness.

The progression is:

```text
Port Discovery
      ↓
Service Enumeration
      ↓
Version Identification
      ↓
Vulnerability Assessment
```

---

# Local Host Investigation

## Q35. How can you inspect listening network sockets on Linux?

**Answer:**

A common command is:

```bash
ss -tulpn
```

It can help identify:

* TCP sockets
* UDP sockets
* Listening endpoints
* Ports
* Associated processes, depending on permissions

---

## Q36. How can you inspect network connections on Windows?

**Answer:**

A common command is:

```cmd
netstat -ano
```

The PID can then be correlated with a process.

---

## Q37. Why is identifying the process behind a listening port important?

**Answer:**

Because the port number alone doesn't tell you whether the service is legitimate or suspicious.

For example:

```text
4444
 ↓
PID
 ↓
Process
 ↓
Executable
 ↓
Parent Process
 ↓
User
```

This gives much more useful evidence for investigation.

---

# Security Questions

## Q38. Does an open port automatically mean there is a vulnerability?

**Answer:**

No.

An open port only indicates that a service is reachable. You must identify the service, version, configuration, exposure, and relevant vulnerabilities before determining security risk.

---

## Q39. Why does service exposure contribute to attack surface?

**Answer:**

Every reachable service provides a potential interaction point for clients or attackers. Unnecessary or poorly secured services can increase the number of paths through which a system might be attacked.

---

## Q40. How can an organization reduce service exposure?

**Answer:**

Organizations can reduce exposure by:

* Removing unnecessary services.
* Restricting network access.
* Using firewalls and ACLs.
* Segmenting networks.
* Patching services.
* Hardening configurations.
* Requiring strong authentication.
* Monitoring service access.

---

## Q41. Why is an exposed database port concerning?

**Answer:**

A database service may contain sensitive information and often does not need to be directly accessible from untrusted networks.

The appropriate question is not simply:

> "Is the port open?"

but:

> "Who can reach it, and is that exposure required?"

---

# SOC Scenario Questions

## Q42.

A workstation starts listening on:

```text
0.0.0.0:4444
```

What should a SOC analyst investigate?

**Answer:**

The analyst should investigate:

```text
Which process owns the port?
Which executable is running?
Who started it?
When did it start?
Why is it listening?
What connections does it make?
Was the system recently modified?
Is the activity authorized?
```

---

## Q43.

A workstation sends connection attempts to:

```text
22
23
80
443
445
3389
```

across hundreds of hosts.

What might this indicate?

**Answer:**

Possible explanations include:

* Port scanning
* Security scanning
* Misconfiguration
* Malware
* Other unusual activity

The analyst should investigate the source process, targets, timing, context, and related evidence before determining whether it is malicious.

---

## Q44.

Nmap reports:

```text
3306/tcp open
```

Does this prove the server is running a vulnerable MySQL installation?

**Answer:**

No.

It indicates that a TCP service is reachable on port 3306.

Further service identification, version detection, configuration analysis, and vulnerability assessment are required.

---

# GRC Questions

## Q45. Why should a GRC analyst understand ports and services?

**Answer:**

GRC analysts may review:

* Firewall rules
* Network architecture
* Security controls
* Approved services
* Infrastructure changes
* Service exposure

Understanding ports and services helps them determine whether technical configurations align with organizational policies and security requirements.

---

## Q46.

A GRC assessment finds an Internet-facing server exposing:

```text
22/tcp
443/tcp
3306/tcp
```

What questions should the analyst ask?

**Answer:**

The analyst should ask:

1. Why is each service exposed?
2. Is each service required?
3. Who approved the exposure?
4. Can access be restricted?
5. Are compensating controls present?
6. Is the exposure documented?
7. Is monitoring enabled?
8. Is the service covered by vulnerability management?

---

# Scenario-Based Interview Questions

## Q47.

A service is:

```text
0.0.0.0:8080 LISTENING
```

but Nmap from another network reports:

```text
8080/tcp filtered
```

Is this contradictory?

**Answer:**

No.

The local host is reporting that a service is listening on port 8080, while the remote scanner is reporting that filtering prevents it from determining the external port state.

Both observations can be correct.

---

## Q48.

A server has:

```text
127.0.0.1:8080
```

and:

```text
0.0.0.0:443
```

Which service is potentially more externally exposed?

**Answer:**

The service bound to `0.0.0.0:443` is potentially more exposed because it generally listens on all available IPv4 interfaces, while `127.0.0.1:8080` is bound to the local loopback interface.

Actual external reachability still depends on network and firewall controls.

---

## Q49.

An administrator says:

> "Port 443 is open, therefore HTTPS is definitely running."

How would you respond?

**Answer:**

Port 443 is commonly associated with HTTPS, but port numbers are conventions rather than proof. Service detection and application-level evidence should be used to confirm what is actually running.

---

## Q50.

Explain the complete investigation path when you discover an unexpected open port.

**Answer:**

A useful investigation path is:

```text
Open Port
    ↓
Protocol
    ↓
Service Identification
    ↓
Version
    ↓
Listening Process
    ↓
Binding Address
    ↓
Who Can Reach It?
    ↓
Why Is It Exposed?
    ↓
Is It Authorized?
    ↓
Security Risk
```

---

# Final Interview Challenge

## Q51.

Explain this to an interviewer:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

What can you infer?

**Expected Answer:**

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

The source port is likely an ephemeral client-side port.

Port 443 is commonly associated with HTTPS, but the port number alone does not prove the application.

If the traffic is TCP, the source and destination information forms part of the information used to identify the TCP flow.

---

# Key Interview Mental Model

When you discover a port:

```text
Port
 ↓
State
 ↓
Service
 ↓
Version
 ↓
Process
 ↓
Binding
 ↓
Reachability
 ↓
Exposure
 ↓
Business Purpose
 ↓
Security Risk
```

This is the mental model to carry into:

```text
SOC
Red Team
Security Engineering
GRC
Incident Response
Network Security
```
