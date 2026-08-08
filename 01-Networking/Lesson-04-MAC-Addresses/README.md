# Chapter 04 - MAC Address: The Physical Identity of a Device

## Module Information

| Field | Details |
|--------|---------|
| Module | Networking for Cybersecurity |
| Chapter | 04 |
| Topic | MAC Address |
| Difficulty | Beginner |
| Estimated Study Time | 90–120 Minutes |
| Status | Completed |
| Prerequisites | Chapter 01, 02 & 03 |

---

# Learning Objectives

After completing this chapter, you should be able to:

- Explain why MAC Addresses were invented.
- Differentiate between IP Addresses and MAC Addresses.
- Understand the role of MAC Addresses in local network communication.
- Identify the MAC Address of your device.
- Explain how MAC Addresses are used in enterprise environments.
- Understand the security implications of MAC Addresses.

---

# The Problem

Imagine five devices connected to your home Wi-Fi.

- Laptop
- Mobile Phone
- Smart TV
- Printer
- Tablet

Every device has an IP Address.

Suppose your laptop wants to send a document to the printer.

The laptop already knows the printer's IP Address.

**Question:**

How does the laptop know which physical device on the local network should receive the data?

This is the problem engineers needed to solve.

---

# Why IP Address Alone Isn't Enough

An IP Address identifies the location of a device on a network.

However, devices on the same Local Area Network (LAN) still need a way to identify the exact network interface that should receive a frame.

Without another identifier, every device would receive and process every frame unnecessarily.

Engineers solved this problem by introducing the **MAC Address**.

---

# What is a MAC Address?

A **MAC (Media Access Control) Address** is a unique hardware identifier assigned to a network interface.

Unlike an IP Address, which is a logical address, a MAC Address identifies the physical network interface within a local network.

Example:

```
00:1A:2B:3C:4D:5E
```

Every network adapter has its own MAC Address.

Examples include:

- Wi-Fi Adapter
- Ethernet Adapter
- Virtual Machine Adapter
- Bluetooth Adapter

---

# Who Assigns a MAC Address?

MAC Addresses are assigned by the hardware manufacturer.

Manufacturers such as Intel, Dell, HP, Realtek, and Broadcom receive unique prefixes from IEEE and assign unique MAC Addresses to their network interfaces.

This helps prevent duplicate hardware addresses.

---

# IP Address vs MAC Address

| IP Address | MAC Address |
|------------|-------------|
| Logical Address | Physical (Hardware) Address |
| Identifies where a device is located | Identifies which device should receive the frame on the local network |
| Can change | Usually remains the same |
| Used for communication across networks | Used within a Local Area Network |
| Assigned by Router, ISP or Administrator | Assigned by Manufacturer |

---

# Enterprise Perspective

In an enterprise environment, MAC Addresses help administrators:

- Identify company-owned devices
- Track network interfaces
- Manage access to secure wireless networks
- Detect unauthorized devices

When a new device joins the corporate network, its MAC Address may be logged and compared against approved assets.

---

# Cybersecurity Perspective

MAC Addresses are valuable during security investigations.

Security teams use them to:

- Identify devices inside a LAN
- Detect rogue devices
- Investigate suspicious network activity
- Correlate network logs

Attackers may attempt **MAC Spoofing** to impersonate another device or bypass simple MAC-based access controls.

---

# Common Misconceptions

## ❌ MAC Address and IP Address are the same.

No.

An IP Address identifies where a device is located.

A MAC Address identifies the specific network interface on the local network.

---

## ❌ Every device has only one MAC Address.

Incorrect.

Each network interface has its own MAC Address.

For example:

- Wi-Fi Adapter
- Ethernet Adapter
- Virtual Adapter

---

## ❌ MAC Addresses never change.

The manufacturer-assigned MAC usually remains constant, but operating systems can temporarily spoof or randomize MAC Addresses.

---

# Hands-on Lab

## Task 1

Run:

```
ipconfig /all
```

Identify:

- Physical Address
- Adapter Name

---

## Task 2

Run:

```
getmac
```

Compare the output with `ipconfig /all`.

---

# Key Takeaways

- Every network interface has a MAC Address.
- MAC Addresses identify devices on a local network.
- IP Addresses and MAC Addresses serve different purposes.
- Enterprises use MAC Addresses for inventory and network management.
- Attackers may spoof MAC Addresses.
- Understanding MAC Addresses is essential before learning ARP.

---

# Summary

MAC Addresses solve the problem of identifying the correct device on a Local Area Network.

While IP Addresses help locate devices across networks, MAC Addresses ensure that frames are delivered to the correct network interface within the local network.

This concept forms the foundation for understanding ARP, switching, and Ethernet communication.
