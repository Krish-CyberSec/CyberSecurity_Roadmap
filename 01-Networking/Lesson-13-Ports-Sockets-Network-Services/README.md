# Lesson 13 - Ports, Sockets & Network Services

## Module Information

| Field          | Details                           |
| -------------- | --------------------------------- |
| Module         | Networking for Cybersecurity      |
| Lesson         | 13                                |
| Topic          | Ports, Sockets & Network Services |
| Difficulty     | Beginner → Intermediate           |
| OSI Layer      | Layer 4 – Transport               |
| Prerequisites  | Lessons 01–12                     |
| TryHackMe Room | Nmap Basic Port Scans             |
| Status         | Completed                         |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Understand what a network port represents.
* Understand the relationship between IP addresses, ports, and services.
* Explain what it means for a service to listen on a port.
* Distinguish between listening, open, closed, and filtered states.
* Understand the difference between local inspection and remote scanning.
* Understand TCP Connect Scans and TCP SYN Scans conceptually.
* Understand why UDP scanning behaves differently from TCP scanning.
* Understand service discovery and service enumeration.
* Understand why port numbers do not prove the service running behind them.
* Understand binding addresses such as `127.0.0.1` and `0.0.0.0`.
* Understand source ports and destination ports.
* Understand sockets and network-flow identification.
* Understand service exposure and attack surface.
* Understand the difference between port discovery, service enumeration, and vulnerability assessment.
* Apply port and service knowledge to SOC investigations.
* Apply service-exposure concepts to GRC and security architecture.
* Connect networking knowledge to authorized reconnaissance and Nmap.

---

# Part 1 — Ports, Services and Port States

## 1. Why Do We Need Ports?

In previous lessons, we learned that an IP address identifies a host.

For example:

```text
10.10.10.50
```

But a server can run many services at the same time:

```text
Web Server
SSH
DNS
Database
Mail Server
```

The operating system therefore needs to know:

> **Which application or service should receive this network traffic?**

Transport-layer ports help identify the service endpoint.

A useful mental model is:

```text
IP Address → Which host?

Port → Which service/application endpoint?
```

---

# 2. Example

Suppose a server has:

```text
10.10.10.50
```

and provides:

```text
10.10.10.50:22
10.10.10.50:443
```

The same host can provide multiple services.

For example:

```text
10.10.10.50:22
        ↓
SSH

10.10.10.50:443
        ↓
HTTPS
```

The IP identifies the host.

The port identifies the transport-layer endpoint.

---

# 3. What Does "Listening on a Port" Mean?

Suppose a server runs an SSH service:

```text
10.10.10.50:22/TCP
```

We can say:

> The SSH service is listening on TCP port 22.

Conceptually:

```text
             SERVER
         10.10.10.50
              |
              v
          TCP Port 22
              |
              v
          SSH Service
          "Waiting"
```

A listening service is waiting for incoming traffic destined for that endpoint.

---

# 4. Listening Does Not Mean Compromised

This is an important cybersecurity distinction.

If a server has:

```text
22/tcp LISTENING
```

that does not mean:

```text
The server is compromised.
```

It means:

> A process is listening on that transport endpoint.

The next questions are:

```text
Which service?
Why is it running?
Who can reach it?
Is it required?
Is it securely configured?
```

---

# 5. Open, Closed and Filtered

When performing network scanning, you may encounter different port states.

## Open

A service is reachable on the port.

Conceptually:

```text
Scanner
   |
   v
Target Port
   |
   v
Service responds
```

---

## Closed

The host is reachable, but no service is listening on that port.

Conceptually:

```text
Scanner
   |
   | Probe
   v
Target
   |
   | "No service here"
   v
Scanner
```

---

## Filtered

Something prevents the scanner from determining the actual port state.

For example:

```text
Scanner
   |
   v
Firewall / Filter
   X
Target
```

The scanner cannot confidently determine whether the port is open or closed.

---

# 6. Open Does Not Mean Vulnerable

Suppose Nmap reports:

```text
80/tcp open
```

That means:

> Something is reachable on TCP port 80.

It does not automatically mean:

```text
The server is vulnerable.
```

The progression should be:

```text
Port Exposure
      ↓
Service Identification
      ↓
Version Identification
      ↓
Configuration
      ↓
Security Assessment
```

---

# 7. Listening vs Open

These concepts are related but not identical.

### Local perspective

The operating system may report:

```text
LISTENING
```

This means:

> A local process is waiting for connections.

### Remote perspective

