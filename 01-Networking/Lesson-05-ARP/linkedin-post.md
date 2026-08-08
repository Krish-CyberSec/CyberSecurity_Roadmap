**One thing that completely changed how I think about networking...**

When I first learned about IP addresses, I thought they were enough for devices to communicate.

If my laptop already knows another device's IP address, why does it need anything else?

Then I learned about **MAC addresses** and **ARP (Address Resolution Protocol)**.

Here's what I realized.

An **IP address** tells us *where* a device is on the network.

A **MAC address** tells us *which physical network interface* should receive the data on the local network.

But this creates another question:

> **If my laptop only knows the destination IP address, how does it discover the destination MAC address?**

That's exactly the problem ARP solves.

A simplified flow looks like this:

```
Know Destination IP
        │
        ▼
Check ARP Cache
        │
   Found? ── Yes ──► Send Data
        │
       No
        ▼
Broadcast:
"Who has this IP?"
        │
        ▼
Target Device Replies
with its MAC Address
        │
        ▼
Update ARP Cache
        │
        ▼
Send the Actual Data
```

While learning ARP, I found something even more interesting.

ARP was designed for simplicity—not security.

It trusts every ARP reply it receives.

That design decision is exactly why attacks like **ARP Spoofing (ARP Poisoning)** are possible, allowing attackers to intercept traffic on a local network through Man-in-the-Middle (MITM) attacks.

This reminded me of something important:

> Understanding **why** a protocol was designed often explains **why** it can be attacked.

That's the mindset I'm trying to develop as I learn cybersecurity.

I'm documenting every concept, lab, interview question, and diagram in a public GitHub repository. My goal is to build an open cybersecurity knowledge base that anyone starting their cybersecurity journey can use to learn alongside me.

**Question for the community:**

Which networking concept made you stop and think, *"Now I finally understand how the Internet actually works."*
