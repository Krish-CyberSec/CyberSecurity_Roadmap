# Lesson 10 - Administrative Distance & Routing Metrics

# Interview Questions

---

## Beginner Level

### Q1. What is Administrative Distance?

**Answer:**

Administrative Distance (AD) is a value used by Cisco-style routing systems to indicate the relative trustworthiness or preference of the source of a route.

A lower Administrative Distance is preferred.

---

### Q2. What does a lower Administrative Distance mean?

**Answer:**

A lower Administrative Distance means that the routing source is generally preferred over a source with a higher Administrative Distance.

For example:

```text
Static Route → AD 1
OSPF         → AD 110
```

The static route is preferred based on Administrative Distance.

---

### Q3. What is the Administrative Distance of a directly connected route?

**Answer:**

The default Administrative Distance for a directly connected route is:

```text
0
```

---

### Q4. What is the default Administrative Distance of a static route?

**Answer:**

The default Administrative Distance of a static route is:

```text
1
```

---

### Q5. What is the default Administrative Distance of OSPF?

**Answer:**

The default Administrative Distance of OSPF in Cisco-style routing is:

```text
110
```

---

### Q6. What is a routing metric?

**Answer:**

A routing metric is a value used by a routing protocol or routing mechanism to compare available paths according to that protocol's path-selection rules.

Depending on the protocol, metrics can involve factors such as cost, bandwidth, delay, hop count, or other protocol-specific information.

---

## Intermediate Level

### Q7. What is the difference between Administrative Distance and a routing metric?

**Answer:**

They answer different questions.

**Administrative Distance:**

> Which routing source should be preferred?

**Routing Metric:**

> Which path does a particular routing protocol prefer?

For example:

```text
Static → AD 1
OSPF   → AD 110
```

AD can determine which routing source is preferred.

If two paths are both learned through OSPF, OSPF's metric can be used to compare those paths.

---

### Q8. Why does a router need Administrative Distance?

**Answer:**

A router can learn the same destination network from multiple routing sources.

For example:

```text
192.168.50.0/24

Static Route
OSPF
```

Administrative Distance provides a way to prefer one routing source over another.

---

### Q9. Consider:

```text
Destination: 192.168.50.0/24

Static → AD 1
OSPF   → AD 110
```

Which route source is preferred?

**Answer:**

The static route is preferred because:

```text
1 < 110
```

Lower Administrative Distance is preferred.

---

### Q10. What is the difference between Longest Prefix Match and Administrative Distance?

**Answer:**

Longest Prefix Match deals with **destination-prefix specificity**.

Administrative Distance deals with **preference between routing sources**.

For example:

```text
10.0.0.0/8
10.10.0.0/16
```

If both match a destination, the `/16` is more specific.

That is a Longest Prefix Match concept.

If the same destination prefix is learned through:

```text
Static → AD 1
OSPF   → AD 110
```

Administrative Distance can determine which routing source is preferred.

---

### Q11. Why can't we simply say "static routes always win"?

**Answer:**

Because routing decisions involve more than just Administrative Distance.

A route must first be considered as a match for the destination, and route-selection behavior depends on the specific routing situation.

For example, a more-specific prefix can matter before comparing routes from different sources.

Therefore, saying "static routes always win" is an oversimplification.

---

### Q12. What is OSPF's metric?

**Answer:**

OSPF uses a **cost-based metric** to compare paths.

For example:

```text
OSPF Path A → Cost 10
OSPF Path B → Cost 30
```

OSPF prefers the path with the lower cost.

---

### Q13. If two OSPF routes have the same destination, what can be used to choose between them?

**Answer:**

Because both routes come from OSPF, their Administrative Distance is the same under the normal defaults.

OSPF can therefore use its routing metric, which is cost, to compare the paths.

---

## Scenario-Based Questions

### Q14.

A router has:

```text
192.168.50.0/24
```

learned from:

```text
Static → AD 1
OSPF   → AD 110
```

Which source is preferred?

**Answer:**

The static route is preferred because it has the lower Administrative Distance.

```text
1 < 110
```

---

### Q15.

A router has these routes:

```text
10.0.0.0/8
10.10.0.0/16
```

The destination is:

```text
10.10.20.50
```

Which route is more specific?

**Answer:**

```text
10.10.0.0/16
```

The `/16` route is more specific than `/8`.

This is a Longest Prefix Match decision, not an Administrative Distance decision.

---

### Q16.

A router has:

```text
10.10.10.0/24
```

learned from:

```text
Static
OSPF
```

Which concept helps determine which source is preferred?

**Answer:**

Administrative Distance.

Both routes have the same destination prefix, so prefix specificity does not distinguish them.

---

### Q17.

Two OSPF paths exist:

```text
Path A → Cost 10
Path B → Cost 50
```

Which path would OSPF prefer?

**Answer:**

Path A.

OSPF uses a cost-based metric, and the lower cost is preferred.

```text
10 < 50
```

---

## Advanced Level