A scanner may report:

```text
OPEN
```

This means:

> From the scanner's location, the port appears reachable/open.

Therefore:

```text
Local machine
"What is running?"

Remote scanner
"What can I reach?"
```

---

# Part 2 — Sockets, Binding and Exposure

# 8. What Is a Socket?

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

or:

```text
10.10.10.50:443/TCP
```

---

# 9. TCP Four-Tuple

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
        |
        v
10.10.10.50:443
```

This provides:

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

This information is frequently visible in:

* Firewall logs
* Flow records
* SIEM data
* Packet captures
* SOC investigations

---

# 10. Source Port and Destination Port

In:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

the source port is:

```text
51544
```

and the destination port is:

```text
443
```

The destination port commonly represents the target service endpoint.

The source port is typically an ephemeral client-side port.

---

# 11. Why Source Ports Matter

Suppose one laptop establishes several connections:

```text
192.168.1.20:51001 → Server A:443

192.168.1.20:51002 → Server B:443

192.168.1.20:51003 → Server C:443
```

The different source ports help the operating system distinguish different communication flows.

---

# 12. TCP and UDP Have Separate Port Namespaces

TCP and UDP use separate transport namespaces.

Therefore:

```text
TCP 53
```

and:

```text
UDP 53
```

are different transport endpoints.

Likewise:

```text
TCP 443
UDP 443
```

are different endpoints.

This becomes important when analyzing DNS and modern protocols such as QUIC.

---

# 13. Binding to `127.0.0.1`

A service may bind to:

```text
127.0.0.1:8080
```

This means the service is bound to the local loopback interface.

Conceptually:

```text
Same machine
     |
     +----> 127.0.0.1:8080
```

A remote machine generally cannot directly access that loopback endpoint.

---

# 14. Binding to `0.0.0.0`

A service may instead bind to:

```text
0.0.0.0:8080
```

Conceptually, this generally means:

> Listen on all available IPv4 interfaces.

If the host has:

```text
10.10.10.50
192.168.1.50
```

the service may be reachable through those interfaces, subject to routing and firewall rules.

---

# 15. Why Binding Matters to Security

Imagine a developer starts:

```text
Test Web App
0.0.0.0:8000
```

They may have intended:

> "Only I should access this."

But binding to all interfaces increases potential exposure.

A security investigation should therefore consider:

```text
Binding Address
      ↓
Network Interface
      ↓
Routing
      ↓
Firewall
      ↓
Who can actually reach it?
```

---

# 16. Listening Does Not Automatically Mean Internet Exposure

Consider:

```text
Database
10.10.20.50:3306
```

The database may be listening.

But a firewall could restrict access:

```text
Application Network → Database
ALLOW

Internet → Database
DENY
```

So:

```text
Database
Listening ✅

Internet
Reachability ❌
```

Important principle:

> **A service can be listening locally without being externally reachable.**

---

# 17. Exposure as a Chain

Think of exposure as a sequence:

```text
Service exists
      ↓
Service is listening
      ↓
Service is bound to an interface
      ↓
A route exists
      ↓
Firewall/ACL permits traffic
      ↓
Remote system can reach it
```

Every layer matters.

---

# Part 3 — TCP and UDP Port Scanning

# 18. TCP Port Scanning

Suppose a scanner wants to determine whether:

```text
10.10.10.50:22
```

is reachable.

TCP gives useful response patterns because TCP has connection behavior.

---

# 19. TCP Connect Scan

A TCP Connect Scan attempts to complete the TCP connection.

Nmap syntax:

```bash
nmap -sT TARGET
```

Conceptually:

```text
Scanner
   |
   | SYN
   v
Target
   |
   | SYN-ACK
   v
Scanner
   |
   | ACK
   v
Connection established
```

This uses the normal TCP three-way handshake.

---

# 20. TCP SYN Scan

A TCP SYN scan probes using the SYN portion of TCP connection establishment.

Nmap syntax:

```bash
sudo nmap -sS TARGET
```

Conceptually:

```text
Scanner
   |
   | SYN
   v
Target
   |
   | SYN-ACK
   v
Scanner
   |
   | RST
   v
Connection terminated
```

The full TCP connection is not completed.

This connects directly to the TCP flags we learned in Lesson 12.

---

# 21. TCP Open/Closed Mental Model

A simplified model:

```text
SYN
 |
 ├── SYN-ACK → likely OPEN
 |
 ├── RST     → likely CLOSED
 |
 └── silence/filtering → may be FILTERED
