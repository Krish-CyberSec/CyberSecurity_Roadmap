# Lesson 09 - Route Selection & Longest Prefix Match

# Labs

## Lab 01 — Read the Routing Table

### Objective

Understand how to inspect a router's routing table and identify the networks it knows about.

### Topology

Use the same topology from Lesson 07:

```text
PC1 ─── Switch1 ─── Router ─── Switch2 ─── PC3
                         │
                         │
                  Two Networks
```

### Networks

```text
Network 1:

192.168.1.0/24

Router:

192.168.1.1
```

```text
Network 2:

192.168.2.0/24

Router:

192.168.2.1
```

### Step 1 — Open Router CLI

Run:

```text
show ip route
```

### Step 2 — Observe

Find the entries related to:

```text
192.168.1.0/24
192.168.2.0/24
```

### Questions

1. Which interface is associated with `192.168.1.0/24`?
2. Which interface is associated with `192.168.2.0/24`?
3. Are these networks directly connected?
4. Why doesn't the router need a separate static route for these networks?

---

# Lab 02 — Basic Route Matching

## Objective

Practice determining whether a destination IP belongs to a route.

Consider:

```text
Route:

10.10.0.0/16
```

Test the following destinations:

```text
10.10.5.20
10.10.50.100
10.20.5.10
```

Create this table:

| Destination    | Does it match `10.10.0.0/16`? | Why? |
| -------------- | ----------------------------- | ---- |
| `10.10.5.20`   |                               |      |
| `10.10.50.100` |                               |      |
| `10.20.5.10`   |                               |      |

### Goal

Don't simply look at the answer.

Try to understand **why** the destination belongs or does not belong to the network.

---

# Lab 03 — Longest Prefix Match

## Objective

Practice selecting the most specific route.

Given:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

Destination:

```text
10.10.10.50
```

### Your Task

Determine:

```text
1. Which routes match?
2. What are their prefix lengths?
3. Which prefix is the longest?
4. Which route is selected?
```

Write your answer **before checking anything**.

### Expected reasoning format

```text
Destination:
10.10.10.50

Matching routes:
________________

Prefix lengths:
________________

Longest prefix:
________________

Selected route:
________________
```

---

# Lab 04 — Multiple Destinations

## Objective

Practice route selection with different destination addresses.

Use this routing table:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
10.10.10.128/25
0.0.0.0/0
```

Determine the selected route for each destination.

| Destination    | Matching Route(s) | Selected Route | Why? |
| -------------- | ----------------- | -------------- | ---- |
| `10.10.10.50`  |                   |                |      |
| `10.10.10.150` |                   |                |      |
| `10.10.20.10`  |                   |                |      |
| `10.50.1.20`   |                   |                |      |
| `8.8.8.8`      |                   |                |      |

### Important

For every answer, follow this process:

```text
Destination
     ↓
Find matching routes
     ↓
Compare prefix lengths
     ↓
Select longest matching prefix
```

---

# Lab 05 — Understand `/24` vs `/25`

## Objective

Understand why a `/25` route can be more specific than a `/24` route.

Consider:

```text
10.10.10.0/24
```

This network can be divided into:

```text
10.10.10.0/25
10.10.10.128/25
```

The ranges are:

```text
10.10.10.0/25

10.10.10.0
        ↓
10.10.10.127
```

and:

```text
10.10.10.128/25

10.10.10.128
        ↓
10.10.10.255
```

### Questions

Which `/25` contains each address?

```text
10.10.10.20
10.10.10.100
10.10.10.128
10.10.10.150
10.10.10.200
```

Complete:

| IP Address     | `/25` Network |
| -------------- | ------------- |
| `10.10.10.20`  |               |
| `10.10.10.100` |               |
| `10.10.10.128` |               |
| `10.10.10.150` |               |
| `10.10.10.200` |               |

---

# Lab 06 — Default Route

## Objective

Understand how the default route acts as a fallback.

Routing table:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
0.0.0.0/0
```

Test these destinations:

```text
10.10.10.50
10.10.20.50
10.50.1.20
8.8.8.8
```

### Questions

For each destination:

1. Is there a more specific matching route?
2. If yes, which one?
3. If not, does the default route get used?

Create:

