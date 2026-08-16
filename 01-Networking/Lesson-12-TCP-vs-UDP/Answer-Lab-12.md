# Lab 01 — Identify the Transport Protocol

| Scenario      | TCP/UDP | Reason                                                                                                                    |
| ------------- | ------- | ------------------------------------------------------------------------------------------------------------------------- |
| File transfer | TCP     | Reliable, ordered delivery with retransmission of lost data.                                                              |
| Voice         | UDP     | Low latency is more important than perfect delivery; avoiding retransmission reduces delay.                               |
| DNS           | UDP     | Typical DNS queries use UDP for low overhead and fast request/response communication. TCP is also used in specific cases. |
| SSH           | TCP     | SSH requires reliable, ordered, connection-oriented communication.                                                        |
| HTTP/3        | UDP     | HTTP/3 uses QUIC, which runs over UDP while providing reliability, encryption, and stream management at the QUIC layer.   |

# Lab 02 — TCP Three-Way Handshake

```text
SYN
SYN-ACK
ACK
```

* **SYN:** The client requests to establish a TCP connection and sends its initial sequence number.
* **SYN-ACK:** The server acknowledges the client's SYN and sends its own SYN with its initial sequence number.
* **ACK:** The client acknowledges the server's SYN, completing the connection establishment.

# Lab 03 — Analyze a TCP Flow

```text
Source IP:        192.168.1.20
Source Port:      51544
Destination IP:   10.20.10.50
Destination Port: 443
Transport Protocol: TCP
Common destination service: HTTPS
```

`51544` is likely an ephemeral client port because it is a temporary high-numbered port selected by the operating system for the client side of the connection. It allows multiple simultaneous connections to the same or different destination services.

# Lab 04 — TCP Reliability

1. Segment 3 was lost or was not successfully received.
2. TCP can detect missing data using sequence numbers, acknowledgements, duplicate acknowledgements, and retransmission timers.
3. Sequence numbers identify the position of data within the TCP byte stream and allow the receiver to detect missing or out-of-order data.
4. Acknowledgements tell the sender which data has been successfully received.
5. TCP can retransmit missing data so the receiver can reconstruct the complete ordered byte stream.

# Lab 05 — Port Identification

| Port | Common Association | TCP/UDP/Common Use                      |
| ---: | ------------------ | --------------------------------------- |
|   22 | SSH                | TCP                                     |
|   23 | Telnet             | TCP                                     |
|   25 | SMTP               | TCP                                     |
|   53 | DNS                | UDP/TCP                                 |
|   80 | HTTP               | TCP                                     |
|  443 | HTTPS              | TCP; UDP is also used by QUIC/HTTP/3    |
|  445 | SMB                | TCP                                     |
| 3389 | RDP                | TCP; UDP can also be used by modern RDP |

Port numbers are common associations and do not prove which application is actually running.

# Lab 06 — Source vs Destination Port

```text
Source IP:        10.10.5.20
Source Port:      49152
Destination IP:   10.20.10.50
Destination Port: 443
```

`49152` is likely an ephemeral source port because it is a high-numbered temporary port dynamically selected by the client operating system for the connection.

# Lab 07 — TCP vs UDP Security Analysis

1. No. Neither flow is automatically malicious.
2. Flow B could legitimately be **QUIC/HTTP/3**, which commonly uses UDP port 443.
3. A SOC analyst should investigate:

   * Source and destination IP addresses
   * Destination domain or hostname
   * DNS activity
   * TLS/QUIC metadata where available
   * Connection frequency and volume
   * Process responsible for the traffic
   * User or host generating the connection
   * Destination reputation and ownership
   * Firewall and proxy logs
   * Endpoint security alerts
   * Whether the traffic matches expected application behavior

# Lab 08 — Port Scanning Scenario

1. This could represent network reconnaissance or port scanning.
2. The ports are interesting because they are commonly associated with services such as SSH, Telnet, HTTP, HTTPS, SMB, and RDP.
3. Examine:

   * Firewall logs
   * IDS/IPS alerts
   * Network flow logs
   * DNS logs
   * Endpoint/network security logs
   * Authentication logs
   * EDR telemetry
4. Identify the originating process using endpoint telemetry, EDR data, operating-system network connection information, and process/network correlation.
5. No. It could be legitimate security scanning, vulnerability assessment, asset discovery, or network administration. Context, authorization, timing, source, and process identity are required to determine whether it is malicious.

# Lab 09 — Firewall Rule Interpretation

The first rule permits TCP traffic from the `10.10.0.0/24` network to the `10.20.0.0/24` network when the destination port is `443`. This commonly permits HTTPS or another service using TCP port 443.

The second rule prevents TCP traffic from the `10.30.0.0/24` network to the `10.20.0.0/24` network when the destination port is `23`, thereby blocking Telnet connections from that source network to that destination network.

# Lab 10 — Wireshark Preparation

* **Source IP:** Identifies the IP address of the host sending the packet.
* **Destination IP:** Identifies the IP address of the intended receiving host.
* **Source Port:** Identifies the transport-layer port used by the sending application or connection endpoint.
* **Destination Port:** Identifies the transport-layer port associated with the receiving service or application endpoint.
* **TCP Flags:** Indicate TCP control states and functions such as connection establishment, acknowledgement, termination, and resetting.
* **Sequence Number:** Identifies the position of TCP data within the byte stream.
* **Acknowledgement Number:** Indicates the next sequence number the receiver expects, acknowledging received data.

