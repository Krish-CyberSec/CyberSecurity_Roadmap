# Chapter 05 – Interview Questions

---

# Beginner Level

## Q1. What is ARP?

### Answer

ARP (Address Resolution Protocol) is a Layer 2/Layer 3 supporting protocol used to resolve an IPv4 Address into a MAC Address within a Local Area Network.

---

## Q2. Why was ARP invented?

### Answer

ARP was invented because devices communicate using IP Addresses, but Ethernet and Wi-Fi require MAC Addresses to deliver frames on a local network.

---

## Q3. What problem does ARP solve?

### Answer

ARP helps a sender discover the MAC Address associated with a known IPv4 Address.

---

## Q4. What is an ARP Request?

### Answer

An ARP Request is a broadcast message asking:

> "Who has this IP Address?"

Every device on the LAN receives it.

---

## Q5. What is an ARP Reply?

### Answer

An ARP Reply is sent by the device that owns the requested IP Address.

It contains the device's MAC Address.

---

# Intermediate Level

## Q6. What is the ARP Cache?

### Answer

The ARP Cache is a temporary table that stores IP-to-MAC mappings.

It reduces unnecessary ARP broadcasts by reusing previously discovered addresses.

---

## Q7. Why does ARP only work inside a LAN?

### Answer

MAC Addresses are only meaningful within a Local Area Network. Routers do not forward ARP broadcasts between networks.

---

## Q8. What happens if the MAC Address already exists in the ARP Cache?

### Answer

The sender immediately builds the Ethernet frame and sends the data without generating another ARP Request.

---

## Q9. Why is ARP considered insecure?

### Answer

ARP has no authentication mechanism. Any device can send a forged ARP Reply, making ARP Spoofing possible.

---

# Advanced Level

## Q10. Explain the complete ARP process.

### Answer

1. The sender knows the destination IP Address.
2. It checks whether the destination is on the same LAN.
3. It checks the ARP Cache.
4. If no mapping exists, it broadcasts an ARP Request.
5. The target device replies with its MAC Address.
6. The sender stores the mapping in the ARP Cache.
7. The sender builds the Ethernet frame and sends the actual data.

---

## Q11. What is ARP Spoofing?

### Answer

ARP Spoofing is an attack in which a malicious device sends fake ARP Replies to associate its own MAC Address with another device's IP Address.

This allows the attacker to intercept or manipulate network traffic.

---

## Q12. How can a SOC Analyst detect ARP Spoofing?

### Answer

Possible indicators include:

- Duplicate IP-to-MAC mappings
- Frequent ARP Replies without corresponding Requests
- Sudden changes in ARP Cache entries
- Multiple IP Addresses resolving to the same MAC Address
- Alerts from IDS/IPS or network monitoring tools

---

# Scenario-Based Questions

## Q13.

Your laptop knows the destination IP Address but not the MAC Address.

What happens next?

### Answer

The laptop checks its ARP Cache.

If no mapping exists, it broadcasts an ARP Request to discover the destination MAC Address.

---

## Q14.

Your laptop wants to communicate with Google's DNS Server (8.8.8.8).

Will it perform ARP for Google's MAC Address?

### Answer

No.

Because 8.8.8.8 is outside the local network, the laptop performs ARP for the **default gateway's MAC Address**, not Google's MAC Address.

The router then forwards the packet toward the Internet.

---

# Think Like an Engineer

### Question

Imagine ARP did not exist.

How would you design a system that allows devices on the same LAN to discover each other's hardware addresses?

Think about:

- Scalability
- Performance
- Security
- Ease of management

There is no single correct answer. The goal is to develop engineering thinking rather than memorization.