```

Actual scanner behavior can be more nuanced, but this model is useful for understanding the fundamentals.

---

# 22. Why Filtering Matters

Suppose:

```text
TCP 8080
```

is listening on a server.

But a firewall blocks inbound traffic.

Locally:

```text
8080 → LISTENING
```

From another network:

```text
8080 → FILTERED
```

Both observations can be correct.

They answer different questions.

---

# 23. UDP Scanning

UDP is different.

There is no TCP-style handshake.

A scanner sends a UDP probe.

If the port is closed, the target may send:

```text
ICMP Destination Unreachable
Port Unreachable
```

Conceptually:

```text
Scanner
   |
   | UDP Probe
   v
Target
   |
   | ICMP Port Unreachable
   v
Scanner
```

This gives evidence that the port is closed.

---

# 24. Why UDP Scanning Is Harder

Suppose the UDP port is open.

The application may not respond to an unexpected probe.

So:

```text
Scanner
   |
   | UDP probe
   v
Open service
   |
   | No response
```

No response could mean:

```text
Open
```

or:

```text
Filtered
```

This is why UDP scanning can produce:

```text
open|filtered
```

in some situations.

---

# Part 4 — Service Discovery and Enumeration

# 25. Port Discovery vs Service Discovery

Suppose a scan reports:

```text
80/tcp open
```

What do we actually know?

We know:

> Something is reachable on TCP port 80.

We do not yet know with certainty which software is providing that service.

The next step is service discovery.

---

# 26. Service Enumeration

A useful progression is:

```text
Port Discovery
      ↓
Service Identification
      ↓
Version Identification
      ↓
Configuration
      ↓
Security Assessment
```

Example:

```text
80/tcp open
      ↓
HTTP
      ↓
nginx
      ↓
specific version
      ↓
configuration/security review
```

---

# 27. Nmap Service Detection

Nmap can perform service/version detection using:

```bash
nmap -sV TARGET
```

Conceptually:

```text
Target
  ↓
Open Port
  ↓
Nmap sends probes
  ↓
Service responds
  ↓
Nmap analyzes response
  ↓
Likely service/version
```

---

# 28. Why Nmap Can Identify Services

Nmap doesn't simply assume:

```text
80 = HTTP
```

It can send probes and analyze service responses.

For example, an HTTP server may return information resembling:

```text
HTTP/1.1 200 OK
Server: nginx
```

This gives the scanner evidence about the service.

---

# 29. Banners

Some services expose information such as:

```text
SSH-2.0-OpenSSH_9.x
```

This can suggest:

```text
Protocol:
SSH

Software:
OpenSSH

Version:
9.x
```

However, banners are not always reliable.

They can be:

* Modified
* Hidden
* Misleading
* Presented by a proxy

So:

> **Enumeration results are evidence, not absolute truth.**

---

# 30. Manual Service Enumeration

Service discovery does not have to rely only on Nmap.

For HTTP, for example:

```bash
curl -I http://TARGET
```

A response may include:

```text
HTTP/1.1 200 OK
Server: nginx
```

This gives direct evidence about the service.

---

# 31. Port Scanning vs Service Enumeration vs Vulnerability Assessment

These are different tasks.

### Port Scanning

Question:

> **Which ports are reachable?**

Example:

```text
22/tcp open
80/tcp open
443/tcp open
```

### Service Enumeration

Question:

> **What service/software is running?**

Example:

```text
22 → OpenSSH
80 → nginx
443 → Web application
```

### Vulnerability Assessment

Question:

> **Does this service have a security weakness?**

These should not be treated as the same activity.

---

# Part 5 — Local Host Investigation

# 32. Remote vs Local Perspective

A remote scan asks:

> **What can I reach from here?**

Local inspection asks:

> **What is running on this machine?**

For example:

```text
Remote:

Nmap
 ↓
Port State
 ↓
Reachability
```

while:

```text
Local:

ss / netstat
 ↓
Listening Socket
 ↓
PID
 ↓
Process
```

Both perspectives are useful.

---

# 33. Linux — `ss`

On Linux, a common command is:

```bash
ss -tulpn
```

It can help identify:

* TCP sockets
* UDP sockets
* Listening endpoints
* Numeric addresses/ports
* Associated processes, depending on permissions

Conceptually:

```text
TCP 0.0.0.0:22
TCP 0.0.0.0:443
UDP 0.0.0.0:53
```

Now you have more than just port information.

You can potentially connect:

```text
Port
 ↓
Protocol
 ↓