### Q18. Explain the relationship between Longest Prefix Match, Administrative Distance, and routing metrics.

**Answer:**

These concepts solve different parts of route selection.

```text
Longest Prefix Match
        ↓
Which destination prefix is
the most specific match?

Administrative Distance
        ↓
Which routing source is
preferred?

Routing Metric
        ↓
Which path does the routing
protocol prefer?
```

They should not be treated as interchangeable concepts.

---

### Q19. Consider these routes:

```text
10.0.0.0/8      Static
10.10.0.0/16    OSPF
```

The destination is:

```text
10.10.10.50
```

Which route is selected?

**Answer:**

The `/16` route is the more specific matching prefix.

Therefore:

```text
10.10.0.0/16
```

is the more specific route.

The important point is that you should not immediately say:

> "Static has AD 1, therefore static wins."

The prefix specificity must be considered.

---

### Q20. Why is Administrative Distance important for network reliability?

**Answer:**

It allows a router to prefer one source of routing information over another.

For example, an organization might use a dynamic routing protocol as the primary source and configure another route source as a backup.

Administrative Distance can help determine which source is preferred when the relevant routes compete.

---

### Q21. Why are routing metrics protocol-specific?

**Answer:**

Different routing protocols use different methods to evaluate paths.

One protocol might consider hop count, while another might use cost based on factors such as bandwidth.

Therefore, there is no single universal metric calculation for all routing protocols.

---

# Cybersecurity & GRC Questions

### Q22. Why should a cybersecurity professional understand Administrative Distance?

**Answer:**

Routing decisions determine how traffic moves through a network.

Understanding Administrative Distance helps security professionals understand why one routing source may be preferred over another and how routing changes can affect traffic paths.

---

### Q23. How can routing changes affect security controls?

**Answer:**

A routing change can alter the path traffic takes through the network.

This could affect:

* Firewall enforcement
* Network segmentation
* Monitoring
* Security boundaries
* Traffic inspection

Therefore, routing changes can have security implications.

---

### Q24. Why should a GRC analyst care about routing metrics?

**Answer:**

A GRC analyst may review infrastructure changes, network architecture, segmentation, and security controls.

Understanding routing metrics helps them ask whether a routing change could alter the intended traffic path or security architecture.

---

### Q25. A company requires all traffic from a user network to pass through a firewall before reaching a production network. A new route is introduced.

What should a GRC analyst ask?

**Answer:**

The analyst should ask:

1. What route was introduced?
2. Which destinations does it match?
3. Is it more specific than the existing route?
4. Which routing source provides it?
5. What is its Administrative Distance?
6. Could the traffic path change?
7. Does traffic still pass through the firewall?
8. Was the change authorized?
9. Was it documented through change management?
10. Does the new configuration still satisfy the organization's security requirements?

---

# Interview Trap Questions

## Q26. Is a lower prefix length always better?

**Answer:**

No.

For Longest Prefix Match, a **longer prefix** is more specific.

For example:

```text
/24 > /16
```

in terms of specificity.

---

## Q27. Is a lower Administrative Distance better?

**Answer:**

Yes.

For Administrative Distance:

```text
Lower AD = more preferred
```

For example:

```text
AD 1 > preferred than > AD 110
```

---

## Q28. Is a lower routing metric always better?

**Answer:**

Not universally.

The meaning and comparison of metrics depend on the routing protocol.

For example, OSPF uses cost, where lower cost is preferred, but different routing protocols use different metric systems.

---

## Q29. Are Administrative Distance and routing metrics the same thing?

**Answer:**

No.

Administrative Distance compares the preference of routing sources.

A routing metric is used by a routing protocol to compare paths according to that protocol's rules.

---

# Final Interview Challenge

Consider:

```text
Destination: 192.168.100.0/24
```

The router knows:

```text
Route A:
Static
AD = 1

Route B:
OSPF
AD = 110
```

At the same time, the routing table contains:

```text
192.168.0.0/16
```

and:

```text
192.168.100.0/24
```

### Question

Explain how you would reason about the route selection.

### Expected thinking

Do not jump directly to:

```text
Static → AD 1
```

Instead think:

```text
Destination
     ↓
Which prefixes match?
     ↓
Which prefix is most specific?
     ↓
Are there competing routes for
that same destination prefix?
     ↓
If yes, compare routing sources.
     ↓
Administrative Distance
     ↓
If same routing source,
protocol metric may matter
```

The goal is to understand the **decision process**, not memorize isolated numbers.

---

# Lesson 10 Interview Checklist

* [ ] I can define Administrative Distance.
* [ ] I know that lower AD is preferred.
* [ ] I know common Cisco default AD values.
* [ ] I understand what a routing metric is.
* [ ] I understand OSPF's cost concept.
* [ ] I can distinguish AD from metrics.
* [ ] I can distinguish AD from Longest Prefix Match.
* [ ] I can solve basic route-selection scenarios.
* [ ] I can explain the concepts from a cybersecurity perspective.
* [ ] I can explain why GRC professionals should understand them.