# Lab 11 — Packet Tracer / Traffic Flow

**Layer 4:**
TCP or UDP, depending on the application being used.

**Layer 3:**
Source and destination IP addresses identify the communicating hosts across networks.

**Layer 2:**
Source and destination MAC addresses identify the local Layer 2 devices or interfaces used to deliver each Ethernet frame. MAC addresses can change at each routed hop.

**Layer 1:**
The physical or wireless medium carries the bits, such as Ethernet cable, fiber, or Wi-Fi radio.

# Lab 12 — Security Scenario

1. Possible service associations:

   * TCP 22 — SSH
   * TCP 80 — HTTP
   * TCP 443 — HTTPS or another service using TCP 443
   * UDP 53 — DNS

2. All services require validation against the actual application, configuration, ownership, and business requirement. Port numbers alone do not prove the running service.

3. Only services that have a documented business requirement should be exposed to the Internet. Internet-facing services should be minimized and appropriately secured.

4. Appropriate controls may include:

   * Restricting source networks where possible
   * Allowing only required destination ports
   * Network segmentation
   * Administrative access through VPN or controlled access paths
   * Strong authentication
   * Encryption
   * Rate limiting
   * IDS/IPS controls
   * Logging and monitoring
   * Regular firewall-rule review

5. Monitor connection attempts, successful connections, authentication events, unusual traffic volumes, scanning behavior, suspicious source addresses, application logs, DNS activity, and endpoint telemetry.

# Lab 13 — GRC Review Scenario

1. Questions:

   * What business requirement justifies each rule?
   * Who owns each service?
   * What systems require the communication?
   * What source and destination systems are involved?
   * Is the access documented and approved?
   * How frequently is the access reviewed?
   * Are there compensating security controls?
   * Is the rule broader than necessary?

2. The TCP 443, TCP 22, UDP 53, and UDP `50000-60000` rules all require business justification.

3. The UDP `50000-60000` range requires particular context because it permits a large range of ports. TCP 22 also requires careful review because SSH is commonly used for administrative access.

4. Compare the firewall rules with the approved service inventory, network diagrams, application requirements, access-control policy, change records, and security standards. Verify that every allowed communication has an approved business purpose and appropriate scope.

5. Request:

   * Firewall configuration
   * Approved firewall rules or rule matrix
   * Business-owner approvals
   * Change-management records
   * Network diagrams
   * Application/service inventory
   * Data-flow documentation
   * Access reviews
   * Firewall logs
   * Vulnerability-scan results
   * Exception/risk-acceptance records
   * Evidence of periodic rule reviews

# Final Assessment — Question 1

TCP uses a three-way handshake to establish a connection, synchronize sequence numbers between both endpoints, and confirm that both sides are ready to communicate.

# Final Assessment — Question 2

UDP does not need a TCP-style handshake because it is connectionless. It sends datagrams without first establishing a stateful connection between the endpoints.

# Final Assessment — Question 3

* **IP address:** Identifies a network interface or host at the network layer.
* **Port:** Identifies a transport-layer endpoint associated with an application or service.
* **Socket:** An endpoint used for network communication, commonly represented by an IP address and port together with the transport protocol.

# Final Assessment — Question 4

The combination of source IP, source port, destination IP, and destination port identifies a TCP connection endpoint pair. Together, these values form the TCP four-tuple and distinguish one connection from other simultaneous connections.

# Final Assessment — Question 5

UDP 443 can be legitimate because **QUIC**, which is used by HTTP/3, operates over UDP and commonly uses destination port 443. Therefore, UDP 443 can represent normal encrypted web traffic.

# Final Assessment — Question 6

Investigate:

* Source host and user
* Destination IP addresses
* Destination ports
* Timing and frequency
* SYN/ACK response patterns
* Whether the activity resembles a port scan
* Process responsible for generating the packets
* EDR telemetry
* Firewall and IDS/IPS logs
* DNS activity
* Destination ownership
* Whether authorized vulnerability scanning or security testing is occurring
* Any follow-up connections after successful probes

# Final Assessment — Question 7

Allowing TCP 443 while blocking TCP 23 permits commonly required HTTPS traffic while preventing Telnet traffic. Telnet provides insecure remote access because its communication is not protected by encryption by default. Blocking TCP 23 reduces exposure to an unnecessary and insecure remote-administration protocol.

# Lab Completion Checklist

* [x] I understand the purpose of Layer 4.
* [x] I can explain TCP vs UDP.
* [x] I understand the TCP three-way handshake.
* [x] I understand sequence numbers conceptually.
* [x] I understand acknowledgements.
* [x] I understand retransmission.
* [x] I understand source and destination ports.
* [x] I understand ephemeral ports.
* [x] I understand sockets and the TCP four-tuple.
* [x] I can interpret basic firewall rules.
* [x] I can reason about suspicious TCP and UDP activity.
* [x] I understand why UDP 443 can be legitimate.
* [x] I can connect transport concepts to SOC and GRC work.
