# Lesson 13 - Ports, Sockets & Network Services

# Labs

---

# Lab 01 — Identify Port, Host and Service

## Objective

Understand the relationship between an IP address, port and service.

Given:

```text
10.10.10.50:22
10.10.10.50:80
10.10.10.50:443
```

Identify the likely service associated with each.

| Endpoint          | Likely Service |
| ----------------- | -------------- |
| `10.10.10.50:22`  |                |
| `10.10.10.50:80`  |                |
| `10.10.10.50:443` |                |

### Questions

1. What does the IP address identify?
2. What does the port identify?
3. Can the same IP have multiple services?

---

# Lab 02 — Open, Closed or Filtered?

## Objective

Practice interpreting basic port states.

For each scenario, determine whether the port should be considered:

```text
Open
Closed
Filtered
```

### Scenario A

A server responds to a TCP SYN with SYN-ACK.

```text
Scanner
   |
   | SYN
   v
Server
   |
   | SYN-ACK
   v
Scanner
```

State:

```text
________________
```

---

### Scenario B

A server responds with TCP RST.

```text
Scanner
   |
   | SYN
   v
Server
   |
   | RST
   v
Scanner
```

State:

```text
________________
```

---

### Scenario C

A firewall silently drops the probe.

```text
Scanner
   |
   | SYN
   v
Firewall
   X
```

State:

```text
________________
```

---

# Lab 03 — Listening vs Reachable

## Scenario

A Linux server shows:

```text
TCP 0.0.0.0:8080 LISTENING
```

But a remote Nmap scan reports:

```text
8080/tcp filtered
```

### Questions

1. Is the service running?
2. Is the service definitely reachable remotely?
3. What could explain the difference?
4. What security control would you investigate?

Write your explanation:

```text
________________________________________________

________________________________________________

________________________________________________
```

---

# Lab 04 — Binding Address

## Objective

Understand the difference between loopback and all-interface binding.

Consider:

```text
Service A:

127.0.0.1:8080
```

and:

```text
Service B:

0.0.0.0:8080
```

### Questions

1. Which service is generally intended for local access?
2. Which service may be reachable through multiple IPv4 interfaces?
3. Why does binding address matter to security?
4. Does `0.0.0.0:8080` automatically mean Internet access?

---

# Lab 05 — Source and Destination Ports

Consider:

```text
192.168.1.20:51544
        ↓
10.10.10.50:443
```

Identify:

```text
Source IP:
Source Port:

Destination IP:
Destination Port:
```

Then answer:

1. Which port is likely ephemeral?
2. Which port is commonly associated with HTTPS?
3. Why does the client need a source port?

---

# Lab 06 — TCP Connect vs SYN Scan

## Objective

Understand what each scan is actually doing.

### TCP Connect

```bash
nmap -sT TARGET
```

Expected conceptual sequence:

```text
SYN
SYN-ACK
ACK
```

### SYN Scan

```bash
sudo nmap -sS TARGET
```

Expected conceptual sequence:

```text
SYN
SYN-ACK
RST
```

### Questions

1. Which scan completes the TCP handshake?
2. Which scan does not complete the full connection?
3. Why does understanding TCP behavior help explain Nmap results?

---

# Lab 07 — UDP Scanning

## Objective

Understand why UDP scanning can be ambiguous.

Suppose a UDP scan sends:

```text
UDP Probe
```

### Scenario A

Target responds:

```text
ICMP Port Unreachable
```

Likely state:

```text
________________
```

### Scenario B

Target provides no response.

Possible states:

```text
________________
```

Explain why the second situation is ambiguous.

---

# Lab 08 — Nmap Basic Port Scan

## TryHackMe Practice

### Room

**Nmap Basic Port Scans**

Room:

```text
https://tryhackme.com/room/nmap02
```

Use only the authorized targets provided by TryHackMe.

### Focus

Complete the room sections covering:

* TCP Connect scans
* TCP SYN scans
* UDP scans
* Port states
* Port selection
* Scan behavior

### Learning Log

Record:

```text
Room:
Date:
Target:
Important concepts:
Commands learned:
Unexpected observation:
Security takeaway:
```

### Do Not Do

Do not blindly memorize:

```text
-sT
-sS
-sU
```

Instead explain what each scan is trying to determine.

---

# Lab 09 — Service Discovery

## Objective

Move from:

```text
Port Discovery
```

to:

```text
Service Identification
```

Given:

```text
80/tcp open
```

Answer:

1. What do we know?
2. What do we not know?
3. What should we do next?

Conceptual sequence:

```text
80/tcp open
      ↓
Service Detection
      ↓
HTTP?
      ↓
Server Software?
      ↓
Version?
```

---

# Lab 10 — Nmap Service Detection

Use the authorized TryHackMe target from the room.

Example:

```bash
nmap -sV TARGET
```

Record the results.

| Port | State | Detected Service | Version |
| ---: | ----- | ---------------- | ------- |
|      |       |                  |         |
|      |       |                  |         |
|      |       |                  |         |

### Questions

1. Why does service detection provide more information than a basic port scan?
2. Does the detected service guarantee the actual software?
3. Why should enumeration results be treated as evidence?

---

# Lab 11 — Manual HTTP Enumeration

On an authorized HTTP target:

```bash
curl -I http://TARGET
```

Record:

```text
HTTP Status:
Server Header:
Other Interesting Headers:
```

### Questions

1. What does the response tell you?
2. Can the `Server` header always be trusted?
3. How is this different from simply knowing that port 80 is open?

---

# Lab 12 — Local Linux Investigation

On a Linux lab machine, run:

```bash
ss -tulpn
```

Record several listening endpoints:

