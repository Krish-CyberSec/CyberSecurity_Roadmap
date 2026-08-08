# Chapter 04 – Interview Questions

---

# Beginner Level

## Q1. What is a MAC Address?

### Answer

A MAC (Media Access Control) Address is a unique hardware identifier assigned to a network interface. It is used to identify devices within a local network.

---

## Q2. Why do devices need MAC Addresses?

### Answer

MAC Addresses help identify the correct destination device on a Local Area Network. Without them, devices would not know which network interface should receive a frame.

---

## Q3. Who assigns MAC Addresses?

### Answer

Hardware manufacturers assign MAC Addresses to network interfaces. The IEEE allocates manufacturer prefixes to ensure uniqueness.

---

## Q4. Can two devices have the same MAC Address?

### Answer

Normally, no. Manufacturers assign unique MAC Addresses. However, MAC Addresses can be spoofed or duplicated intentionally in certain situations.

---

# Intermediate Level

## Q5. Explain the difference between an IP Address and a MAC Address.

### Answer

An IP Address is a logical address used for communication across networks, while a MAC Address is a physical hardware identifier used for communication within a local network.

---

## Q6. Why isn't an IP Address enough for communication inside a LAN?

### Answer

An IP Address identifies the destination network, but devices still need a hardware identifier to deliver frames to the correct network interface. This role is performed by the MAC Address.

---

## Q7. What is MAC Spoofing?

### Answer

MAC Spoofing is the practice of changing the MAC Address presented by a device to impersonate another device or bypass certain network restrictions.

---

## Q8. Why do enterprises keep records of MAC Addresses?

### Answer

Organizations use MAC Addresses for asset management, network monitoring, device identification, and detecting unauthorized devices.

---

# Advanced Level

## Q9. How can a SOC Analyst use MAC Addresses during an investigation?

### Answer

SOC Analysts use MAC Addresses to identify devices on a local network, correlate network events, trace suspicious activity, and detect rogue or unauthorized systems.

---

## Q10. Imagine MAC Addresses did not exist. What problems would occur?

### Answer

Without MAC Addresses, devices on a Local Area Network would not know which network interface should receive a frame. Local communication would become unreliable and Ethernet-based networking would not function as designed.

---

# Think Like an Engineer

### Scenario

Your laptop knows the destination IP Address of another computer on the same network.

It does **not** know its MAC Address.

**Question**

How do you think your laptop discovers the destination MAC Address before sending the data?

> This question will be answered in the next chapter: **ARP (Address Resolution Protocol).**
