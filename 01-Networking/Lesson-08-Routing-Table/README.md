# Chapter 08 - Routing Table: The GPS of a Network

## Module Information

| Field                | Details                      |
| -------------------- | ---------------------------- |
| Module               | Networking for Cybersecurity |
| Chapter              | 08                           |
| Topic                | Routing Table                |
| Difficulty           | Beginner                     |
| Estimated Study Time | 90–120 Minutes               |
| Status               | Completed                    |
| Prerequisites        | Chapters 01–07               |

---

# Learning Objectives

After completing this chapter, you should be able to:

* Explain what a Routing Table is.
* Understand why routers need Routing Tables.
* Explain how a router makes a forwarding decision.
* Understand Directly Connected Routes.
* Understand the Default Route.
* Explain Next Hop and Exit Interface.
* Read a basic Routing Table.
* Understand why Routing Tables matter from a cybersecurity perspective.

---

# The Engineering Problem

In the previous chapter, we learned that a Router connects different networks.

But this creates another question:

> **When a router receives a packet, how does it know where to send it next?**

Imagine a router receiving a packet destined for:

```text
8.8.8.8
```

The router cannot ask every router on the Internet where that destination is.

It needs a structured way to make forwarding decisions.

This is where the **Routing Table** comes in.

---

# What is a Routing Table?

A **Routing Table** is a collection of routes maintained by a router that helps it determine where packets should be forwarded.

A route can contain information such as:

* Destination Network
* Prefix / Subnet Mask
* Next Hop
* Exit Interface
* Route Type

The basic question a router is trying to answer is:

> **"For this destination IP, where should I send the packet next?"**

---

# Example Routing Table

Consider this simplified routing table:

| Destination Network | Prefix | Next Hop    | Exit Interface | Type      |
| ------------------- | ------ | ----------- | -------------- | --------- |
| 192.168.1.0         | /24    | Direct      | G0/0           | Connected |
| 192.168.2.0         | /24    | Direct      | G0/1           | Connected |
| 10.10.0.0           | /16    | 192.168.2.2 | G0/1           | Static    |
| 0.0.0.0             | /0     | ISP Router  | G0/0           | Default   |

This table gives the router information about networks it can reach.

---

# Understanding the Routing Table

## 1. Destination Network

This identifies the network the route applies to.

Example:

```text
192.168.1.0/24
```

This represents the network containing addresses from:

```text
192.168.1.0
to
192.168.1.255
```

---

## 2. Prefix

The prefix tells the router how large the destination network is.

For example:

```text
/24
```

corresponds to:

```text
255.255.255.0
```

We will study subnetting and prefixes in much greater depth later.

---

## 3. Next Hop

The **Next Hop** identifies the next router that should receive the packet.

Example:

```text
Destination:

10.10.0.0/16

Next Hop:

192.168.2.2
```

The router forwards the packet toward `192.168.2.2`.

---

## 4. Exit Interface

The Exit Interface is the router interface through which the packet should leave.

Example:

```text
G0/1
```

---

## 5. Route Type

Routes can come from different sources.

For our current level, the important categories are:

* Connected
* Static
* Dynamic
* Default

We will study these in greater detail as networking progresses.

---

# Directly Connected Routes

When an interface on a router is configured with an IP address and is operational, the router knows about the network directly connected to that interface.

For example:

```text
Router G0/0

192.168.1.1/24
```

The router knows:

```text
192.168.1.0/24
```

is directly connected.

Similarly:

```text
Router G0/1

192.168.2.1/24
```

means:

```text
192.168.2.0/24
```

is directly connected.

---

# Default Route

Sometimes a router does not have a specific route for a destination.

It can use a **Default Route**.

The common notation is:

```text
0.0.0.0/0
```

The default route acts as a catch-all route.

Conceptually:

```text
Specific Route Found
        │
        ▼
Forward using that route

        OR

No Specific Route
        │
        ▼
Use Default Route
```

A common example is a home router forwarding unknown Internet destinations toward the ISP.

---

# How a Router Uses the Routing Table

Suppose a router receives a packet destined for:

```text
192.168.2.25
```

The router examines its Routing Table.

It finds:

```text
192.168.2.0/24
```

The destination belongs to that network.