Process
```

---

# 34. Windows — `netstat`

On Windows, a common command is:

```cmd
netstat -ano
```

This can show network connections and the associated PID.

Conceptually:

```text
Port 443
   ↓
PID 1234
   ↓
Process
```

During incident response, this is useful for determining which process owns a suspicious listener.

---

# 35. Why PID Matters

Imagine you find:

```text
TCP 0.0.0.0:4444 LISTENING
PID 7312
```

The useful question isn't:

> "Is 4444 malicious?"

Instead:

```text
Which process?
      ↓
Which executable?
      ↓
Who started it?
      ↓
When?
      ↓
Why?
```

The port is only the starting clue.

---

# 36. Security Investigation Chain

A useful investigation flow is:

```text
Network Port
      ↓
Protocol
      ↓
Listening Address
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
      ↓
Command Line
      ↓
Network Connections
      ↓
Reason for Listening
```

This is how a simple network artifact becomes a real security investigation.

---

# Part 6 — Service Exposure and Attack Surface

# 37. What Is Service Exposure?

Suppose a public server exposes:

```text
22/tcp
80/tcp
443/tcp
3306/tcp
```

The security question isn't simply:

> "How many ports are open?"

Instead:

```text
What services?
Why exposed?
Who can reach them?
Are they necessary?
Are they securely configured?
```

---

# 38. Attack Surface

Attack surface is the collection of points through which an attacker could potentially interact with a system.

Exposed network services contribute to attack surface.

For example:

```text
Internet
   |
   +── TCP 22
   +── TCP 80
   +── TCP 443
   +── TCP 3306
```

Each exposed service should have a reason for existing and appropriate security controls.

---

# 39. Reduce the Attack Surface

Security teams can reduce exposure by:

```text
Remove unnecessary services
        ↓
Restrict network access
        ↓
Patch services
        ↓
Harden configuration
        ↓
Require authentication
        ↓
Monitor access
```

For example, rather than:

```text
Internet
   |
   +── Database :3306
```

a safer architecture may be:

```text
Internet
   |
Web/Application Layer
   |
Private Network
   |
Database
```

The database can still use port 3306 internally without being directly exposed to the Internet.

---

# 40. Port Exposure and Subnetting

Our previous subnetting lesson now becomes useful.

Imagine:

```text
Web Network
10.10.10.0/24

Database Network
10.10.20.0/24
```

The database might listen on:

```text
10.10.20.50:3306
```

but firewall policy could enforce:

```text
Web/App → Database
ALLOW

Internet → Database
DENY
```

This demonstrates:

```text
Subnetting
+
Routing
+
Firewalling
+
Service exposure
```

working together.

---

# Part 7 — Security Perspectives

# 41. SOC Perspective

Suppose a workstation starts listening on:

```text
0.0.0.0:8080
```

A SOC analyst should investigate:

```text
Which process?
Who launched it?
When did it start?
Why is it listening?
Is the software expected?
What connections does it make?
Was the machine recently modified?
```

The correct approach is evidence-driven.

---

# 42. Red Team Perspective

During authorized reconnaissance:

```text
Host Discovery
      ↓
Port Scanning
      ↓
Service Detection
      ↓
Version Detection
      ↓
Enumeration
      ↓
Vulnerability Research
```

The goal is to progressively learn more about the attack surface.

An open port does not automatically mean an exploit exists.

---

# 43. GRC Perspective

Imagine a GRC assessment identifies:

```text
Internet-facing server

22/tcp
443/tcp
3306/tcp
```

Useful questions include:

```text
Is SSH exposure necessary?
Is database exposure necessary?
Who approved these services?
Can access be restricted?
Are compensating controls present?
Is monitoring enabled?
Are exceptions documented?
```

This turns technical information into risk and control questions.

---

# 44. Why This Lesson Matters to Cybersecurity

Ports and services appear everywhere:

### SOC

```text
Ports
Protocols
Network flows
Listeners
Connections
```

### Security Engineering

```text
Firewall rules
Network exposure
Service architecture
Segmentation
```

### Red Team

```text
Port scanning
Service enumeration
Attack surface mapping
```

### GRC

```text
Approved services
Network controls
Exposure review
Change management
Risk assessment
```

---

# 45. TryHackMe Practice

## Room

**Nmap Basic Port Scans**

The room supports the practical concepts from this lesson, including:

* TCP Connect scanning
* TCP SYN scanning
* UDP scanning
* Port states
* Port selection
* Scan behavior

Use it only against the authorized TryHackMe targets provided by the platform.

The important learning sequence is:

```text
Scan
 ↓
