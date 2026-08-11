# Lesson 09 - Route Selection & Longest Prefix Match

## Module Information

| Field         | Details                                |
| ------------- | -------------------------------------- |
| Module        | Networking for Cybersecurity           |
| Lesson        | 09                                     |
| Topic         | Route Selection & Longest Prefix Match |
| Difficulty    | Beginner → Intermediate                |
| Prerequisites | Lessons 01–08                          |
| OSI Layer     | Layer 3 – Network                      |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Understand why routers need route-selection rules.
* Understand what a network prefix represents.
* Understand the relationship between prefix length and specificity.
* Explain Longest Prefix Match.
* Determine whether a route matches a destination IP.
* Identify the most specific matching route.
* Understand the role of the default route.
* Distinguish route matching from route selection.
* Understand why routing decisions matter in cybersecurity and GRC.

---

# 1. The Problem

In the previous lesson, we learned that a router uses a **Routing Table** to determine where packets should be forwarded.

But what happens when **multiple routes match the same destination IP address?**

Consider:

```text
10.0.0.0/8

10.10.0.0/16

10.10.10.0/24
```

Now a packet arrives with:

```text
Destination IP:

10.10.10.50
```

The destination belongs to all three networks.

Therefore, the router needs a rule to determine which route is the most appropriate.

---

# 2. Understanding Prefixes

IPv4 addresses contain:

```text
32 bits
```

The number after `/` represents the number of bits used for the network prefix.

For example:

```text
/8
```

means:

```text
8 network bits
24 host bits
```

While:

```text
/16
```

means:

```text
16 network bits
16 host bits
```

And:

```text
/24
```

means:

```text
24 network bits
8 host bits
```

Therefore:

```text
/8   → Broad
/16  → More specific
/24  → More specific
/25  → Even more specific
```

A larger prefix length represents a more specific network.

---

# 3. Prefix and Subnet Mask

The prefix length corresponds to a subnet mask.

Examples:

| Prefix | Subnet Mask       |
| ------ | ----------------- |
| `/8`   | `255.0.0.0`       |
| `/16`  | `255.255.0.0`     |
| `/24`  | `255.255.255.0`   |
| `/25`  | `255.255.255.128` |

Detailed subnetting and binary calculations will be covered separately.

For this lesson, the important concept is:

> **A longer prefix represents a more specific network.**

---

# 4. Broad vs Specific Networks

Consider:

```text
10.0.0.0/8
```

This represents a broad network.

Compare it with:

```text
10.10.10.0/24
```

This describes a much more specific network.

Conceptually:

```text
10.0.0.0/8
      ↓
10.10.0.0/16
      ↓
10.10.10.0/24
```

As the prefix becomes longer, the network becomes more specific.

---

# 5. What is Longest Prefix Match?

> **Longest Prefix Match is the process of selecting the most specific matching route for a destination IP address.**

Consider:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Destination:

```text
10.10.10.50
```

All three routes match.

The prefix lengths are:

```text
/8
/16
/24
```

The longest prefix is:

```text
/24
```

Therefore:

```text
10.10.10.0/24
```

is selected.

---

# 6. Step-by-Step Example

Routing table:

| Destination  | Prefix | Next Hop |
| ------------ | -----: | -------- |
| `10.0.0.0`   |   `/8` | Router A |
| `10.10.0.0`  |  `/16` | Router B |
| `10.10.10.0` |  `/24` | Router C |

Destination:

```text
10.10.10.50
```

### Step 1 — Check `/8`

```text
10.0.0.0/8
```

The destination matches.

### Step 2 — Check `/16`

```text
10.10.0.0/16
```

The destination also matches.

### Step 3 — Check `/24`

```text
10.10.10.0/24
```

The destination also matches.

### Step 4 — Compare Prefix Lengths

```text
/8
/16
/24
```

The longest prefix is:

```text
/24
```

Therefore:

```text
10.10.10.0/24
```

is the selected route.

---

# 7. Another Example

Consider:

| Destination  | Prefix |
| ------------ | -----: |
| `10.0.0.0`   |   `/8` |
| `10.10.0.0`  |  `/16` |
| `10.10.10.0` |  `/24` |
| `0.0.0.0`    |   `/0` |

Destination:

```text
10.10.20.10
```

Does:

```text
10.10.10.0/24
```

match?

No.

But:

```text
10.10.0.0/16
```

does match.

Therefore, the `/16` route becomes the most specific matching route.

This is important:

> **The router doesn't simply choose the largest prefix. The prefix must first actually match the destination.**

---

# 8. The Default Route

We previously learned about:

```text
0.0.0.0/0
```

This is the IPv4 **Default Route**.

It is extremely broad and can match any IPv4 destination.

However, it is less specific than routes such as:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Therefore:

```text
/24 > /16 > /8 > /0
```

when comparing specificity.

The default route acts as a fallback when a more specific matching route isn't available.

---

# 9. Route Matching vs Route Selection

These are two different concepts.

## Route Matching

The question is:

> **Does this route contain the destination IP?**

Example:

```text
Route:

10.10.0.0/16

Destination:

10.10.10.50
```

Yes, it matches.

---

## Route Selection

