# Lesson 11 - Subnetting and CIDR

## Module Information

| Field         | Details                      |
| ------------- | ---------------------------- |
| Module        | Networking for Cybersecurity |
| Lesson        | 11                           |
| Topic         | Subnetting & CIDR            |
| Difficulty    | Beginner → Intermediate      |
| Prerequisites | Lessons 01–10                |
| OSI Layer     | Layer 3 – Network            |
| Status        | Completed                    |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Explain why subnetting is necessary.
* Understand CIDR notation.
* Explain network bits and host bits.
* Understand why `/25` divides a `/24` network into two subnets.
* Calculate the number of addresses in a subnet.
* Calculate usable host addresses.
* Determine subnet boundaries.
* Find network and broadcast addresses.
* Determine usable host ranges.
* Choose a subnet based on a host requirement.
* Understand Variable Length Subnet Masking (VLSM).
* Explain how subnetting supports network segmentation and security architecture.

---

# 1. What Problem Does Subnetting Solve?

Imagine an organization receives:

```text
192.168.1.0/24
```

This is one large network containing 256 IPv4 addresses.

Instead of putting every device into one network, the organization may want separate networks for:

* Employees
* Servers
* Security systems
* Guest devices
* Management

Subnetting allows one larger network to be divided into smaller networks.

> **Subnetting is the process of dividing a larger IP network into smaller logical networks.**

---

# 2. IPv4 and Host Bits

IPv4 addresses contain:

```text
32 bits
```

A CIDR prefix tells us how many bits belong to the network portion.

Example:

```text
192.168.1.0/24
```

means:

```text
24 network bits
8 host bits
```

Therefore:

```text
32 - 24 = 8 host bits
```

---

# 3. Why Does /25 Create Two Networks?

Start with:

```text
192.168.1.0/24
```

A `/24` has:

```text
24 network bits
8 host bits
```

Changing it to `/25` means one host bit is borrowed for the network portion:

```text
/24

24 network bits | 8 host bits
```

becomes:

```text
/25

25 network bits | 7 host bits
```

That newly borrowed bit can have two values:

```text
0
1
```

Therefore, the original `/24` can be divided into:

```text
192.168.1.0/25
192.168.1.128/25
```

So:

```text
1 borrowed bit
        ↓
2¹ = 2 subnets
```

---

# 4. Why Does /26 Create Four Networks?

Starting with `/24`:

```text
24 network bits
8 host bits
```

Moving to `/26` borrows two host bits:

```text
26 network bits
6 host bits
```

Two bits have four possible combinations:

```text
00
01
10
11
```

Therefore:

```text
2² = 4 subnets
```

For:

```text
192.168.1.0/24
```

the four `/26` networks are:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

---

# 5. Host Capacity

The number of host bits determines the number of addresses available.

```text
Total Addresses = 2^host_bits
```

For traditional IPv4 subnetting:

```text
Usable Hosts = 2^host_bits - 2
```

The two excluded addresses are:

* Network address
* Broadcast address

Examples:

| CIDR  | Host Bits | Total Addresses | Usable Hosts |
| ----- | --------: | --------------: | -----------: |
| `/24` |         8 |             256 |          254 |
| `/25` |         7 |             128 |          126 |
| `/26` |         6 |              64 |           62 |
| `/27` |         5 |              32 |           30 |
| `/28` |         4 |              16 |           14 |
| `/29` |         3 |               8 |            6 |

---

# 6. CIDR and Subnet Masks

CIDR notation is a compact representation of a subnet mask.

Common examples:

| CIDR  | Subnet Mask       |
| ----- | ----------------- |
| `/24` | `255.255.255.0`   |
| `/25` | `255.255.255.128` |
| `/26` | `255.255.255.192` |
| `/27` | `255.255.255.224` |
| `/28` | `255.255.255.240` |
| `/29` | `255.255.255.248` |
| `/30` | `255.255.255.252` |

---

# 7. Block Size

When the subnetting occurs in an octet, block size can be calculated as:

```text
Block Size = 256 - Mask Value
```

For `/26`:

```text
Subnet Mask:

255.255.255.192
```

Therefore:

```text
256 - 192 = 64
```

