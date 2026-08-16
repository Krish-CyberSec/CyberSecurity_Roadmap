# Lesson 12 - TCP vs UDP

# Labs

---

# Lab 01 — Identify the Transport Protocol

## Objective

Learn to identify whether a connection uses TCP or UDP and explain why.

For each scenario, choose the most appropriate transport protocol and explain why.

### Scenario A

Large file transfer.

### Scenario B

Real-time voice communication.

### Scenario C

Typical DNS query.

### Scenario D

SSH session.

### Scenario E

Modern HTTP/3 connection.

Complete:

| Scenario      | TCP/UDP | Reason |
| ------------- | ------- | ------ |
| File transfer |         |        |
| Voice         |         |        |
| DNS           |         |        |
| SSH           |         |        |
| HTTP/3        |         |        |

---

# Lab 02 — TCP Three-Way Handshake

## Objective

Understand the TCP connection-establishment process.

Write the correct sequence:

```text
_____
_____
_____
```

Available:

```text
SYN
ACK
SYN-ACK
```

Then explain what each step means.

---

# Lab 03 — Analyze a TCP Flow

Consider:

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
Transport Protocol:
Common destination service:
```

Then answer:

> Why is `51544` likely to be an ephemeral client port?

---

# Lab 04 — TCP Reliability

Imagine:

```text
Segment 1 ✓
Segment 2 ✓
Segment 3 ✗
Segment 4 ✓
Segment 5 ✓
```

Answer:

1. What problem occurred?
2. How can TCP detect that data is missing?
3. What role do sequence numbers play?
4. What role do acknowledgements play?
5. What can TCP do to recover missing data?

---

# Lab 05 — Port Identification

Identify the common association for:

```text
22
23
25
53
80
443
445
3389
```

Complete:

| Port | Common Association | TCP/UDP/Common Use |
| ---: | ------------------ | ------------------ |
|   22 |                    |                    |
|   23 |                    |                    |
|   25 |                    |                    |
|   53 |                    |                    |
|   80 |                    |                    |
|  443 |                    |                    |
|  445 |                    |                    |
| 3389 |                    |                    |

Remember:

> Port numbers are common associations, not proof of the actual application.

---

# Lab 06 — Source vs Destination Port

Consider:

```text
10.10.5.20:49152
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

Then explain why the source port is likely ephemeral.

---

# Lab 07 — TCP vs UDP Security Analysis

Consider:

### Flow A

```text
10.10.5.20:52133
        ↓
10.20.10.50:443
TCP
```

### Flow B

```text
10.10.5.20:52134
        ↓
10.20.10.50:443
UDP
```

Questions:

1. Are both flows automatically malicious?
2. What legitimate protocol might explain Flow B?
3. What additional information would a SOC analyst investigate?

---

# Lab 08 — Port Scanning Scenario

A workstation produces:

```text
Destination:
10.10.10.20

Ports:
22
23
80
443
445
3389
```

Then repeats this against hundreds of hosts.

Questions:

1. What activity might this represent?
2. Why are these ports interesting?
3. What logs would you examine?
4. Which process generated the traffic?
5. Is this definitely malicious? Why or why not?

---

# Lab 09 — Firewall Rule Interpretation

Given:

```text
ALLOW
Source: 10.10.0.0/24
Destination: 10.20.0.0/24
Protocol: TCP
Destination Port: 443
```

Explain in plain language what traffic this rule permits.

Then analyze:

```text
DENY
Source: 10.30.0.0/24
Destination: 10.20.0.0/24
Protocol: TCP
Destination Port: 23
```

What does this rule prevent?

---

# Lab 10 — Wireshark Preparation

When you later use Wireshark, look for these fields in TCP traffic:

```text
Source IP
Destination IP
Source Port
Destination Port
TCP Flags
Sequence Number
Acknowledgement Number
```

Your task is to understand what each field represents before beginning detailed packet analysis.

---

# Lab 11 — Packet Tracer / Traffic Flow

Use the network from your previous lessons.

Observe:

```text
PC
 ↓
Switch
 ↓
Router
 ↓
Network 2
```

Then explain:

```text
Layer 4:
Which protocol?

Layer 3:
Which IP addresses?

Layer 2:
Which MAC addresses?

Layer 1:
What medium carries the data?
```

The goal is to connect the Transport Layer to the networking concepts you already learned.

---

# Lab 12 — Security Scenario

A server exposes:

```text
TCP 22
TCP 80
TCP 443
UDP 53
```

Questions:

1. What services might these ports represent?
2. Which ones require further validation?
3. Which services should be exposed to the Internet?
4. What firewall controls might be appropriate?
5. What monitoring would you recommend?

Do not assume the port numbers alone prove which services are running.

---

# Lab 13 — GRC Review Scenario

A policy states:

> Only approved services may communicate with the production environment.

The firewall configuration contains:

```text
ALLOW TCP 443
ALLOW TCP 22
ALLOW UDP 53
ALLOW UDP 50000-60000
```

As a GRC reviewer:

1. What questions would you ask?
2. Which rules need business justification?
3. Which rules may require additional context?
4. How would you verify that the rules align with policy?
5. What evidence would you request?

---

# Final Assessment

## Question 1

Explain why TCP uses a three-way handshake.

## Question 2

Explain why UDP does not need a TCP-style handshake.

## Question 3

Explain the difference between:

```text
IP address
Port
Socket
```

## Question 4

Explain why a TCP connection can be identified using:

```text
Source IP
Source Port
Destination IP
Destination Port
```

## Question 5

Why can UDP 443 be legitimate?

## Question 6

A host sends thousands of SYN packets across many ports. What would you investigate?

## Question 7

A firewall allows TCP 443 but blocks TCP 23. Explain the security purpose.

---

# Lab Completion Checklist

* [ ] I understand the purpose of Layer 4.
* [ ] I can explain TCP vs UDP.
* [ ] I understand the TCP three-way handshake.
* [ ] I understand sequence numbers conceptually.
* [ ] I understand acknowledgements.
* [ ] I understand retransmission.
* [ ] I understand source and destination ports.
* [ ] I understand ephemeral ports.
* [ ] I understand sockets and the TCP four-tuple.
* [ ] I can interpret basic firewall rules.
* [ ] I can reason about suspicious TCP and UDP activity.
* [ ] I understand why UDP 443 can be legitimate.
* [ ] I can connect transport concepts to SOC and GRC work.
