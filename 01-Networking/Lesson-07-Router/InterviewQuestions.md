# Chapter 07 – Interview Questions

## Beginner Level

### Q1. What is a Router?

**Answer:**

A Router is a Layer 3 networking device that forwards packets between different networks using IP Addresses.

---

### Q2. Why was the Router invented?

**Answer:**

Routers were invented because Switches can only communicate within the same Local Area Network. Routers enable communication between different networks.

---

### Q3. Which OSI Layer does a Router operate on?

**Answer:**

Layer 3 – Network Layer.

---

### Q4. What information does a Router use to forward packets?

**Answer:**

The Destination IP Address.

---

## Intermediate Level

### Q5. What is the Default Gateway?

**Answer:**

The Default Gateway is the router that serves as the exit point from a local network to other networks.

---

### Q6. Why doesn't a laptop perform ARP for Google's MAC Address?

**Answer:**

Because Google is outside the local network. The laptop performs ARP only for the MAC Address of the Default Gateway.

---

### Q7. What is the difference between a Switch and a Router?

**Answer:**

A Switch forwards Ethernet frames using MAC Addresses within a LAN, while a Router forwards IP packets between different networks using IP Addresses.

---

### Q8. What is a Routing Table?

**Answer:**

A Routing Table is a collection of routes that helps a router determine where to forward packets.

---

## Advanced Level

### Q9. Explain how a packet reaches Google after leaving your laptop.

**Answer:**

1. The laptop creates an IP packet.
2. It checks whether Google is on the same network.
3. Since it is not, the laptop performs ARP to find the Default Gateway's MAC Address.
4. The Switch forwards the frame to the Router.
5. The Router examines the destination IP Address.
6. The Router consults its Routing Table.
7. The packet is forwarded toward the ISP and eventually reaches Google's servers.

---

### Q10. Why are Routers important in enterprise networks?

**Answer:**

Routers connect multiple departments, branch offices, cloud networks, and external services while enforcing routing and security policies.

---

### Q11. Name three attacks that target Routers.

**Answer:**

* Route Hijacking
* Firmware Exploitation
* Unauthorized Administrative Access

---

## Scenario-Based Question

### Q12.

Your laptop successfully communicates with devices inside your office but cannot access websites on the Internet.

Which device would you investigate first?

**Answer:**

The Default Gateway (Router), because it is responsible for forwarding traffic outside the local network.

---

# Think Like an Engineer

Imagine the Internet had no Routers.

How would computers communicate across cities or countries?

Would the Internet still exist?

Explain your reasoning.
