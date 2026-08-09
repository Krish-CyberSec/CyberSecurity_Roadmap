# Chapter 06 – Interview Questions

## Beginner Level

### Q1. What is a Network Switch?

**Answer:**

A Network Switch is a Layer 2 networking device that forwards Ethernet frames using MAC Addresses.

---

### Q2. Why was the Switch invented?

**Answer:**

Switches were invented to overcome the limitations of Hubs, which broadcast every frame to all connected devices.

---

### Q3. Which OSI Layer does a Switch operate on?

**Answer:**

Layer 2 – Data Link Layer.

---

### Q4. What information does a Switch use to forward frames?

**Answer:**

The Destination MAC Address.

---

## Intermediate Level

### Q5. What is a CAM Table?

**Answer:**

A CAM (Content Addressable Memory) Table stores MAC Address to Port mappings learned by the switch.

---

### Q6. How does a Switch learn MAC Addresses?

**Answer:**

By reading the Source MAC Address of incoming frames and recording the associated switch port.

---

### Q7. What happens if the destination MAC Address is not in the CAM Table?

**Answer:**

The Switch performs Unknown Unicast Flooding, forwarding the frame to all ports except the incoming port.

---

### Q8. Why is a Switch more secure than a Hub?

**Answer:**

A Switch sends traffic only to the intended destination port, reducing unnecessary exposure of network traffic.

---

## Advanced Level

### Q9. Explain the complete frame forwarding process.

**Answer:**

1. A frame arrives at the switch.
2. The switch learns the Source MAC Address.
3. It checks the Destination MAC Address.
4. If found in the CAM Table, it forwards the frame to the correct port.
5. If not found, it floods the frame.
6. After receiving a reply, it updates the CAM Table.

---

### Q10. What is MAC Flooding?

**Answer:**

MAC Flooding is an attack where an attacker sends thousands of fake MAC Addresses to fill the CAM Table, potentially forcing the switch to flood traffic.

---

### Q11. What is Port Security?

**Answer:**

Port Security is a switch feature that restricts which MAC Addresses are allowed on a particular switch port.

---

## Scenario-Based Questions

### Q12.

Your switch receives a frame whose destination MAC Address is unknown.

What will it do?

**Answer:**

It floods the frame to every port except the incoming port. Once the destination responds, the switch learns the MAC Address and updates its CAM Table.

---

### Q13.

Why do enterprise networks prefer switches instead of hubs?

**Answer:**

Because switches provide better performance, reduced collisions, improved security, efficient bandwidth usage, and intelligent forwarding.

---

# Think Like an Engineer

Imagine a switch had no CAM Table.

How would it know where to send frames?

Would it behave differently from a Hub?

What impact would that have on performance and security?
