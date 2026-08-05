# Lesson 01 - Lab

## Objective

Understand basic networking commands.

## Commands Used

### ipconfig

Purpose:
Display IP configuration of the local machine.

Observation:
- IPv4: 192.168.1.34
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DNS Server: NA

---

### ping google.com

Purpose: 
Check connectivity.

Observation:
- Packets Sent: 4
- Packets Received: 4
- Average Time: 3ms

---

### tracert google.com

Purpose:
See the path packets take.

Observation:
- Number of hops:
- Interesting findings:
```
| Hop   | Observation                                                                | Interpretation                                                                                                                                                                       |
| ----- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1     | `192.168.1.1`                                                              | Your local router. Normal.                                                                                                                                                           |
| 2     | `10.102.231.1`                                                             | ISP private network (CGNAT or internal routing). Normal.                                                                                                                             |
| 3     | `103.44.18.97`                                                             | First public ISP router. One probe took **116 ms**, while the others were **7 ms** and **2 ms**. This is likely ICMP rate-limiting or momentary congestion, not actual path latency. |
| 4     | `192.168.192.129`                                                          | Another private address inside your ISP. Private addresses within an ISP backbone are common.                                                                                        |
| 5     | `103.241.47.29`                                                            | One timeout, one successful reply. Router is forwarding traffic but not responding consistently to traceroute.                                                                       |
| 6–11  | Google backbone (`72.14.x.x`, `142.251.x.x`, `108.170.x.x`, `192.178.x.x`) | Traffic has entered Google's network. Latency remains very low (3–7 ms).                                                                                                             |
| 12–22 | `Request timed out.`                                                       | These routers are almost certainly configured not to reply to TTL-expired ICMP packets. Since later hops respond, these are **not packet-loss events**.                              |
| 23    | `lcdels-in-f139.1e100.net`                                                 | Destination reached successfully in about **3–4 ms**.                                                                                                                                |
```
---

## Conclusion

Today I learned how to identify my local network configuration and how packets travel across multiple devices before reaching a destination.
