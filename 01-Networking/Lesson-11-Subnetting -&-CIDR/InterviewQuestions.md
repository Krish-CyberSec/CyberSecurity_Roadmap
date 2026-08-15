# Lesson 11 - Subnetting & CIDR

# Interview Questions

## Beginner Level

### Q1. What is subnetting?

**Answer:**

Subnetting is the process of dividing a larger IP network into smaller logical networks called subnets.

---

### Q2. Why is subnetting used?

**Answer:**

Subnetting is used to organize IP address space, reduce unnecessary address usage, create logical network boundaries, and support network design and segmentation.

---

### Q3. What does `/24` mean?

**Answer:**

`/24` means that 24 of the 32 IPv4 bits are part of the network prefix and 8 bits remain for hosts.

---

### Q4. How many usable hosts are available in a `/26` subnet?

**Answer:**

A `/26` has:

```text
32 - 26 = 6 host bits
```

Therefore:

```text
2^6 = 64 total addresses
64 - 2 = 62 usable hosts
```

---

### Q5. What is CIDR?

**Answer:**

CIDR (Classless Inter-Domain Routing) is a notation used to represent an IP network and its prefix length.

Example:

```text
192.168.10.0/24
```

---

## Intermediate Level

### Q6. Why does `/25` divide a `/24` network into two subnets?

**Answer:**

A `/24` has 8 host bits. Moving to `/25` borrows one host bit for the network portion.

That bit has two possible values:

```text
0
1
```

Therefore:

```text
2^1 = 2 subnets
```

---

### Q7. How many subnets are created when a `/24` is divided into `/26` networks?

**Answer:**

Two bits are borrowed:

```text
26 - 24 = 2
```

Therefore:

```text
2^2 = 4 subnets
```

---

### Q8. What is the subnet mask for `/27`?

**Answer:**

```text
255.255.255.224
```

---

### Q9. What is block size?

**Answer:**

Block size represents the number of addresses contained in each subnet within the relevant octet.

For example, `/26` has a block size of:

```text
256 - 192 = 64
```

when subnetting occurs in the last octet.

---

### Q10. What is the difference between a network address and a broadcast address?

**Answer:**

The network address identifies the subnet itself, while the broadcast address is used to communicate with all hosts within that IPv4 subnet.

---

### Q11. Find the network and broadcast address for:

```text
192.168.10.145/28
```

**Answer:**

```text
Network:
192.168.10.144

Broadcast:
192.168.10.159
```

---

### Q12. Find the network and broadcast address for:

```text
192.168.50.140/27
```

**Answer:**

```text
Network:
192.168.50.128

Broadcast:
192.168.50.159
```

---

## Subnet Design Questions

### Q13. A department needs 50 usable host addresses. What is the smallest suitable subnet?

**Answer:**

`/26`.

A `/27` provides only:

```text
30 usable hosts
```

while `/26` provides:

```text
62 usable hosts
```

---

### Q14. A team needs 100 usable hosts. What prefix should you choose?

**Answer:**

`/25`.

A `/26` provides only 62 usable hosts, while `/25` provides 126 usable hosts.

---

### Q15. What subnet would you choose for 10 hosts?

**Answer:**

`/28`.

A `/28` provides:

```text
14 usable hosts
```

---

# VLSM Questions

### Q16. What is VLSM?

**Answer:**

VLSM (Variable Length Subnet Masking) allows different subnet sizes to be used within the same larger address space based on actual host requirements.

---

### Q17. Why is VLSM useful?

**Answer:**

VLSM reduces address waste by assigning each network a subnet size appropriate to its actual requirements.

---

### Q18. A company has:

```text
Engineering → 100 hosts
HR → 50 hosts
Finance → 20 hosts
```

What prefixes would you choose?

**Answer:**

```text
Engineering → /25
HR → /26
Finance → /27
```

---

### Q19. Why should larger subnets generally be allocated first in a VLSM design?

**Answer:**

Larger requirements need larger contiguous address blocks. Allocating them first reduces the chance of creating fragmented address space that cannot accommodate later large subnets.

---

## Cybersecurity Questions

### Q20. Does putting two systems in different subnets automatically isolate them?

**Answer:**

No.

Different subnets create different network boundaries, but routing and security controls determine whether communication between those networks is allowed.

---

### Q21. How does subnetting help a SOC analyst?

**Answer:**

Subnetting gives network context to IP addresses.

For example, if an alert originates from a Guest subnet and targets a Server subnet, the analyst can immediately recognize that the communication may be unusual and investigate further.

---

### Q22. Why should a GRC analyst understand subnetting?

**Answer:**

Subnetting is involved in network architecture, segmentation, security boundaries, and infrastructure changes. Understanding it helps a GRC analyst evaluate whether network design aligns with security requirements and controls.

---

### Q23. How does subnetting support firewall architecture?

**Answer:**

Subnets provide logical network boundaries around which firewall policies can be designed.

For example:

```text
Users
   ↓
Firewall
   ↓
Servers
```

The firewall can enforce policies between the networks.

---

## Advanced Scenario Questions

### Q24. An organization has:

```text
192.168.100.0/24
```

Requirements:

```text
Engineering → 100 hosts
HR → 50 hosts
Finance → 20 hosts
```

Design the subnet sizes.

**Answer:**

```text
Engineering → /25
HR → /26
Finance → /27
```

A possible allocation is:

```text
Engineering:
192.168.100.0/25

HR:
192.168.100.128/26

Finance:
192.168.100.192/27
```

---

### Q25. A security team discovers that Guest and Production devices share the same subnet. What security concerns might this raise?

**Answer:**

It may indicate insufficient network segmentation.

The team should investigate:

* Whether the architecture requires isolation.
* Whether VLANs or separate subnets should exist.
* What firewall or ACL controls are present.
* Whether guest traffic can reach production systems.
* Whether compensating controls exist.

---

# Interview Challenge

### Q26.

Explain this to a non-technical manager:

> Why does `192.168.10.0/24` become four smaller networks when divided into `/26`?

**Expected reasoning:**

A `/24` has eight host bits.

A `/26` uses two additional bits for the network portion, leaving six host bits.

Two borrowed bits create:

```text
2^2 = 4
```

smaller subnets.

Each subnet contains:

```text
2^6 = 64 total addresses
```

---

# Final Mental Model

```text
CIDR Prefix
     ↓
Network / Host Bits
     ↓
Subnet Size
     ↓
Network Boundaries
     ↓
Network Address
     ↓
Broadcast Address
     ↓
Usable Hosts
     ↓
VLSM / Network Design
     ↓
Segmentation & Security Architecture
```
