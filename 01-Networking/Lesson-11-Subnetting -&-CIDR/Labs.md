# Lesson 11 - Subnetting & CIDR

# Labs

---

# Lab 01 - Understand Host Bits

## Objective

Understand how the CIDR prefix determines the number of host bits.

Calculate the host bits for:

```text
/24
/25
/26
/27
/28
```

Complete:

| CIDR  | Host Bits |
| ----- | --------: |
| `/24` |           |
| `/25` |           |
| `/26` |           |
| `/27` |           |
| `/28` |           |

Use:

```text
Host Bits = 32 - Prefix Length
```

---

# Lab 02 - Calculate Total and Usable Addresses

## Objective

Understand how host bits determine the size of a subnet.

Calculate total and usable addresses for:

```text
/24
/25
/26
/27
/28
```

Use:

```text
Total Addresses = 2^Host Bits

Usable Hosts = Total Addresses - 2
```

Complete:

| CIDR  | Host Bits | Total Addresses | Usable Hosts |
| ----- | --------: | --------------: | -----------: |
| `/24` |           |                 |              |
| `/25` |           |                 |              |
| `/26` |           |                 |              |
| `/27` |           |                 |              |
| `/28` |           |                 |              |

---

# Lab 03 - Find Network and Broadcast Addresses

## Objective

Practice identifying the subnet containing a given IP.

### Exercise 1

```text
192.168.50.77/27
```

Find:

```text
Network:
First Host:
Last Host:
Broadcast:
```

---

### Exercise 2

```text
192.168.10.145/28
```

Find:

```text
Network:
First Host:
Last Host:
Broadcast:
```

---

### Exercise 3

```text
192.168.50.140/27
```

Find:

```text
Network:
First Host:
Last Host:
Broadcast:
```

---

### Exercise 4

```text
172.16.35.200/28
```

Find:

```text
Network:
First Host:
Last Host:
Broadcast:
```

---

# Lab 04 - Block Size

## Objective

Practice finding subnet boundaries.

Calculate the block size for:

```text
/25
/26
/27
/28
/29
```

For the common last-octet cases:

```text
Block Size = 256 - Mask Value
```

Complete:

| CIDR  | Subnet Mask | Block Size |
| ----- | ----------- | ---------: |
| `/25` |             |            |
| `/26` |             |            |
| `/27` |             |            |
| `/28` |             |            |
| `/29` |             |            |

---

# Lab 05 - Subnetting in a Different Octet

## Objective

Understand that subnetting does not always happen in the last octet.

### Exercise

```text
172.16.50.10/20
```

Determine:

```text
Subnet Mask:
Interesting Octet:
Block Size:
Network Address:
Broadcast Address:
Usable Host Range:
```

---

# Lab 06 - Host Requirement

## Objective

Choose the smallest subnet that can satisfy a host requirement.

### Exercise 1

A team needs:

```text
50 hosts
```

Determine the smallest suitable prefix.

---

### Exercise 2

A team needs:

```text
100 hosts
```

Determine the smallest suitable prefix.

---

### Exercise 3

A team needs:

```text
25 hosts
```

Determine the smallest suitable prefix.

---

### Exercise 4

A team needs:

```text
10 hosts
```

Determine the smallest suitable prefix.

---

### Exercise 5

A management network needs:

```text
5 hosts
```

Determine the smallest suitable prefix.

---

# Lab 07 - VLSM Design

## Objective

Design multiple subnets of different sizes inside one larger network.

Starting network:

```text
192.168.50.0/24
```

Requirements:

```text
Web Servers    → 60 hosts
Application    → 25 hosts
Database       → 10 hosts
Management     → 5 hosts
```

### Step 1

Determine the smallest suitable prefix for each requirement.

### Step 2

Sort the requirements from largest to smallest.

### Step 3

Allocate the subnets without overlap.

### Step 4

Record:

```text
Network Address
First Usable
Last Usable
Broadcast
```

Complete:

| Department  | Hosts Needed | Prefix | Network | First Host | Last Host | Broadcast |
| ----------- | -----------: | ------ | ------- | ---------- | --------- | --------- |
| Web Servers |           60 |        |         |            |           |           |
| Application |           25 |        |         |            |           |           |
| Database    |           10 |        |         |            |           |           |
| Management  |            5 |        |         |            |           |           |

