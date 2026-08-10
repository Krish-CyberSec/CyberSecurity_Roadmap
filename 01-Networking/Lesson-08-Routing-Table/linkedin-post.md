**How does a router know where to send a packet?**

While learning about routers, I initially thought the router simply "knows" where every destination is.

But that immediately raises a bigger question:

If a router receives a packet destined for `8.8.8.8`, how does it decide where to send it next?

The answer is the **Routing Table**.

A routing table contains information about reachable networks and how traffic should be forwarded toward them.

A simplified decision process looks like this:

```text
Packet arrives
      |
      v
Read destination IP
      |
      v
Search Routing Table
      |
      v
Matching route?
   /        \
 Yes        No
  |          |
  v          v
Forward    Default
using      Route?
route
```

A route can provide information such as:

* Destination network
* Prefix
* Next hop
* Exit interface
* Route type

For example:

```text
Destination: 192.168.2.0/24
Exit Interface: G0/1
```

The router can use this information to forward traffic toward that network.

And then there's the route that caught my attention:

```text
0.0.0.0/0
```

The **default route**.

At a high level, it acts as a catch-all route when a more specific route isn't available.

What I found particularly interesting is how this connects with everything I've learned so far:

**IP Address**

"What network am I trying to reach?"

↓

**Router**

"I need to send this packet toward another network."

↓

**Routing Table**

"Here's the route I should use."

↓

**Next Hop / Exit Interface**

"This is where the packet goes next."

This also made me realise why routing isn't only a networking topic.

For a SOC analyst, unexpected routing behaviour can be a clue for investigation.

For a security engineer, routing affects network architecture and segmentation.

For a GRC professional, understanding routing helps when reviewing network diagrams, security boundaries, access paths, and configuration changes.

I'm documenting this entire learning journey in GitHub—concepts, diagrams, labs, interview questions, and the mistakes I make along the way.

The goal isn't just to collect certifications or memorise definitions.

It's to understand **why the technology works the way it does**.

The repository is being built step by step so that anyone learning cybersecurity can follow the same path and learn from it.

**Next question I'm exploring:**

What happens when a router has multiple possible routes to the same destination?

That leads into route selection, longest-prefix matching, metrics, and routing protocols.

I'm documenting the journey here:


