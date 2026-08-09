**Why did engineers replace Hubs with Switches?**

One thing I've started noticing while learning networking is that every technology exists because it solves a real problem.

This week, I learned about **Network Switches**, and the reason behind their invention was surprisingly simple.

Imagine you're in a classroom.

You want to hand a notebook to one classmate.

Would you:

* Give a copy to every student in the room?

Or

* Hand it directly to the person who needs it?

Early networks used **Hubs**, which behaved like the first option—they forwarded every frame to every connected device.

That created several problems:

* Unnecessary network traffic
* Reduced performance
* Increased collisions
* Poor security, since every device could potentially inspect traffic that wasn't intended for it

Engineers solved this by introducing the **Network Switch**.

Instead of broadcasting everything, a switch learns where devices are connected by building a **CAM (Content Addressable Memory) Table**.

When a frame arrives, the switch:

1. Learns the sender's MAC Address.
2. Stores it with the corresponding port.
3. Checks the destination MAC Address.
4. Forwards the frame only to the correct port.

What I found interesting is that a switch doesn't know every device immediately—it learns over time as devices communicate.

From a cybersecurity perspective, this also introduces new attack surfaces.

For example, attackers can attempt **MAC Flooding**, where thousands of fake MAC Addresses are sent to overwhelm the switch's CAM Table and disrupt normal forwarding behavior.

Every networking concept I've learned so far has answered one question while creating another:

* IP Address → *Where should data go?*
* MAC Address → *Which device should receive it?*
* ARP → *How do we discover the destination MAC Address?*
* Switch → *How does the frame reach the correct device?*

I'm documenting my entire cybersecurity learning journey—including notes, labs, diagrams, interview questions, and practical exercises—in a public GitHub repository. The goal is to build an open knowledge base that anyone beginning cybersecurity can learn from.

**Question for the community:**

What's one networking concept that completely changed the way you understood how computers communicate?
