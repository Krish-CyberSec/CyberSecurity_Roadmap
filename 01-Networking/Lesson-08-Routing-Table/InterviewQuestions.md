# Chapter 08 – Routing Table Interview Questions

## Beginner Level

### Q1. What is a Routing Table?

**Answer:**

A Routing Table is a collection of routes maintained by a router that helps it determine where packets should be forwarded based on their destination IP addresses.

---

### Q2. Why does a router need a Routing Table?

**Answer:**

A router needs a Routing Table to determine which route or interface should be used to forward a packet toward its destination.

---

### Q3. What information can a Routing Table contain?

**Answer:**

A routing entry can contain information such as:

* Destination Network
* Prefix / Subnet Mask
* Next Hop
* Exit Interface
* Route Type

---

### Q4. What is a Directly Connected Route?

**Answer:**

A Directly Connected Route represents a network that is directly attached to one of the router's active interfaces.

---

## Intermediate Level

### Q5. What is a Next Hop?

**Answer:**

The Next Hop is the next router or network device to which a packet should be forwarded on its journey toward the destination.

---

### Q6. What is an Exit Interface?

**Answer:**

The Exit Interface is the router interface through which the packet leaves the router.

---

### Q7. What is a Default Route?

**Answer:**

A Default Route is a catch-all route used when a more specific matching route is not available.

It is commonly represented as:

```text
0.0.0.0/0
```

---

### Q8. What does `0.0.0.0/0` mean?

**Answer:**

It represents the default IPv4 route and matches any IPv4 destination when a more specific route is not selected.

---

### Q9. How can you view the routing table on a Cisco router?

**Answer:**

Use:

```text
show ip route
```

---

### Q10. What is the difference between a CAM Table and a Routing Table?

**Answer:**

A CAM Table is primarily used by a Layer 2 switch to map MAC Addresses to switch ports.

A Routing Table is used by a Layer 3 router to determine how IP packets should be forwarded toward destination networks.

---

## Advanced Level

### Q11. A router receives a packet destined for 192.168.2.25. How does it use its Routing Table?

**Answer:**

The router examines the destination IP address and searches for matching routes in its Routing Table. If a suitable route exists, the router uses the corresponding forwarding information, such as the next hop or exit interface.

---

### Q12. What happens if a router has no route for a destination?

**Answer:**

If there is no matching route and no usable default route, the router cannot forward the packet and will generally discard it.

---

### Q13. Why is a Routing Table important for cybersecurity?

**Answer:**

Routing determines how traffic moves between networks. Unauthorized routing changes or route manipulation can redirect, disrupt, or expose network traffic, making routing information an important part of network security.

---

# Scenario-Based Questions

## Q14.

A company has these networks:

```text
HR          10.10.1.0/24
Finance     10.10.2.0/24
Engineering 10.10.3.0/24
```

A router connects all three networks.

A packet arrives with destination:

```text
10.10.2.50
```

What should the router do?

**Answer:**

The router should find the route matching `10.10.2.0/24` and forward the packet using the associated forwarding information.

---

## Q15.

A router receives a packet for:

```text
8.8.8.8
```

It has no specific route for `8.8.8.8`, but it has:

```text
0.0.0.0/0
```

What happens?

**Answer:**

The router can use the default route to forward the packet toward its upstream router.

---

## Q16.

A network administrator notices that traffic is unexpectedly being sent through a different router.

What should they investigate?

**Answer:**

They should investigate routing configuration and routing information for unexpected route changes, incorrect next hops, or other routing anomalies.

---

# Think Like a Security Professional

A GRC analyst is reviewing an organization's network architecture.

They see several routers connecting:

* Production
* Finance
* Development
* Guest
* Data Center

Why should the GRC analyst care about routing?

Think about:

* Network segmentation
* Security boundaries
* Access between networks
* Change management
* Critical infrastructure
* Unauthorized configuration changes
