# Lesson 14 - TryHackMe

## Room

**Wireshark 101**

Platform:

[TryHackMe](https://tryhackme.com/room/wireshark)

---

# Why This Room?

Lesson 14 focuses on:

```text
TCP Flags
TCP Connection States
TCP Handshake
TCP Termination
RST
FIN
Packet Analysis
```

The Wireshark 101 room gives practical exposure to packet captures and TCP traffic analysis.

The purpose is not to memorize Wireshark commands.

The purpose is to connect the TCP concepts learned in this lesson with actual packets.

---

# What To Focus On

While completing the room, pay particular attention to TCP traffic containing:

```text
SYN
SYN-ACK
ACK
PSH / ACK
FIN
RST
```

Try to identify:

```text
Connection Establishment
        ↓
Data Transfer
        ↓
Connection Termination
```

---

# Learning Objectives

After completing the room, you should be able to:

* Identify TCP traffic in a packet capture.
* Recognize a TCP three-way handshake.
* Identify SYN, SYN-ACK and ACK packets.
* Recognize TCP data transfer.
* Identify FIN packets.
* Identify RST packets.
* Recognize graceful connection termination.
* Distinguish normal TCP behavior from unusual traffic patterns.
* Connect packet observations with TCP connection states.

---

# Practical Observation

For at least one TCP conversation, record:

```text
Source IP:
Source Port:

Destination IP:
Destination Port:

Protocol:
```

Then identify:

```text
SYN:
SYN-ACK:
ACK:
Data packets:
FIN:
RST:
```

---

# Packet Story

For one TCP conversation, write the sequence:

```text
Example:

SYN
↓
SYN-ACK
↓
ACK
↓
PSH-ACK
↓
ACK
↓
FIN
↓
ACK
↓
FIN
↓
ACK
```

Then explain the story in your own words.

Example structure:

```text
1. Connection was initiated.
2. Server responded.
3. Connection was established.
4. Data was exchanged.
5. Connection was gracefully terminated.
```

---

# Security Observations

Look for traffic that might resemble:

```text
SYN
SYN
SYN
SYN
...
```

or:

```text
SYN
SYN-ACK
RST
```

Ask:

```text
What could this indicate?

Is it normal?

Could it be scanning?

Could it be legitimate security tooling?

What additional evidence would I need?
```

---

# Connection With Lesson 13

Lesson 13 taught:

```text
Port
 ↓
Port State
 ↓
Service
 ↓
Reachability
```

Lesson 14 adds:

```text
TCP Connection
 ↓
Flags
 ↓
States
 ↓
Packet Sequence
```

Together:

```text
Port
 ↓
Service
 ↓
TCP Connection
 ↓
TCP Flags
 ↓
Connection State
 ↓
Traffic Analysis
```

---

# Connection With Cybersecurity

### SOC

Use packet information to investigate:

```text
Scanning
Suspicious Connections
Unexpected Resets
Unusual TCP Patterns
```

### Red Team

Understand:

```text
Port Scanning
SYN Scanning
Network Reconnaissance
```

### Security Engineering

Understand:

```text
Firewall Behavior
IDS/IPS Detection
Connection Handling
```

### GRC

Understand technical findings involving:

```text
Network Monitoring
Security Controls
Availability
Incident Response
```

---

# Personal Notes

## Room Completed

```text
[ ] Yes
[ ] No
```

## Date

```text
________________
```

## Most Important Thing I Learned

```text
________________________________________________

________________________________________________
```

## Something I Found Confusing

```text
________________________________________________

________________________________________________
```

## Security Connection I Noticed

```text
________________________________________________

________________________________________________
```

## One Thing I Can Now Explain

```text
________________________________________________

________________________________________________
```

---

# Key Takeaway

The goal of this room is to move from:

```text
"I know what SYN and FIN mean."
```

to:

```text
"I can look at a packet sequence and understand
what is happening in the TCP connection."
```

That is the practical skill this lesson is building.
