# Chapter 05 - Address Resolution Protocol (ARP)

## Module Information

| Field | Details |
|--------|---------|
| Module | Networking for Cybersecurity |
| Chapter | 05 |
| Topic | Address Resolution Protocol (ARP) |
| Difficulty | Beginner |
| Estimated Study Time | 90–120 Minutes |
| Status | Completed |
| Prerequisites | Chapter 01, Chapter 02, Chapter 03, Chapter 04 |

---

# Learning Objectives

After completing this chapter, you should be able to:

- Explain why ARP was invented.
- Describe the problem ARP solves.
- Explain the complete ARP Request and Reply process.
- Understand how the ARP Cache works.
- Explain why ARP only works inside a Local Area Network (LAN).
- Understand why ARP is vulnerable to ARP Spoofing.
- Analyze ARP communication using Wireshark.

---

# The Problem

Imagine your laptop wants to send data to another device connected to the same Wi-Fi network.

Your laptop already knows the destination IP Address.

Example:

```
Destination IP

192.168.3.150
```

However, Ethernet and Wi-Fi cannot deliver data using only an IP Address.

They require the destination device's **MAC Address** to build an Ethernet frame.

This creates an important problem.

> **How can a device discover the MAC Address associated with a known IP Address?**

This is the exact problem that ARP was designed to solve.

---

# What is ARP?

**ARP (Address Resolution Protocol)** is a protocol used to map an IPv4 Address to a MAC Address within a Local Area Network (LAN).

Simply put,

> If a device knows the destination IP Address but not the destination MAC Address, ARP helps discover it.

---

# Why Was ARP Invented?

We already know:

- IP Address identifies **where** a device is located.
- MAC Address identifies **which network interface** should receive the frame.

Although a sender may know the destination IP Address, it cannot communicate until it learns the destination MAC Address.

ARP bridges the gap between Layer 3 (IP Addressing) and Layer 2 (MAC Addressing).

---

# Complete ARP Working

The following diagram explains the complete communication process.

```
(SENDER)

Laptop

IP:
192.168.3.143

MAC:
AA-AA-AA-AA-AA-AA

           │
           │
           ▼

Destination IP

192.168.3.150

           │
           │
           ▼

Is destination inside my network?

        │
     YES │
        ▼

Check ARP Cache

        │
        │
Exists? │
        │
  YES   ▼

Send Ethernet Frame

Destination MAC

↓

Receiver

--------------------------------

NO

↓

Broadcast ARP Request

"Who has 192.168.3.150?"

↓

Switch floods the broadcast

↓

All devices receive it

↓

Only

192.168.3.150

Replies

"My MAC is

BB-BB-BB-BB-BB"

↓

Sender updates ARP Cache

↓

Build Ethernet Frame

↓

Send Actual Data
```

---

# Step-by-Step Explanation

## Step 1

The sender wants to communicate with another device.

It already knows the destination IP Address.

---

## Step 2

The sender checks whether the destination belongs to the same Local Area Network.

If not,

the packet is sent to the Default Gateway (Router).

If yes,

communication continues using ARP.

---

## Step 3

The sender checks its ARP Cache.

The ARP Cache stores recently learned IP-to-MAC mappings.

If an entry already exists,

communication starts immediately.

---

## Step 4

If no entry exists,

the sender broadcasts an ARP Request.

Example:

> Who has IP Address 192.168.3.150?

---

## Step 5

Every device connected to the LAN receives the broadcast.

Only the device that owns the requested IP Address responds.

Example:

> I am 192.168.3.150.

> My MAC Address is BB-BB-BB-BB-BB-BB.

---

## Step 6

The sender stores this mapping inside the ARP Cache.

Example:

| IP Address | MAC Address |
|------------|-------------|
|192.168.3.150|BB-BB-BB-BB-BB-BB|

---

## Step 7

The sender builds the Ethernet Frame using the destination MAC Address.

The actual data communication now begins.

---

# Enterprise Perspective

Imagine an office with hundreds of employees.

Every employee accesses:

- File Servers
- Printers
- Internal Applications

Without ARP,

every communication would require manually knowing every device's MAC Address.

ARP automates this process, making local communication efficient and scalable.

---

# Cybersecurity Perspective

Although ARP is simple,

it has one major weakness.

ARP does **not authenticate** replies.

Any device can claim:

> I am 192.168.3.150.

If another device trusts this false reply,

the attacker can intercept traffic.

This attack is known as:

- ARP Spoofing
- ARP Poisoning

It is commonly used in:

- Man-in-the-Middle (MITM) attacks
- Traffic interception
- Credential theft
- Session hijacking

---

# Common Misconceptions

## ❌ ARP works across the Internet.

False.

ARP only works inside a Local Area Network.

---

## ❌ Every communication requires an ARP Request.

False.

Previously learned mappings are stored inside the ARP Cache.

---

## ❌ Routers send ARP Requests for local communication.

False.

The sender device generates the ARP Request when it needs the destination MAC Address on the local network.


# Key Takeaways

- ARP maps IPv4 Addresses to MAC Addresses.
- ARP only works inside a Local Area Network.
- ARP uses Broadcast Requests and Unicast Replies.
- The ARP Cache improves communication efficiency.
- ARP has no built-in authentication.
- ARP Spoofing is possible because devices trust ARP Replies.

---

# Summary

ARP acts as the bridge between IP Addresses and MAC Addresses.

Whenever a device knows the destination IP Address but does not know the destination MAC Address, ARP helps discover it.

This process allows Ethernet and Wi-Fi networks to deliver data to the correct device efficiently.

Understanding ARP is essential before learning Switching, Ethernet, and Layer 2 security attacks.
