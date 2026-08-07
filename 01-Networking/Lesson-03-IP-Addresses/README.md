
# Lesson 03 - IP Addresses

## Module Information

| Field | Details |
|--------|---------|
| Module | Networking for Cybersecurity |
| Lesson | 03 |
| Topic | IP Addresses |
| Difficulty | Beginner |
| Estimated Study Time | 90–120 Minutes |
| Status | Completed |
| Prerequisites | Lesson 01 & Lesson 02 |

---

# Learning Objectives

After completing this lesson, you should be able to:

- Explain what an IP address is.
- Understand why every device requires an IP address.
- Differentiate between IPv4 and IPv6.
- Understand the difference between Public and Private IP addresses.
- Explain why IP addresses are important in networking and cybersecurity.
- Identify your own public and private IP addresses.

---

# Introduction

Imagine ordering a package from Amazon.

The delivery partner cannot deliver your package by knowing only your name.

They need your complete address.

Similarly, computers also require an address before they can communicate.

That address is called an **IP Address**.

Whenever we browse the Internet, send an email, watch YouTube, or play online games, our devices communicate using IP addresses.

Without them, data would never know where to go.

---

# What is an IP Address?

An **Internet Protocol (IP) Address** is a unique logical address assigned to a device connected to a network.

It allows devices to identify each other and exchange information.

Think of it as the digital address of a device.

Just like your home has a postal address, every device connected to a network requires an IP address.

---

# Why Do We Need IP Addresses?

Imagine a college with 2,000 students.

If the teacher simply says:

> "Call Rahul."

How would anyone know which Rahul?

Now imagine every student has a unique roll number.

Finding the correct student becomes easy.

An IP address works the same way.

It uniquely identifies devices so that data reaches the correct destination.

---

# IPv4

Most networks today still use IPv4.

Example:

192.168.1.10

IPv4 consists of four numbers separated by dots.

Each number ranges from **0 to 255**.

Example IPv4 Addresses:

- 192.168.1.10
- 10.0.0.15
- 172.16.20.5
- 8.8.8.8

---

# IPv6

IPv6 was introduced because IPv4 addresses are limited.

Example:

2001:4860:4860::8888

Compared to IPv4, IPv6 provides a significantly larger address space.

Although IPv6 adoption is increasing, many organizations still use IPv4.

---

# Public vs Private IP Address

## Private IP

A private IP address is used inside a local network.

Examples:

- 192.168.x.x
- 10.x.x.x
- 172.16.x.x – 172.31.x.x

Private IP addresses cannot be accessed directly from the Internet.

Examples include:

- Home Wi-Fi
- College labs
- Office networks

---

## Public IP

A public IP address is assigned by an Internet Service Provider (ISP).

It allows communication over the Internet.

When you open Google or LinkedIn, websites see your public IP address instead of your private IP address.

---

# Enterprise Example

Imagine an office with 500 employees.

Each employee's laptop has a private IP address.

Example:

192.168.1.25

When an employee accesses the Internet:

Laptop

↓

Office Router

↓

Public IP

↓

Internet

The office router translates the private IP into the organization's public IP before sending traffic to the Internet.

---

# Cybersecurity Perspective

IP addresses are extremely important during security investigations.

Security analysts use them to:

- Identify suspicious login attempts.
- Track malicious traffic.
- Investigate attack sources.
- Detect unusual network activity.
- Correlate security events.

Example:

Failed Login

Source IP:

185.xxx.xxx.xxx

Questions a SOC Analyst may ask:

- Is it a public IP?
- Which country is it from?
- Has it appeared in previous attacks?
- Should it be blocked?

---

# Common Misconceptions

### ❌ An IP Address identifies a person.

Not exactly.

An IP address identifies a network interface at a particular point in time.

It does not always identify an individual user.

---

### ❌ Every device has a Public IP Address.

False.

Most devices use private IP addresses.

Usually, only the router communicates with the Internet using a public IP address.

---

### ❌ IP Addresses never change.

False.

Many Internet connections use dynamic IP addresses, which can change over time.

---

# Real-World Example

Suppose you visit:

https://github.com

Your computer first resolves GitHub's domain name into an IP address.

It then sends packets to that IP address through your router and ISP.

GitHub's server processes your request and returns the webpage.

Without an IP address, your browser would never know where GitHub's server is located.

---

# Commands Used

```cmd
ipconfig
```

Displays your local network configuration.

```cmd
ping google.com
```

Checks connectivity and resolves the domain name to an IP address.

---

# Key Takeaways

- Every device on a network requires an IP address.
- IP addresses uniquely identify devices on a network.
- IPv4 is still the most commonly used version.
- IPv6 solves the IPv4 address exhaustion problem.
- Public and Private IP addresses serve different purposes.
- IP addresses are one of the most valuable pieces of information during cyber investigations.

---

# Summary

IP addresses are the foundation of modern networking.

They allow devices to identify one another and communicate across local and global networks.

Understanding IP addressing is essential before learning advanced networking topics such as routing, subnetting, NAT, DNS, firewalls, and cloud networking.