| Protocol | Address | Port | Process |
| -------- | ------- | ---: | ------- |
|          |         |      |         |
|          |         |      |         |
|          |         |      |         |

### Questions

1. Which services are listening?
2. Which are bound to `127.0.0.1`?
3. Which are bound to `0.0.0.0`?
4. Which services might be externally reachable?
5. What additional controls would you check?

---

# Lab 13 — Local Windows Investigation

On a Windows lab machine, run:

```cmd
netstat -ano
```

Identify at least three listening or established connections.

Record:

| Local Endpoint | Remote Endpoint | State | PID |
| -------------- | --------------- | ----- | --- |
|                |                 |       |     |
|                |                 |       |     |
|                |                 |       |     |

Then identify the process associated with one interesting PID.

### Investigation Chain

```text
Port
 ↓
PID
 ↓
Process
 ↓
Executable
 ↓
Purpose
```

---

# Lab 14 — Port Scanning vs Service Enumeration

Consider:

```text
22/tcp open
80/tcp open
443/tcp open
```

### Task 1

Classify this as:

```text
Port Scanning
Service Enumeration
Vulnerability Assessment
```

### Task 2

Now suppose you discover:

```text
22 → OpenSSH
80 → nginx
443 → nginx
```

What stage are you now in?

### Task 3

Suppose you identify:

```text
nginx version X
```

What should happen next?

---

# Lab 15 — Security Investigation

## Scenario

A workstation unexpectedly starts listening on:

```text
0.0.0.0:4444
```

PID:

```text
7312
```

### Investigate

Build this chain:

```text
Port
 ↓
Protocol
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
Purpose
```

### Questions

1. Is port 4444 automatically malicious?
2. What evidence would increase your suspicion?
3. What evidence might prove it is legitimate?
4. What would you document in the incident?

---

# Lab 16 — Attack Surface Review

A public server exposes:

```text
22/tcp
80/tcp
443/tcp
3306/tcp
```

Create an exposure table:

| Port | Common Service | Why Might It Be Exposed? | Security Question |
| ---: | -------------- | ------------------------ | ----------------- |
|   22 |                |                          |                   |
|   80 |                |                          |                   |
|  443 |                |                          |                   |
| 3306 |                |                          |                   |

Then answer:

> Which service would you question most strongly and why?

---

# Lab 17 — Network Architecture Scenario

Consider:

```text
                Internet
                   |
                Firewall
                   |
          +--------+--------+
          |                 |
       Web Tier          Database
    10.10.10.0/24      10.10.20.0/24
          |                 |
        TCP 443           TCP 3306
```

Required policy:

```text
Internet → Web
ALLOW

Web → Database
ALLOW

Internet → Database
DENY
```

### Questions

1. Why is the database service still allowed to listen on port 3306?
2. Why isn't listening on 3306 itself a problem?
3. What security control prevents Internet access?
4. How do subnetting and firewalling work together here?

---

# Lab 18 — SOC Scenario

A workstation makes connection attempts to:

```text
22
23
80
443
445
3389
```

across hundreds of hosts.

### Investigate

1. What activity might this represent?
2. Which telemetry would you examine?
3. Which process initiated the activity?
4. Is the activity authorized?
5. Is the behavior normal for this system?
6. What could distinguish security scanning from malicious scanning?

---

# Lab 19 — GRC Review

Policy:

> Only approved and necessary network services may be exposed.

Architecture:

```text
Internet-facing Server

22/tcp
443/tcp
3306/tcp
```

### Questions

1. What evidence would you request?
2. Why is 3306 worth reviewing?
3. Should 22 automatically be removed?
4. What compensating controls could reduce risk?
5. How would you verify compliance with the policy?

---

# Lab 20 — Final Integrated Scenario

You discover:

```text
Server:
10.10.10.50
```

Remote scan:

```text
22/tcp open
80/tcp open
443/tcp open
3306/tcp filtered
```

Local inspection:

```text
0.0.0.0:22 LISTENING
0.0.0.0:80 LISTENING
0.0.0.0:443 LISTENING
0.0.0.0:3306 LISTENING
```

### Questions

Explain why the results aren't contradictory.

Then determine:

```text
1. Which services are locally listening?
2. Which services appear reachable remotely?
3. Which service is being filtered?
4. What security control might explain the difference?
5. What would you investigate next?
```

---

# Final Lesson Challenge

Without looking at your notes, explain this chain:

```text
10.10.10.50:443
```

from beginning to end:

```text
IP
 ↓
Port
 ↓
Socket
 ↓
Listening
 ↓
Reachability
 ↓
Port State
 ↓
Service
 ↓
Version
 ↓
Exposure
 ↓
Security Risk
```

Then explain the difference between:

```text
Port Discovery
Service Enumeration
Vulnerability Assessment
```

---

# Lab Completion Checklist

* [ ] I understand what a port represents.
* [ ] I understand listening ports.
* [ ] I understand sockets.
* [ ] I understand source and destination ports.
* [ ] I understand ephemeral ports.
* [ ] I understand open, closed and filtered.
* [ ] I understand local vs remote observations.
* [ ] I understand `127.0.0.1` vs `0.0.0.0`.
* [ ] I practiced TCP Connect scanning.
* [ ] I understand SYN scanning.
* [ ] I understand UDP scanning.
* [ ] I completed the aligned TryHackMe room.
* [ ] I practiced service detection.
* [ ] I practiced local socket inspection.
* [ ] I understand service enumeration.
* [ ] I understand attack surface.
* [ ] I can investigate a suspicious listener.
* [ ] I can analyze service exposure from a SOC perspective.
* [ ] I can analyze service exposure from a GRC perspective.