Therefore, the router forwards the packet through:

```text
G0/1
```

---

# Another Example

Now suppose the destination is:

```text
8.8.8.8
```

If the router has no more specific route for `8.8.8.8`, it can use:

```text
0.0.0.0/0
```

and forward the packet toward its upstream router.

---

# Complete Decision Flow

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
Is there a matching route?
      │
   ┌──┴──┐
  YES    NO
   │      │
   ▼      ▼
Use      Is there a
matching Default Route?
route       │
            ├── YES → Use Default Route
            │
            └── NO → Drop Packet
```

The exact route-selection process is more sophisticated than this simplified diagram. We will study **longest-prefix matching, route preference, and metrics** in later chapters.

---

# Enterprise Perspective

Large organizations may have many interconnected networks:

* HR
* Finance
* Engineering
* Data Center
* Branch Offices
* Cloud Networks
* Guest Networks

Routers need routing information to determine how traffic should move between these networks.

A routing problem can therefore affect an entire department or organization.

---

# Cybersecurity Perspective

Routing information is also security-sensitive.

Attackers may attempt to:

* Modify routing information.
* Redirect traffic.
* Perform route manipulation.
* Compromise network infrastructure.
* Interfere with legitimate communication.

Security teams may monitor network infrastructure and configuration changes to identify unexpected routing behavior.

---

# GRC Perspective

Routing is not only a technical concern.

A GRC professional may need to understand routing concepts when reviewing:

* Network architecture.
* Network segmentation.
* Security boundaries.
* Change management.
* Network access controls.
* Critical infrastructure configurations.

You don't necessarily need to configure routers as a GRC analyst, but understanding what a routing table represents helps you understand the organization's network architecture and associated risks.

---

# OSI Model Mapping

| OSI Layer               | Topics Learned                        |
| ----------------------- | ------------------------------------- |
| Layer 7 – Application   | Browser / Applications                |
| Layer 6 – Presentation  | Not covered yet                       |
| Layer 5 – Session       | Not covered yet                       |
| Layer 4 – Transport     | Not covered yet                       |
| **Layer 3 – Network**   | **IP Address, Router, Routing Table** |
| **Layer 2 – Data Link** | **Switch, MAC Address, ARP**          |
| Layer 1 – Physical      | Ethernet / Wi-Fi                      |

Routing Tables belong to the **Layer 3 networking process** because they are used to make forwarding decisions based on IP networks.

---

# Hands-on Lab

## Lab 1 – View the Routing Table

In Cisco Packet Tracer, open the router CLI and run:

```text
show ip route
```

Identify:

* Connected routes
* Local routes
* Default route, if configured
* Associated interfaces

---

## Lab 2 – Observe Your Computer's Routing Table

On Windows:

```cmd
route print
```

or:

```cmd
ipconfig
```

Compare the concepts you see with the routing table we studied.

---

## Lab 3 – Trace a Destination

Run:

```cmd
tracert google.com
```

Observe the path taken toward the destination.

Ask yourself:

> What role does each router along the path play?

---

# Key Takeaways

* A Routing Table helps a router decide where packets should go.
* Routes describe reachable networks.
* A route can identify a next hop or an exit interface.
* Directly connected networks are automatically known by the router.
* `0.0.0.0/0` represents a default route.
* Routing is primarily a Layer 3 function.
* Routing knowledge is important for Network Engineering, SOC, Security Engineering, and GRC.

---

# Before You Move On

Can you answer these without looking at your notes?

* [ ] What is a Routing Table?
* [ ] Why does a router need one?
* [ ] What is a Destination Network?
* [ ] What is a Next Hop?
* [ ] What is an Exit Interface?
* [ ] What is a Directly Connected Route?
* [ ] What does `0.0.0.0/0` represent?
* [ ] What happens when no matching route exists?
* [ ] Which OSI layer is responsible for routing?

If you cannot explain these clearly, revise this chapter before continuing.

---

# Summary

A Router gives us the ability to connect different networks.

A Routing Table gives the Router the information required to make forwarding decisions.

This creates the next important question:

> **What happens when multiple routes match the same destination?**

That question leads us to the next stage of routing: **route selection, longest-prefix matching, metrics, and route preference.**