The subnet boundaries are:

```text
0
64
128
192
```

---

# 8. Finding Network and Broadcast Addresses

Consider:

```text
192.168.50.77/27
```

`/27` gives:

```text
5 host bits
```

Therefore:

```text
2^5 = 32 addresses per subnet
```

Subnet boundaries:

```text
0
32
64
96
128
...
```

The value `77` lies between:

```text
64 and 96
```

Therefore:

```text
Network:
192.168.50.64
```

The next subnet starts at:

```text
192.168.50.96
```

Therefore the broadcast address is one address before it:

```text
Broadcast:
192.168.50.95
```

Usable range:

```text
192.168.50.65
-
192.168.50.94
```

---

# 9. Another Example

Consider:

```text
192.168.10.145/28
```

A `/28` has:

```text
4 host bits
2⁴ = 16 addresses
```

Boundaries:

```text
0
16
32
48
64
80
96
112
128
144
160
...
```

`145` falls within:

```text
144 – 159
```

Therefore:

```text
Network:
192.168.10.144

First Host:
192.168.10.145

Last Host:
192.168.10.158

Broadcast:
192.168.10.159
```

---

# 10. Subnetting Beyond the Last Octet

Subnetting does not always happen in the final octet.

Consider:

```text
172.16.50.10/20
```

The subnet mask is:

```text
255.255.240.0
```

The interesting octet is the third octet:

```text
255.255.240.0
       ↑
```

Block size:

```text
256 - 240 = 16
```

The third-octet boundaries are:

```text
0
16
32
48
64
80
...
```

The value `50` falls between:

```text
48 – 63
```

Therefore:

```text
Network:
172.16.48.0/20

Broadcast:
172.16.63.255
```

This is why subnetting cannot be reduced to a last-octet memorization trick.

---

# 11. Designing a Subnet From a Requirement

Suppose a team needs:

```text
50 hosts
```

We need enough host bits to satisfy:

```text
2^h - 2 >= 50
```

Try 5 host bits:

```text
2^5 - 2 = 30
```

Not enough.

Try 6:

```text
2^6 - 2 = 62
```

Enough.

Therefore:

```text
32 - 6 = 26
```

The smallest suitable subnet is:

```text
/26
```

---

# 12. Common Requirements

| Hosts Needed | Suitable Prefix | Usable Hosts |
| -----------: | --------------: | -----------: |
|            5 |           `/29` |            6 |
|           10 |           `/28` |           14 |
|           20 |           `/27` |           30 |
|           25 |           `/27` |           30 |
|           30 |           `/27` |           30 |
|           50 |           `/26` |           62 |
|           60 |           `/26` |           62 |
|          100 |           `/25` |          126 |

The goal is to choose the **smallest subnet that satisfies the requirement**.

---

# 13. VLSM

**VLSM (Variable Length Subnet Masking)** allows different subnet sizes to be used within the same larger network.

Example:

```text
Engineering → /25
HR          → /26
Finance     → /27
Security    → /28
```

This is more efficient than assigning the same large subnet to every department.

---

# 14. VLSM Example

Starting network:

```text
192.168.50.0/24
```

Requirements:

```text
Web Servers  → 60 hosts
Application  → 25 hosts
Database     → 10 hosts
Management   → 5 hosts
```

Suitable subnet sizes:

```text
Web Servers → /26
Application → /27
Database    → /28
Management  → /29
```

Allocate the largest subnet first:

```text
Web Servers
192.168.50.0/26
.0 – .63
```

Next:

```text
Application
192.168.50.64/27
.64 – .95
```

Next:

```text
Database
192.168.50.96/28
.96 – .111
```

Next:

```text
Management
192.168.50.112/29
.112 – .119
```

Remaining address space:

```text
.120 – .255
```

---

# 15. Why VLSM Matters

Without VLSM, every department might receive the same subnet size.

That can create significant address waste.

VLSM allows the address space to reflect actual requirements.

Conceptually:

```text
Requirements
     ↓
Choose smallest suitable subnet
     ↓
Sort largest → smallest
     ↓
Allocate at valid boundaries
     ↓
Repeat
```

---