Port State
 ↓
Service
 ↓
Version
 ↓
Security Relevance
```

The point is to understand **what Nmap is observing**, not just memorize command syntax.

---

# 46. OSI Model Mapping

Lesson 13 remains primarily connected to:

```text
Layer 4 — Transport
    |
    ├── TCP
    ├── UDP
    ├── Ports
    └── Sockets

Layer 3 — Network
    |
    ├── IP
    ├── Routing
    └── Subnets

Layer 2 — Data Link
    |
    ├── MAC
    ├── ARP
    └── Switch
```

We are using Layer 4 information to understand **services running on hosts and how those services are exposed to networks**.

---

# 47. Complete Mental Model

By the end of Lesson 13, the goal is to think:

```text
Host
  ↓
IP Address
  ↓
Port
  ↓
Protocol
  ↓
Listening Service
  ↓
Port State
  ↓
Service Identification
  ↓
Version
  ↓
Exposure
  ↓
Security Context
```

Or, from an investigation perspective:

```text
Something unusual
      ↓
Identify IP
      ↓
Identify Port
      ↓
Identify Protocol
      ↓
Determine Reachability
      ↓
Identify Service
      ↓
Identify Process
      ↓
Understand Purpose
      ↓
Assess Security Significance
```

---

# Key Takeaways

* An IP address identifies a host; a port identifies a transport-layer service endpoint.
* A listening port means a local process is waiting on that endpoint.
* `LISTENING` is a local host state, while `OPEN` is a scanner's observation of reachability.
* `CLOSED` generally means the host is reachable but no service is listening.
* `FILTERED` means filtering prevents a clear determination of port state.
* A service can be listening locally without being Internet-accessible.
* Binding to `127.0.0.1` generally limits access to the local machine.
* Binding to `0.0.0.0` generally allows listening on all IPv4 interfaces, subject to other controls.
* TCP Connect scans complete the TCP handshake.
* TCP SYN scans probe connection establishment without completing the full connection.
* UDP scanning behaves differently because UDP has no TCP-style handshake.
* Port scanning answers "what ports are reachable?"
* Service enumeration answers "what service/software is there?"
* Vulnerability assessment answers "is there a security weakness?"
* Nmap can use probes and responses to identify likely services and versions.
* Local tools such as `ss` and `netstat` can help identify listening sockets and associated processes.
* Open ports are not automatically vulnerabilities.
* Service exposure contributes to attack surface.
* Subnetting, routing, firewalling, and service configuration together determine practical exposure.
* Port and service information is valuable to SOC, Red Team, Security Engineering, and GRC professionals.

---

# Lesson 13 Checkpoint

Before moving on, you should be able to explain:

* [ ] What a port is.
* [ ] What a socket is.
* [ ] What listening means.
* [ ] Open vs closed vs filtered.
* [ ] Local inspection vs remote scanning.
* [ ] TCP Connect Scan vs SYN Scan.
* [ ] Why UDP scanning is different.
* [ ] What service discovery means.
* [ ] What service enumeration means.
* [ ] Why port numbers do not prove a service.
* [ ] What `127.0.0.1` means in service binding.
* [ ] What `0.0.0.0` generally means.
* [ ] Why a listening service may not be externally reachable.
* [ ] How to investigate a listening process locally.
* [ ] Port scanning vs service enumeration vs vulnerability assessment.
* [ ] Why service exposure contributes to attack surface.
* [ ] How subnetting, routing and firewalls affect exposure.
* [ ] Why this knowledge matters to SOC.
* [ ] Why this knowledge matters to Red Team.
* [ ] Why this knowledge matters to GRC.

---

# Summary

A port is a transport-layer service endpoint. A listening service represents an application waiting for network communication, while a remote scanner observes whether that endpoint is reachable from its location.

TCP and UDP behave differently during scanning because TCP provides connection-oriented behavior while UDP does not use a TCP-style handshake.

Port discovery is only the first stage of understanding a host. Service discovery and enumeration provide more information about what is actually running, while vulnerability assessment determines whether the identified service presents a security weakness.

A mature security investigation therefore moves from:

```text
Port
  ↓
Service
  ↓
Process
  ↓
Exposure
  ↓
Purpose
  ↓
Risk
```

rather than assuming that an open port automatically represents a vulnerability.

This lesson bridges the networking foundation into practical cybersecurity reconnaissance and defensive investigation.
