# Networking Labs

## Lab 1 — Routing Table

### Objective

Use Cisco Packet Tracer to view the router's routing table and observe connected routes, the default route, and interfaces.

### Command

```text
Router> show ip route
```

### Output

```text
Gateway of last resort is not set

C    192.168.1.0/24 is directly connected, FastEthernet0/0
C    192.168.2.0/24 is directly connected, FastEthernet1/0
```

### Observations

* **Connected Route 1:** `192.168.1.0/24` is directly connected through `FastEthernet0/0`.
* **Connected Route 2:** `192.168.2.0/24` is directly connected through `FastEthernet1/0`.
* **Default Route:** No default route is configured.
* **Interfaces:**

  * `FastEthernet0/0` → `192.168.1.0/24`
  * `FastEthernet1/0` → `192.168.2.0/24`

The message **"Gateway of last resort is not set"** indicates that the router does not currently have a default route.

---

## Lab 2 — Traceroute

### Objective

Use `tracert` to observe the path packets take from the computer to Google.

### Command

On Windows PowerShell:

```text
tracert google.com
```

### Result

The traceroute reached Google at hop 17:

```text
1     4 ms     6 ms     1 ms  192.168.1.1
2     3 ms     7 ms    11 ms  10.102.231.1
3     3 ms     2 ms     2 ms  103.44.18.97
4     3 ms     3 ms     *     192.168.192.129
5     *        *        *     Request timed out.
6     4 ms     3 ms     4 ms  72.14.198.180
7     3 ms     3 ms    10 ms  142.251.66.169
8     4 ms     4 ms     5 ms  192.178.83.226
9     3 ms     3 ms     3 ms  142.251.226.81
10    4 ms     4 ms     4 ms  142.251.194.218
...
17    3 ms     2 ms     3 ms  nw-in-f100.1e100.net [142.250.29.100]
```

### Questions and Answers

#### 1. What is the first hop?

The first hop is:

```text
192.168.1.1
```

This is the local router/default gateway.

#### 2. Why is it the first hop?

The computer sends traffic destined for another network through its **default gateway**. Since Google is outside the local `192.168.1.0/24` network, the computer forwards the packet to `192.168.1.1`.

#### 3. What does each additional hop represent?

Each additional hop represents a **router or Layer 3 device** that forwards the packet toward the destination.

Some hops displayed:

```text
Request timed out.
```

This does not necessarily indicate a network failure. Some routers do not respond to traceroute probes or may limit such responses.

The traceroute eventually reached Google's server:

```text
nw-in-f100.1e100.net [142.250.29.100]
```

at **hop 17**.

---

## Lab 3 — Simple Routing Table

### Objective

Create a simple routing table based on the network used in Lab 1.

### Routing Table

| Network        | Interface       |
| -------------- | --------------- |
| 192.168.1.0/24 | FastEthernet0/0 |
| 192.168.2.0/24 | FastEthernet1/0 |
| 0.0.0.0/0      | Not configured  |

### Network Diagram

```text
          192.168.1.0/24
                |
         FastEthernet0/0
                |
             [Router]
                |
         FastEtheRouter0
                |
          192.168.2.0/24
```

### Observation

There is currently **no default route** because the router reports:

```text
Gateway of last resort is not set.
```

Therefore, `0.0.0.0/0` should not be assigned to an ISP unless a default route is actually configured.

---

# Summary

| Lab   | Topic                 | Main Result                                          |
| ----- | --------------------- | ---------------------------------------------------- |
| Lab 1 | Routing Table         | Two directly connected networks                      |
| Lab 2 | Traceroute            | First hop is `192.168.1.1`; Google reached at hop 17 |
| Lab 3 | Routing Table Diagram | Two connected networks and no default route          |

## Key Concepts

* **Connected route (`C`)** — A network directly connected to a router interface.
* **Default route (`0.0.0.0/0`)** — Used when no more specific route is available.
* **Default gateway** — The Router a host uses to reach networks outside its local network.
* **Hop** — A router or Layer 3 device traversed by a packet.
* **Traceroute** — A tool used to identify the path packets take to a destination.