| Destination   | Specific Route? | Selected Route |
| ------------- | --------------- | -------------- |
| `10.10.10.50` |                 |                |
| `10.10.20.50` |                 |                |
| `10.50.1.20`  |                 |                |
| `8.8.8.8`     |                 |                |

---

# Lab 07 — Packet Tracer Routing Table

## Objective

Connect the theory to the router you already built.

Use the two-network topology from previous lessons.

### Router

```text
G0/0

192.168.1.1/24
```

```text
G0/1

192.168.2.1/24
```

### Step 1

Open the router CLI.

Run:

```text
show ip route
```

### Step 2

Find the routes for:

```text
192.168.1.0/24
192.168.2.0/24
```

### Step 3

Record them:

| Network       | Prefix | Interface | Route Type |
| ------------- | ------ | --------- | ---------- |
| `192.168.1.0` |        |           |            |
| `192.168.2.0` |        |           |            |

### Step 4

Now ask yourself:

> If the router receives a packet destined for `192.168.2.20`, which route should it use?

Write your prediction before testing.

---

# Lab 08 — Simulation Mode

## Objective

Observe how routing affects packet movement.

Switch Packet Tracer from:

```text
Realtime
```

to:

```text
Simulation
```

Send traffic from:

```text
PC1
192.168.1.10
```

to:

```text
PC3
192.168.2.10
```

### Observe

Pay attention to:

```text
PC1
 ↓
Switch1
 ↓
Router
 ↓
Switch2
 ↓
PC3
```

### Questions

1. What is the destination IP of the packet?
2. Which router interface receives it?
3. Which route does the router use?
4. Which interface does the packet leave from?
5. Why does the router need Layer 3 information to make this decision?

---

# Lab 09 — Troubleshooting Challenge

## Scenario

Your organization has:

```text
User Network
     |
     v
  Firewall
     |
     v
Production Network
```

The security policy requires traffic from the User Network to Production to pass through the firewall.

A network administrator adds a new, more-specific route.

After the change, the security team notices unexpected traffic behavior.

### Your Task

Think like a security analyst.

Investigate:

```text
1. What route was added?
2. What destination network does it match?
3. Is it more specific than the previous route?
4. Which traffic will now use it?
5. Did the traffic path change?
6. Does traffic still pass through the firewall?
7. Was the routing change authorized?
```

### Security Questions

* Could the change affect network segmentation?
* Could it bypass a security control?
* What evidence would you request?
* Would this change require change-management approval?

---

# Lab 10 — Interview Challenge

Do this without looking at your notes.

Given:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
10.10.10.128/25
0.0.0.0/0
```

Determine the selected route for:

### A

```text
10.10.10.50
```

### B

```text
10.10.10.150
```

### C

```text
10.10.20.10
```

### D

```text
10.50.1.20
```

### E

```text
8.8.8.8
```

For every answer, provide:

```text
Destination:
Matching routes:
Longest prefix:
Selected route:
Reason:
```

---

# Lab 11 — Explain It Without Technical Jargon

Imagine you're explaining route selection to a non-technical manager.

Explain:

> "Why does the router choose `10.10.10.128/25` instead of `10.0.0.0/8` for `10.10.10.150`?"

Your explanation should be understandable to someone who doesn't know networking.

This is useful because cybersecurity professionals often need to communicate technical decisions to:

* Managers
* Auditors
* Risk teams
* Clients
* Business stakeholders

---

# Lab Completion Checklist

Before marking Lesson 09 as complete:

* [ ] I can read a basic routing table.
* [ ] I understand what a prefix means.
* [ ] I understand `/8`, `/16`, `/24`, and `/25` at a basic level.
* [ ] I can identify whether a route matches a destination.
* [ ] I can identify multiple matching routes.
* [ ] I can apply Longest Prefix Match.
* [ ] I understand why `/25` is more specific than `/24`.
* [ ] I understand the role of `0.0.0.0/0`.
* [ ] I completed the Packet Tracer exercise.
* [ ] I observed packet movement in Simulation Mode.
* [ ] I can explain how routing can affect security controls.
* [ ] I can explain why GRC professionals should understand routing.

---

# Final Reflection

Write a short answer in your own words:

> **How does a router decide which route to use when multiple routes match a destination?**

Do not copy the definition from the README.

The goal is to see whether you can explain the concept from your own understanding.
