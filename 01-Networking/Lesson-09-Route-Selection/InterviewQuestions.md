# Lesson 09 - Route Selection & Longest Prefix Match

# Interview Questions

---

## Beginner Level

### Q1. What is route selection?

**Answer:**

Route selection is the process a router uses to determine which available route should be used to forward a packet toward its destination.

---

### Q2. What is a network prefix?

**Answer:**

A network prefix identifies the portion of an IP address that represents the network. The prefix length is written using CIDR notation such as `/8`, `/16`, `/24`, or `/25`.

---

### Q3. What does `/24` mean in IPv4?

**Answer:**

`/24` means that the first 24 bits of the 32-bit IPv4 address represent the network prefix, leaving 8 bits for the host portion.

The corresponding subnet mask is:

```text
255.255.255.0
```

---

### Q4. Which is more specific: `/16` or `/24`?

**Answer:**

`/24` is more specific because it contains a longer network prefix.

Generally:

```text
/8  → less specific
/16 → more specific
/24 → even more specific
```

---

### Q5. What is Longest Prefix Match?

**Answer:**

Longest Prefix Match is the process of selecting the most specific matching route for a destination IP address.

If multiple routes match the destination, the route with the longest matching prefix is preferred.

---

## Intermediate Level

### Q6. Why does a router need Longest Prefix Match?

**Answer:**

A router may have multiple routes that match the same destination IP address.

Longest Prefix Match allows the router to select the most specific matching route instead of choosing a broader route.

---

### Q7. Consider these routes:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Which route should be selected for:

```text
10.10.10.50
```

**Answer:**

```text
10.10.10.0/24
```

All three routes match the destination, but `/24` is the longest prefix and therefore the most specific match.

---

### Q8. What is the difference between route matching and route selection?

**Answer:**

**Route matching** determines whether a particular route contains the destination IP.

**Route selection** determines which route should be used when multiple routes match.

For example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

may all match a destination such as:

```text
10.10.10.50
```

The router then selects the `/24` route because it is the most specific match.

---

### Q9. What is the default route?

**Answer:**

The default route is a broad route used when a more specific route is not available.

For IPv4, it is represented as:

```text
0.0.0.0/0
```

---

### Q10. Why is `0.0.0.0/0` less specific than `/24`?

**Answer:**

`/0` contains zero network-prefix bits, while `/24` contains 24 network-prefix bits.

Therefore, `/24` provides much more specific information about the destination network.

---

### Q11. Can the default route match an IP address that also matches a more specific route?

**Answer:**

Yes.

For example, a destination can match both:

```text
10.10.10.0/24
```

and:

```text
0.0.0.0/0
```

The `/24` route is preferred because it is more specific.

---

## Scenario-Based Questions

### Q12. A router has the following routes:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
0.0.0.0/0
```

The destination is:

```text
10.10.10.50
```

Which route is selected?

**Answer:**

```text
10.10.10.0/24
```

The destination matches all four routes, but `/24` is the longest matching prefix.

---

### Q13. Consider:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
0.0.0.0/0
```

The destination is:

```text
10.10.20.10
```

Which route is selected?

**Answer:**

```text
10.10.0.0/16
```

The `/24` route does not match `10.10.20.10`.

The `/16` route is therefore the most specific matching route.

---

### Q14. Consider:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
0.0.0.0/0
```

The destination is:

```text
10.50.1.20
```

Which route is selected?

**Answer:**

```text
10.0.0.0/8
```

The `/16` and `/24` routes do not match the destination, while the `/8` route does.

---

### Q15. The routing table contains only:

```text
0.0.0.0/0
```

A packet arrives for:

```text
8.8.8.8
```

What happens?

**Answer:**

The default route can be used because there is no more specific route available.

---

## Advanced Level

### Q16. Explain Longest Prefix Match using a real-world analogy.

**Answer:**

Imagine delivery instructions that say:

```text
India
Delhi
South Delhi
A specific street
```

All of them provide information about the destination, but the specific street provides the most precise information.

Similarly, a `/24` route is more specific than a `/16` route, so when both match a destination, the `/24` route is preferred.

---

### Q17. Why is Longest Prefix Match important in real networks?

**Answer:**

It allows network administrators to define broad routes while also creating more specific routes for particular networks.

This gives routers a precise way to determine where traffic should be forwarded.

---

### Q18. How can route selection affect cybersecurity?

**Answer:**

Routing determines the path network traffic takes.

An unexpected routing decision could affect:

* Network segmentation
* Firewall enforcement
* Monitoring
* Security boundaries
* Access paths

Therefore, routing changes can have security implications.

---

### Q19. Why should a GRC analyst understand route selection?

**Answer:**

A GRC analyst may review network architecture, segmentation, security boundaries, and infrastructure changes.

Understanding route selection helps them determine whether a routing change could alter traffic paths or affect security controls.

---

### Q20. What is the difference between Longest Prefix Match and a routing metric?

**Answer:**

Longest Prefix Match determines which matching destination prefix is the most specific.

A routing metric is used by routing mechanisms or protocols to compare paths according to their own path-selection criteria.

They solve different parts of the routing decision process and should not be treated as the same concept.

---

# Interview Scenario

### Q21.

A company has the following architecture:

```text
User Network
     |
     v
  Firewall
     |
     v
Production Network
```

The company requires all user traffic to production to pass through the firewall.

A network administrator adds a more specific route that creates another path.

As a security professional, what would you investigate?

**Answer:**

I would investigate:

1. The new routing entry.
2. Which destination traffic matches the route.
3. The previous traffic path.
4. The new traffic path.
5. Whether the new path bypasses the firewall.
6. Whether the change was authorized.
7. Whether change-management procedures were followed.
8. Whether the organization's network-segmentation requirements are still satisfied.

---

# Think Before Answering

Don't memorize the phrase:

> "Longest prefix match means longest prefix wins."

Instead, explain the reasoning:

```text
Destination IP
      ↓
Find routes that actually match
      ↓
Compare matching prefix lengths
      ↓
Select the most specific match
      ↓
Forward traffic
```

That demonstrates actual understanding rather than memorization.
