# Chapter 06 - Network Switch: The Intelligent Traffic Manager

## Module Information

| Field                | Details                      |
| -------------------- | ---------------------------- |
| Module               | Networking for Cybersecurity |
| Chapter              | 06                           |
| Topic                | Network Switch               |
| Difficulty           | Beginner                     |
| Estimated Study Time | 90–120 Minutes               |
| Status               | Completed                    |
| Prerequisites        | Chapters 01–05               |

---

# Learning Objectives

After completing this chapter, you should be able to:

* Explain why switches were invented.
* Differentiate between a Hub and a Switch.
* Understand how a switch learns MAC Addresses.
* Explain the purpose of the CAM Table.
* Describe how switches forward Ethernet frames.
* Understand Unknown Unicast Flooding.
* Explain why switches are essential in enterprise networks.
* Identify common Layer 2 attacks against switches.

---

# The Engineering Problem

Early computer networks commonly used **Hubs**.

A Hub simply repeated incoming signals to every connected device.

Imagine five computers connected to a Hub.

If Computer A sends data to Computer E, every computer receives the frame—even though only one device actually needs it.

This causes:

* Wasted bandwidth
* Poor performance
* Security risks
* Increased network collisions

Engineers needed a smarter solution.

---

# The Solution

The solution was the **Network Switch**.

Unlike a Hub, a Switch does not blindly forward every frame.

Instead, it learns where devices are connected and forwards frames only to the correct destination port.

---

# What is a Network Switch?

A **Network Switch** is a Layer 2 (Data Link Layer) networking device that forwards Ethernet frames using **MAC Addresses**.

Its primary responsibility is to efficiently deliver frames to the correct device within the same Local Area Network (LAN).

---

# How Does a Switch Learn?

When a frame arrives, the switch reads the **Source MAC Address**.

It records:

* Source MAC Address
* Incoming Port

This information is stored in a **CAM (Content Addressable Memory) Table**.

Example:

| MAC Address | Port   |
| ----------- | ------ |
| AA-AA-AA-AA | Port 1 |
| BB-BB-BB-BB | Port 2 |
| CC-CC-CC-CC | Port 3 |

As devices communicate, the table grows automatically.

---

# Frame Forwarding

When another frame arrives, the switch checks the **Destination MAC Address**.

If the destination MAC exists in the CAM Table:

* The frame is forwarded only to the correct port.

If the destination MAC does not exist:

* The switch performs **Unknown Unicast Flooding**, sending the frame to every port except the incoming one.

Once the destination replies, the switch learns its MAC Address and updates the CAM Table.

---

# OSI Model Mapping

| Layer   | Topic                    |
| ------- | ------------------------ |
| Layer 3 | IP Address               |
| Layer 2 | Switch, MAC Address, ARP |
| Layer 1 | Ethernet Cable, Wi-Fi    |

The Switch operates primarily at **Layer 2 (Data Link Layer)**.

---

# Enterprise Perspective

Enterprise networks may contain thousands of devices.

Switches make communication efficient by forwarding traffic only where it needs to go.

They are commonly used to connect:

* Employee laptops
* Servers
* IP Phones
* Printers
* Wireless Access Points

Without switches, enterprise networks would become congested and inefficient.

---

# Cybersecurity Perspective

Switches improve security compared to hubs, but they can still be targeted.

Common attacks include:

* MAC Flooding
* CAM Table Exhaustion
* VLAN Hopping (later chapter)

Administrators often implement **Port Security** to restrict which MAC Addresses are allowed on specific switch ports.

---

# Common Misconceptions

## ❌ A Switch sends frames to every device.

False.

A Switch forwards frames only to the destination port once it has learned the destination MAC Address.

---

## ❌ A Hub and a Switch are the same.

False.

A Hub repeats traffic.

A Switch makes forwarding decisions using MAC Addresses.

---

## ❌ Switches use IP Addresses to forward frames.

False.

Traditional Layer 2 switches forward frames using **MAC Addresses**.

---

# Key Takeaways

* A Switch operates at Layer 2.
* It forwards frames using MAC Addresses.
* It learns device locations automatically.
* It stores learned addresses in the CAM Table.
* Unknown MAC Addresses cause frame flooding.
* Switches significantly improve network performance and security compared to Hubs.

---

# Summary

The invention of the Switch solved one of the biggest limitations of Hub-based networks.

By learning MAC Addresses and intelligently forwarding Ethernet frames, switches dramatically improved network performance, scalability, and security.

Understanding how switches work is essential before learning routers, VLANs, and enterprise network design.