# 16. Cybersecurity Perspective

Subnetting is important because it provides logical network boundaries.

For example:

```text
10.10.0.0/24  → Employees
10.20.0.0/24  → Servers
10.30.0.0/24  → Security
10.40.0.0/24  → Guest
```

These boundaries can then be used with:

* Firewalls
* ACLs
* VLANs
* Routing policies
* Network monitoring
* Security groups

Important:

> **Different subnets do not automatically mean that traffic between them is blocked.**

Routing and security controls determine whether communication is allowed.

---

# 17. SOC Perspective

Subnet knowledge helps analysts understand network context.

For example:

```text
Source:
10.40.20.55

Destination:
10.20.10.10
```

If the security team knows:

```text
10.40.0.0/16 → Guest
10.20.0.0/16 → Servers
```

the alert immediately has more context.

The analyst can ask:

> Why is a guest device communicating with a server network?

---

# 18. GRC Perspective

A GRC professional may encounter subnetting during:

* Network architecture reviews
* Segmentation assessments
* Change management
* Security control assessments
* Infrastructure audits
* Security architecture reviews

Useful questions include:

> Are sensitive systems placed in appropriate network segments?

> What controls restrict traffic between segments?

> Can guest systems reach production systems?

> Does a network change affect an existing security boundary?

---

# 19. Connection to Previous Lessons

Subnetting connects several things we've already learned:

```text
IP Address
    ↓
Subnet Mask / CIDR
    ↓
Network Boundary
    ↓
Routing Table
    ↓
Longest Prefix Match
    ↓
Traffic Path
    ↓
Security Architecture
```

This is why subnetting is not an isolated calculation topic.

It is part of understanding how networks are designed and how traffic moves through them.

---

# 20. OSI Model Mapping

| OSI Layer               | Topics                                                                         |
| ----------------------- | ------------------------------------------------------------------------------ |
| Layer 7 – Application   | Browser / Applications                                                         |
| Layer 6 – Presentation  | Not studied yet                                                                |
| Layer 5 – Session       | Not studied yet                                                                |
| Layer 4 – Transport     | Not studied yet                                                                |
| **Layer 3 – Network**   | **IP Address, Router, Routing Table, Route Selection, Subnetting, CIDR, VLSM** |
| **Layer 2 – Data Link** | **MAC Address, ARP, Switch**                                                   |
| Layer 1 – Physical      | Ethernet / Wi-Fi                                                               |

---

# Key Takeaways

* Subnetting divides a larger network into smaller networks.
* Increasing the prefix length borrows bits from the host portion.
* More network bits create smaller and more specific networks.
* Host capacity decreases as the prefix gets longer.
* CIDR notation represents the network prefix length.
* Network and broadcast addresses define the boundaries of a subnet.
* Block size helps identify subnet boundaries efficiently.
* Host requirements can be used to determine the appropriate subnet size.
* VLSM allows different subnet sizes within the same address space.
* Subnetting supports network segmentation but does not itself enforce security.
* Subnet knowledge is useful in SOC, Security Engineering, Cloud Security, Red Teaming, and GRC.

---

# Before You Move On

You should now be able to:

* [ ] Explain why subnetting exists.
* [ ] Explain why `/25` divides a `/24` into two subnets.
* [ ] Calculate host bits.
* [ ] Calculate total addresses.
* [ ] Calculate usable addresses.
* [ ] Convert common CIDR prefixes to subnet masks.
* [ ] Find block size.
* [ ] Find network and broadcast addresses.
* [ ] Find usable host ranges.
* [ ] Choose a subnet based on a host requirement.
* [ ] Perform basic VLSM allocation.
* [ ] Explain how subnetting supports security architecture.

---

# Summary

Subnetting is the process of dividing a larger IP network into smaller logical networks.

CIDR notation tells us how many bits belong to the network prefix. By increasing the prefix length, we borrow bits from the host portion, creating more smaller networks while reducing the number of hosts available in each subnet.

VLSM extends this concept by allowing different subnet sizes to be allocated according to actual requirements.

These concepts form an important foundation for routing, VLANs, firewalls, cloud networking, network segmentation, SOC investigations, and security architecture.
