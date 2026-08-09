# Chapter 07 - Router: Connecting Different Networks

## Module Information

| Field                | Details                      |
| -------------------- | ---------------------------- |
| Module               | Networking for Cybersecurity |
| Chapter              | 07                           |
| Topic                | Router                       |
| Difficulty           | Beginner                     |
| Estimated Study Time | 90–120 Minutes               |
| Status               | Completed                    |
| Prerequisites        | Chapters 01–06               |

---

# Learning Objectives

After completing this chapter, you should be able to:

* Explain why routers were invented.
* Differentiate between a switch and a router.
* Understand the purpose of a Default Gateway.
* Explain how a router forwards packets.
* Understand the basic role of a Routing Table.
* Explain why routers are essential for Internet communication.
* Understand the importance of routers in enterprise security.

---

# The Engineering Problem

Imagine your laptop wants to print a document.

Laptop

IP:
192.168.3.143

Printer

IP:
192.168.3.150

Both devices belong to the same Local Area Network (LAN).

The laptop:

* Resolves the printer's MAC Address using ARP.
* Sends the frame to the Switch.
* The Switch forwards it to the Printer.

Everything works perfectly.

Now imagine you want to open:

https://github.com

The GitHub server is **not** part of your local network.

The Switch has no knowledge of devices outside your LAN.

This creates a new problem:

> **How can a device communicate with another network?**

---

# Why Switches Aren't Enough

Switches are designed for communication **within the same network**.

They forward Ethernet frames based on MAC Addresses.

However, they cannot determine how to reach another network.

To communicate with remote networks, a new device was required.

---

# The Solution

The solution is the **Router**.

A Router is responsible for connecting different networks and forwarding packets between them.

Unlike a Switch, which uses MAC Addresses, a Router makes forwarding decisions using **IP Addresses**.

---

# What is a Router?

A **Router** is a Layer 3 networking device that forwards packets between different networks using IP Addresses.

Its primary responsibility is to determine where packets should travel after they leave the local network.

---

# How Does a Router Work?

Suppose your laptop wants to access Google's DNS server.

Destination IP:

8.8.8.8

The laptop first asks:

"Is this destination inside my network?"

If the answer is **No**,

the laptop sends the packet to its **Default Gateway**.

Before doing so, it uses ARP to discover **the router's MAC Address**, not Google's MAC Address.

Once the packet reaches the router, the router examines the destination IP Address and decides where the packet should go next.

---

# Default Gateway

The Default Gateway is the router that acts as the exit point from your local network.

Whenever a device wants to communicate with another network, it forwards the packet to the Default Gateway.

Without a Default Gateway, devices can communicate only within their own local network.

---

# Router vs Switch

| Switch                               | Router                      |
| ------------------------------------ | --------------------------- |
| Layer 2 Device                       | Layer 3 Device              |
| Uses MAC Addresses                   | Uses IP Addresses           |
| Connects devices within the same LAN | Connects different networks |
| Uses a CAM Table                     | Uses a Routing Table        |
| Forwards Ethernet Frames             | Forwards IP Packets         |

---

# OSI Model Mapping

| Layer   | Related Topics           |
| ------- | ------------------------ |
| Layer 7 | Browser (Introduction)   |
| Layer 6 | Coming Later             |
| Layer 5 | Coming Later             |
| Layer 4 | TCP / UDP (Upcoming)     |
| Layer 3 | Router, IP Address       |
| Layer 2 | Switch, ARP, MAC Address |
| Layer 1 | Ethernet, Wi-Fi          |

---

# Enterprise Perspective

Large organizations consist of multiple departments such as:

* Human Resources
* Finance
* Engineering
* Guest Network
* Data Center

Each department is often placed in a separate network.

Routers enable communication between these networks while enforcing security and routing policies.

---

# Cybersecurity Perspective

Routers are among the most critical devices in an enterprise network.

Common security concerns include:

* Route Hijacking
* Weak Administrator Passwords
* Firmware Vulnerabilities
* Misconfigured Routing
* Unauthorized Remote Access

Compromising a router may allow an attacker to monitor or manipulate traffic across multiple networks.

---

# Common Misconceptions

## ❌ Routers use MAC Addresses to decide where packets go.

False.

Routers examine the destination **IP Address** when making forwarding decisions.

---

## ❌ ARP works across the Internet.

False.

ARP is used only within the local network to discover the MAC Address of the Default Gateway or another local device.

---

## ❌ Switches can replace Routers.

False.

Switches connect devices inside a LAN.

Routers connect different networks.

---

# Key Takeaways

* Routers operate at Layer 3.
* Routers forward packets using IP Addresses.
* The Default Gateway is the exit point from a LAN.
* ARP is used only to discover the router's MAC Address within the local network.
* Routers connect different networks and make Internet communication possible.

---

# Summary

Routers solve the problem of communication between different networks.

While Switches deliver Ethernet frames inside a Local Area Network, Routers examine destination IP Addresses and forward packets toward their final destination using routing decisions.

Understanding routers is essential before learning Routing Tables, NAT, Firewalls, VPNs, and enterprise network architecture.