---

# Lab 08 - VLSM Security Architecture

## Scenario

An organization owns:

```text
192.168.100.0/24
```

They need:

```text
Engineering → 100 hosts
HR          → 50 hosts
Finance     → 20 hosts
Security    → 10 hosts
```

### Your Task

Design the network using VLSM.

Then answer:

1. Which network is the largest?
2. Which subnet should be allocated first?
3. How much address space remains?
4. Are the networks non-overlapping?
5. Could these subnets be used as security zones?
6. What additional security controls would still be required?

---

# Lab 09 - Packet Tracer Application

## Objective

Connect subnetting concepts to the router lab from previous lessons.

Use:

```text
Network 1:
192.168.1.0/24

Network 2:
192.168.2.0/24
```

Inspect the router:

```text
show ip route
```

Identify:

* Network prefixes
* Subnet masks
* Interfaces
* Directly connected routes

Then explain:

> Why does the router treat `192.168.1.0/24` and `192.168.2.0/24` as different networks?

---

# Lab 10 - Security Investigation Scenario

## Scenario

A SOC alert reports:

```text
Source:
10.40.20.55

Destination:
10.20.10.10
```

The network documentation states:

```text
10.40.0.0/16 → Guest Network

10.20.0.0/16 → Server Network
```

### Questions

1. Which network does the source belong to?
2. Which network does the destination belong to?
3. Should Guest devices normally communicate with Servers?
4. What network controls would you investigate?
5. Could routing or firewall configuration be involved?
6. What logs would you request?

---

# Lab 11 - GRC Architecture Review

## Scenario

A company states:

> "Guest users are isolated from production systems."

The architecture shows:

```text
Guest Network
     |
     |
Production Network
```

Both are different subnets.

### Questions

Would you consider the statement proven?

Explain why or why not.

Consider:

```text
Subnetting
Routing
Firewall
ACL
VLAN
Monitoring
Change Management
```

---

# Lab 12 - Final Subnetting Challenge

Without looking at your notes:

### A

```text
192.168.10.77/26
```

Find:

```text
Network:
Broadcast:
Usable Range:
```

### B

```text
192.168.10.145/28
```

Find:

```text
Network:
Broadcast:
Usable Range:
```

### C

```text
172.16.35.200/28
```

Find:

```text
Network:
Broadcast:
Usable Range:
```

### D

A network needs 60 usable hosts.

What prefix should be used?

### E

A network needs 10 usable hosts.

What prefix should be used?

---

# Lab Completion Checklist

* [ ] I understand network bits and host bits.
* [ ] I can calculate total addresses.
* [ ] I can calculate usable hosts.
* [ ] I can convert common CIDR prefixes to subnet masks.
* [ ] I can calculate block size.
* [ ] I can find subnet boundaries.
* [ ] I can find network addresses.
* [ ] I can find broadcast addresses.
* [ ] I can determine usable host ranges.
* [ ] I can select a subnet based on a host requirement.
* [ ] I can perform basic VLSM allocation.
* [ ] I can relate subnetting to network segmentation.
* [ ] I can apply subnet knowledge to a SOC scenario.
* [ ] I can discuss subnetting from a GRC perspective.

---

# Final Reflection

Write your own explanation of:

> **How does subnetting turn one large network into multiple smaller networks, and why is that useful for cybersecurity?**



Subnetting divides one large IP network into multiple smaller logical networks called subnets. For example, a `192.168.1.0/24` network can be divided into four `/26` subnets: `192.168.1.0/26`, `192.168.1.64/26`, `192.168.1.128/26`, and `192.168.1.192/26`. Each `/26` subnet contains 64 total IP addresses and 62 usable host IPs. Subnetting is useful for cybersecurity because it provides network segmentation, allowing organizations to separate employees, servers, security cameras, and guest Wi-Fi into different subnets. Firewalls, ACLs, and routing rules can then control communication between these subnets. For example, employees may be allowed to access servers, while guest Wi-Fi is blocked from accessing servers. This helps isolate compromised devices, limit lateral movement, reduce the attack surface, improve access control, and make network monitoring easier. **Subnetting itself is not a security mechanism; it becomes a security benefit when combined with firewalls, VLANs, ACLs, routing rules, and access controls.**