The question is:

> **Several routes match. Which one should the router use?**

Example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

If all three match:

```text
10.10.10.0/24
```

is selected because it has the longest matching prefix.

---

# 10. A More Detailed Example

Consider:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
10.10.10.128/25
0.0.0.0/0
```

Destination:

```text
10.10.10.150
```

The destination matches:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
10.10.10.128/25
0.0.0.0/0
```

The prefix lengths are:

```text
/8
/16
/24
/25
/0
```

The longest matching prefix is:

```text
/25
```

Therefore:

```text
10.10.10.128/25
```

is selected.

---

# 11. Why `/25` Can Be More Specific Than `/24`

A `/24` network:

```text
10.10.10.0/24
```

can be divided into two `/25` networks:

```text
10.10.10.0/25
```

and:

```text
10.10.10.128/25
```

The ranges are:

```text
10.10.10.0   – 10.10.10.127
10.10.10.128 – 10.10.10.255
```

Therefore:

```text
10.10.10.150
```

belongs to:

```text
10.10.10.128/25
```

This is an early example of why understanding subnetting will become important.

---

# 12. Router Decision Process

A simplified mental model is:

```text
Packet Arrives
      │
      ▼
Read Destination IP
      │
      ▼
Search Routing Table
      │
      ▼
Find Matching Routes
      │
      ▼
Compare Prefix Lengths
      │
      ▼
Select Longest Matching Prefix
      │
      ▼
Forward Packet
```

This is a conceptual model. Real routing decisions can involve additional factors such as route preference and metrics.

---

# 13. Don't Confuse Prefix Matching With Metrics

This distinction is important.

### Longest Prefix Match

Answers:

> **Which destination prefix is the most specific match?**

### Administrative Distance

Helps determine:

> **Which routing source should be preferred?**

### Metric

Helps determine:

> **Which path is preferred within a routing protocol or routing mechanism?**

These concepts will be studied separately.

For now, focus on understanding Longest Prefix Match.

---

# 14. Why Route Selection Matters

Routing determines how network traffic moves.

For example:

```text
User Network
      │
      ▼
Firewall
      │
      ▼
Production Network
```

An organization may expect traffic to follow this path.

A routing change could potentially create another path:

```text
User Network
      │
      ▼
Router
      │
      ▼
Production Network
```

That could affect:

* Security controls
* Network segmentation
* Firewall enforcement
* Monitoring
* Traffic visibility
* Access paths

Therefore, routing decisions are also security-relevant.

---

# 15. Cybersecurity Perspective

Security professionals should understand routing because attackers and defenders both care about traffic paths.

Routing can affect:

* Where traffic is inspected.
* Where firewalls are placed.
* Which networks can communicate.
* How network segmentation is enforced.
* Where monitoring systems see traffic.

Unexpected routing behavior can therefore become an investigation clue.

---

# 16. GRC Perspective

A GRC professional may encounter routing while reviewing:

* Network architecture
* Network segmentation
* Security boundaries
* Change requests
* Infrastructure configurations
* Access paths
* Security control placement

A GRC analyst does not necessarily need to configure routers, but understanding route selection helps them ask better questions.

For example:

> "Does this routing change alter the intended security boundary?"

or:

> "Will this traffic still pass through the required security control?"

---

# 17. OSI Model Mapping

| OSI Layer               | Topics                                                                       |
| ----------------------- | ---------------------------------------------------------------------------- |
| Layer 7 – Application   | Browser / Applications                                                       |
| Layer 6 – Presentation  | Not studied yet                                                              |
| Layer 5 – Session       | Not studied yet                                                              |
| Layer 4 – Transport     | Not studied yet                                                              |
| **Layer 3 – Network**   | **IP Address, Router, Routing Table, Route Selection, Longest Prefix Match** |
| **Layer 2 – Data Link** | **MAC Address, ARP, Switch**                                                 |
| Layer 1 – Physical      | Ethernet / Wi-Fi                                                             |

Lesson 9 continues our **Layer 3** journey.

---

# 18. Key Takeaways

* A router can have multiple routes that match a destination.
* The router needs a rule to select the appropriate route.
* A prefix length represents the size of the network prefix.
* A larger prefix length means a more specific network.
* Longest Prefix Match selects the most specific matching route.
* `/24` is more specific than `/16`.
* `/16` is more specific than `/8`.
* `/0` represents the default route and is the least specific.
* A route must first match the destination before it can be selected.
* Route selection affects real network traffic and therefore security architecture.

---

# Lesson 9 Checkpoint

Before considering this lesson complete, you should be able to explain:

* What a network prefix is.
* What `/8`, `/16`, `/24`, and `/25` mean at a high level.
* What Longest Prefix Match means.
* The difference between route matching and route selection.
* Why a `/24` route can beat a `/16` route.
* Why the default route acts as a fallback.
* Why routing decisions matter to cybersecurity.
* Why routing knowledge is useful in GRC.

---

# What's Next?

We now know **which destination prefix is the most specific match**.

But we haven't answered another important question:

> **What happens when multiple routes have the same destination prefix?**

That takes us into:

**Administrative Distance, Route Preference, and Metrics.**
