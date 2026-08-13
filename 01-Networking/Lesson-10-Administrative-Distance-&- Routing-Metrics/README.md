# Lesson 10 - Administrative Distance & Routing Metrics

## Module Information

| Field         | Details                                   |
| ------------- | ----------------------------------------- |
| Module        | Networking for Cybersecurity              |
| Lesson        | 10                                        |
| Topic         | Administrative Distance & Routing Metrics |
| Difficulty    | Beginner → Intermediate                   |
| Prerequisites | Lessons 01–09                             |
| OSI Layer     | Layer 3 – Network                         |

---

# Learning Objectives

After completing this lesson, you should be able to:

* Explain why routers may have multiple routes to the same destination.
* Understand Administrative Distance (AD).
* Understand why a lower Administrative Distance is preferred.
* Understand what a routing metric is.
* Distinguish Administrative Distance from a routing metric.
* Understand how Longest Prefix Match relates to route selection.
* Understand how different routing sources can provide the same destination.
* Understand why these concepts matter to cybersecurity and GRC.

---

# 1. Why Do We Need Administrative Distance?

A router can learn routes from different sources.

For example, a router may learn:

```text
192.168.50.0/24
```

from:

```text
Static Route
```

and also:

```text
192.168.50.0/24
```

from:

```text
OSPF
```

Both routes describe the same destination network.

The router therefore needs a way to determine which source of routing information it should prefer.

This is where **Administrative Distance** is used.

---

# 2. What is Administrative Distance?

Administrative Distance (AD) is a value used by Cisco-style routing systems to indicate the relative trustworthiness of the source of a route.

For the common Cisco defaults:

| Route Source       | Administrative Distance |
| ------------------ | ----------------------: |
| Directly Connected |                       0 |
| Static             |                       1 |
| EIGRP              |                      90 |
| OSPF               |                     110 |
| RIP                |                     120 |

The general rule is:

> **Lower Administrative Distance is preferred.**

For example:

```text
Static Route
AD = 1

OSPF
AD = 110
```

If both provide the same destination prefix, the static route is preferred based on Administrative Distance.

---

# 3. Why Does the Router Need AD?

Imagine the router receives routing information from different sources.

Conceptually:

```text
              ROUTER
                 |
       ┌─────────┼─────────┐
       │         │         │
       ▼         ▼         ▼
    Static     OSPF      RIP
```

These sources may provide information about the same destination.

The router needs to determine which source should be preferred.

Administrative Distance provides a preference value for the routing source.

---

# 4. Administrative Distance Example

Suppose:

```text
Destination:

192.168.50.0/24
```

is learned through:

```text
Static Route → AD 1
OSPF         → AD 110
```

The router prefers:

```text
Static Route
```

because:

```text
1 < 110
```

Therefore:

> Lower Administrative Distance means the route source is preferred.

---

# 5. What is a Routing Metric?

A **routing metric** is a value used by a routing protocol or routing mechanism to compare available paths according to that protocol's path-selection rules.

Depending on the protocol, a metric can be based on factors such as:

* Hop count
* Bandwidth
* Delay
* Cost
* Other protocol-specific information

Different routing protocols use different metric systems.

---

# 6. Administrative Distance vs Metric

These concepts should not be confused.

### Administrative Distance

Answers:

> **Which routing source should I prefer?**

### Metric

Answers:

> **Which path does this routing protocol prefer?**

For example:

```text
                 ROUTER
                   |
          ┌────────┴────────┐
          │                 │
       Static              OSPF
       AD = 1             AD = 110
```

If both provide the same destination prefix, Administrative Distance can determine which routing source is preferred.

Now imagine the router has two OSPF paths:

```text
OSPF Path A → Cost 10
OSPF Path B → Cost 30
```

Both paths come from OSPF, so the routing protocol's metric is used to compare them.

---

# 7. Common Example: OSPF

OSPF uses a cost-based metric.

Conceptually:

```text
OSPF Path A → Cost 10
OSPF Path B → Cost 30
```

OSPF prefers:

```text
Path A
```

because it has the lower cost.

The important point is that **OSPF's metric is different from Administrative Distance**.

---

# 8. Relationship With Longest Prefix Match

Lesson 9 introduced:

> **Longest Prefix Match**

This determines the most specific destination prefix that matches the packet.

For example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

For:

```text
10.10.10.50
```

the `/24` route is the most specific matching prefix.

Administrative Distance is a different concept.

It becomes relevant when the router has competing routes from different routing sources for the same destination prefix.

---

# 9. Conceptual Decision Model

A useful simplified model is:

```text
Packet
  |
  ▼
Destination IP
  |
  ▼
Find matching routes
  |
  ▼
Longest Prefix Match
  |
  ▼
If competing routes remain,
consider route preference
  |
  ▼
Administrative Distance
  |
  ▼
Routing protocol metric
  |
  ▼
Forwarding decision
```

This is a conceptual learning model rather than a complete specification of every routing platform.

---

# 10. Why You Should Not Mix These Concepts

Consider:

```text
10.0.0.0/8
```

and:

```text
10.10.0.0/16
```

If both match the destination:

```text
10.10.10.50
```

the `/16` is more specific.

You should not say:

> "The `/8` route has a better AD."

That is mixing two different concepts.

The first question is:

> **Which destination prefix matches most specifically?**

That is the Longest Prefix Match concept.

---

# 11. Same Prefix, Different Sources

Now consider:

```text
Destination:

192.168.50.0/24
```

Two sources provide it:

```text
Static → AD 1
OSPF   → AD 110
```

Now the destination prefix is the same:

```text
/24
```

So prefix specificity doesn't distinguish them.

Administrative Distance can now determine which routing source is preferred.

---

# 12. Same Source, Multiple Paths

Suppose OSPF provides:

```text
192.168.50.0/24
```

through two paths:

```text
Path A → Cost 10
Path B → Cost 30
```

Both paths come from OSPF.

Therefore, the routing protocol's metric becomes relevant.

OSPF prefers the lower-cost path.

---

# 13. Key Difference

Remember this:

```text
Longest Prefix Match
        ↓
Which destination network is
the most specific match?

Administrative Distance
        ↓
Which routing source is
preferred?

Metric
        ↓
Which path does the routing
protocol prefer?
```

These answer different questions.

---

# 14. Security Perspective

Routing decisions determine how traffic moves through an organization.

Unexpected routing behavior can affect:

* Network segmentation
* Firewall placement
* Monitoring
* Security boundaries
* Access paths
* Availability

For example:

```text
User Network
      |
      ▼
Firewall
      |
      ▼
Production Network
```

If a routing change creates an unexpected path, traffic may no longer follow the intended security architecture.

Understanding AD and metrics helps security professionals understand why a particular path may have been selected.

---

# 15. GRC Perspective

A GRC professional may encounter routing decisions during:

* Network architecture reviews
* Infrastructure change reviews
* Security assessments
* Network segmentation reviews
* Change management
* Control assessments

Useful questions include:

> What is the primary route?

> What is the backup route?

> What routing source provides each route?

> Could the routing change alter the security path?

> Does the alternate path still satisfy security requirements?

> Was the change authorized and documented?

---

# 16. OSI Model Mapping

| OSI Layer               | Topics                                                                                                         |
| ----------------------- | -------------------------------------------------------------------------------------------------------------- |
| Layer 7 – Application   | Browser / Applications                                                                                         |
| Layer 6 – Presentation  | Not studied yet                                                                                                |
| Layer 5 – Session       | Not studied yet                                                                                                |
| Layer 4 – Transport     | Not studied yet                                                                                                |
| **Layer 3 – Network**   | **IP, Router, Routing Table, Route Selection, Longest Prefix Match, Administrative Distance, Routing Metrics** |
| **Layer 2 – Data Link** | **MAC, ARP, Switch**                                                                                           |
| Layer 1 – Physical      | Ethernet / Wi-Fi                                                                                               |

Lesson 10 continues the Layer 3 portion of the networking foundation.

---

# 17. Key Takeaways

* A router can learn routes from different sources.
* Administrative Distance represents the relative preference of a route source.
* Lower Administrative Distance is preferred.
* A routing metric helps a routing protocol compare paths.
* Administrative Distance and metrics are not the same thing.
* Longest Prefix Match deals with destination-prefix specificity.
* Administrative Distance deals with preference between routing sources.
* Routing metrics help a routing protocol compare paths.
* These concepts affect network traffic and therefore can affect security architecture.

---

# Lesson 10 Checkpoint

Before moving forward, you should be able to explain:

* What Administrative Distance is.
* Why lower AD is preferred.
* What a routing metric is.
* The difference between AD and a metric.
* The difference between Longest Prefix Match and AD.
* Why two routes can exist for the same destination.
* Why OSPF needs a metric.
* Why these concepts matter to cybersecurity.
* Why GRC professionals should understand routing decisions.

---

# Next Step

We now have the core concepts required to understand basic route selection:

```text
Routing Table
      ↓
Longest Prefix Match
      ↓
Administrative Distance
      ↓
Routing Metrics
```

The next major networking concept is **Subnetting & CIDR**, where we will go much deeper into how prefixes such as `/24`, `/25`, `/26`, etc. actually divide networks.
