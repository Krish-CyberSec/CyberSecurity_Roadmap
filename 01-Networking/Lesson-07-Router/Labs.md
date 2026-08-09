# Hands-on Networking Lab

## Lab 1 — Find Your IP Address and Default Gateway

### Step 1: Run the Command

Open **Command Prompt** on Windows and run:

```cmd
ipconfig
```

### Step 2: Find

Look for:

* **IPv4 Address**
* **Default Gateway**

### Questions

**1. What is your Default Gateway?**

> **Answer:** 192.xx.xx.1

**2. Why is the Default Gateway in the same subnet as your computer?**

> **Answer:**
> The Default Gateway is usually in the same subnet as the computer because the computer needs to communicate directly with the router over the local network. If the gateway were outside the local subnet, the computer would not be able to reach it directly using normal local-network communication.

---

## Lab 2 — Trace the Route to Google

### Step 1: Run the Command

Open **Command Prompt** and run:

```cmd
tracert google.com
```

### Step 2: Observe

Look at the first hop in the output.

### Questions

**1. What is the first hop?**

> **Answer:** Router Default Gateway 

**2. Why is it usually your router?**

> **Answer:**
> The first hop is usually the home Wi-Fi router because the router is the device that connects the local home network to the ISP. When your computer sends traffic outside the local network, it normally forwards that traffic to the router, which then sends it toward the ISP and the Internet.

---

# Lab 3 — Draw Your Home Network

## Network Diagram

Draw a network similar to the example below:

```text
                    ┌───────────────┐
                    │ Google Server │
                    └───────┬───────┘
                            │
                         Internet
                            │
                    ┌───────┴───────┐
                    │      ISP      │
                    └───────┬───────┘
                            │
                    ┌───────┴───────┐
                    │  Wi-Fi Router │
                    │               │
                    │ Router        │
                    │ Switch        │
                    │ ARP           │
                    └───┬───────┬───┘
                        Wi-Fi   Wi-Fi
                         │        │
                  ┌──────┘        └──────┐
                  │                      │
             ┌────┴────┐            ┌────┴────┐
             │ Laptop  │            │ Mobile  │
             └─────────┘            └─────────┘
```

## Where Networking Concepts Are Involved

### Switch

The **switch** is usually built into a home Wi-Fi router. It connects devices on the local network, such as your laptop and mobile phone.

### Router

The **router** connects your home/local network to the ISP and forwards traffic between different networks.

### IP Address

Every device on the network normally has an **IP address**.

Example:

```text
Laptop:  192.168.1.10
Mobile:  192.168.1.11
Router:  192.168.1.1
```

> These are example addresses. Your actual addresses may be different.

### MAC Address

A **MAC address** identifies a network interface at the local network level.

Example:

```text
Laptop MAC:  AA-BB-CC-11-22-33
Mobile MAC:  DD-EE-FF-44-55-66
```

### ARP

**ARP (Address Resolution Protocol)** is used on an IPv4 local network to find the MAC address associated with an IP address.

For example:

```text
Laptop knows:
192.168.1.1

ARP asks:
"Who has 192.168.1.1?"

Router replies:
"192.168.1.1 is at AA-BB-CC-11-22-33"
```

The laptop can then send the local Ethernet/Wi-Fi frame to the router's MAC address.

---

# Summary

| Concept           | Where It Is Involved                                          |
| ----------------- | ------------------------------------------------------------- |
| **IP Address**    | Laptop, mobile, router, and other network devices             |
| **MAC Address**   | Network interfaces of devices on the local network            |
| **ARP**           | Maps local IPv4 addresses to MAC addresses                    |
| **Switch**        | Connects devices within the local network                     |
| **Router**        | Connects the home network to other networks/Internet          |
| **ISP**           | Provides the connection from the home network to the Internet |
| **Internet**      | Connects the home network to external services                |
| **Google Server** | Example destination on the Internet                           |


