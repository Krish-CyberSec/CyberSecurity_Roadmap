# LabAnswer.md

# Lesson 14 — TCP Flags & Connection States

---

## Lab 01 — Identify TCP Flags

| Flag | Purpose                                                                                 |
| ---- | --------------------------------------------------------------------------------------- |
| SYN  | Synchronize sequence numbers and initiate a TCP connection.                             |
| ACK  | Acknowledge received data or a TCP control segment.                                     |
| FIN  | Gracefully close one direction of a TCP connection.                                     |
| RST  | Immediately reset/abort a TCP connection or reject a connection attempt.                |
| PSH  | Indicate that buffered data should be pushed to the receiving application promptly.     |
| URG  | Indicate that the segment contains urgent data, with the urgent pointer identifying it. |

---

## Lab 02 — TCP Three-Way Handshake

```text
Client
   |
   | SYN
   v
Server
   |
   | SYN-ACK
   v
Client
   |
   | ACK
   v
Server
```

### Answers

1. The client enters **SYN-SENT** after sending SYN.
2. The server enters **SYN-RECEIVED** after receiving SYN and sending SYN-ACK.
3. After the final ACK, the connection reaches **ESTABLISHED**.

---

## Lab 03 — TCP Connection States

| State        | Meaning                                           |
| ------------ | ------------------------------------------------- |
| LISTEN       | Waiting for an incoming connection                |
| SYN-SENT     | Client has sent SYN and is waiting for a response |
| SYN-RECEIVED | Server has received SYN and sent SYN-ACK          |
| ESTABLISHED  | Active connection                                 |
| CLOSE-WAIT   | Remote side has closed its sending direction      |
| FIN-WAIT     | Local side has initiated termination              |
| LAST-ACK     | Waiting for final ACK after sending FIN           |
| TIME-WAIT    | Post-termination state                            |
| CLOSED       | Connection no longer exists                       |

---

## Lab 04 — Read the TCP Story

Given:

```text
SYN
SYN-ACK
ACK
PSH-ACK
ACK
PSH-ACK
ACK
FIN
ACK
FIN
ACK
```

### Answers

1. The client initiated the connection with **SYN**.
2. The connection was established after the **SYN → SYN-ACK → ACK** handshake.
3. Application data was exchanged in the **PSH-ACK** packets.
4. The connection was closed using **FIN/ACK** exchanges.
5. Yes. The FIN/ACK exchange indicates **graceful termination**.

---

## Lab 05 — Closed Port

Given:

```text
Scanner → SYN → Target
Target  → RST → Scanner
```

### Answers

1. RST suggests that the connection attempt was rejected or reset.
2. Yes, the host is likely reachable because it responded.
3. The port is likely **closed**.
4. No. RST by itself does not prove malicious activity. It can be normal TCP behavior.

---

## Lab 06 — SYN Scan

Given:

```text
Scanner → SYN
Target  → SYN-ACK
Scanner → RST
```

### Answers

1. This resembles a **TCP SYN scan**.
2. The scanner does not send the final ACK because it wants to avoid completing the TCP connection.
3. It can learn whether a port is likely **open**, **closed**, or potentially **filtered** based on the response.
4. TCP flags reveal the state of the connection and allow the analyst to distinguish connection attempts, responses, resets, and established communication.

---

## Lab 07 — SYN Flood Investigation

### Answers

1. The pattern could represent a **TCP SYN flood**.
2. The server's connection-tracking resources, backlog, CPU, memory, or network capacity could be affected.
3. Rule out legitimate traffic spikes, load testing, vulnerability scanners, application changes, misconfigured clients, and other network events.
4. Investigate source IPs, destination ports, SYN rate, SYN-ACK rate, completed connections, retransmissions, connection states, timestamps, packet captures, firewall/load-balancer logs, and server resource utilization.
5. Relevant controls include firewalls, IDS/IPS, DDoS protection, rate limiting, SYN cookies, load balancers, access controls, and monitoring/alerting.

---

## Lab 08 — RST Investigation

### Answers

1. RST indicates that the TCP connection was abruptly reset.
2. No. RST alone does not identify the exact cause.
3. Investigate application logs, process information, socket information, server logs, endpoint telemetry, packet captures, and system events.
4. Yes. A firewall, IDS/IPS, load balancer, or other security/network device could generate or cause the reset.
5. Yes. An application or operating system can reset a TCP connection.

---

## Lab 09 — FIN Investigation

### Answers

1. FIN indicates that a host wants to gracefully close its sending direction.
2. It represents **graceful termination**, assuming the exchange completes normally.
3. Two FINs are involved because TCP closes the two communication directions independently.
4. The first FIN closes only the sender's direction. The other direction can continue until the peer sends its own FIN.

---

## Lab 10 — TIME-WAIT Investigation

### Answers

1. No. TIME-WAIT is a normal TCP state and is not automatically malicious.
2. TIME-WAIT indicates that a TCP connection has been closed and the endpoint is waiting before the connection's identifiers can safely be reused.
3. High connection rates, short-lived HTTP connections, proxies, APIs, load balancers, port scanning, or other high-volume client/server activity can create many TIME-WAIT sockets.
4. It becomes operationally important if the number of sockets consumes significant resources, causes ephemeral-port exhaustion, increases memory usage, or contributes to connection failures.

---

## Lab 11 — CLOSE-WAIT Investigation

### Answers

1. CLOSE-WAIT means the remote peer has closed its sending direction, but the local application has not yet closed its side.
2. The **remote side** has already closed its sending direction.
3. A large number can indicate that an application is not properly closing sockets after receiving FINs, potentially causing resource exhaustion.
4. Inspect the owning process, application logs, socket tables, open file descriptors, process resource usage, application configuration, and relevant connection-handling behavior.

---

## Lab 12 — Packet Direction Analysis

### Answers

1. The **client** initiated the connection.
2. The **server** responded.
3. The **SYN-ACK** indicates that the server accepted the connection request and responded to the SYN.
4. The **PSH-ACK** from the client carries application data.
5. Normal data transfer commonly uses **ACK**, often combined with flags such as **PSH** when application data is being delivered promptly.

---

## Lab 13 — Wireshark Practice

Record the following from an authorized lab capture:

```text
Source IP: <IP of connection initiator>
Source Port: <ephemeral/client port>
Destination IP: <server IP>
Destination Port: <server port>
```

The connection should contain:

```text
SYN
SYN-ACK
ACK
```

A later **FIN** indicates graceful termination, while a **RST** indicates an abrupt reset.

The packet sequence should be checked to determine which endpoint initiated termination and whether the closing exchange completed.

---

## Lab 14 — Wireshark TCP Flags

Example:

| Packet | Flags   | Meaning                             |
| ------ | ------- | ----------------------------------- |
| 1      | SYN     | Connection initiation               |
| 2      | SYN-ACK | Server response and acknowledgement |
| 3      | ACK     | Final handshake acknowledgement     |
| 4      | PSH-ACK | Application data transfer           |
| 5      | FIN-ACK | Graceful connection termination     |

### Connection Story

The client initiated a TCP connection, the server responded, the three-way handshake completed, application data was transferred, and the connection was then gracefully terminated.

---

## Lab 15 — Port Scan Detection

### Answers

1. This resembles a **TCP port scan**, potentially combined with scanning multiple hosts.
2. It is suspicious because one workstation is sending SYNs to multiple ports across multiple destinations in a systematic pattern.
3. Yes. It could be legitimate vulnerability management, inventory, monitoring, penetration testing, or an approved security scanner.
4. Check the source asset owner, security-scanner inventory, maintenance windows, authorization records, destination ownership, timing, and expected scanning behavior.
5. Identify the source workstation's process using endpoint telemetry, EDR, process/network connection data, operating-system socket information, and security-tool logs.

---

## Lab 16 — SYN Scan vs SYN Flood

| Feature                 | SYN Scan                                                    | SYN Flood                                                                               |
| ----------------------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Primary goal            | Discover TCP port states                                    | Exhaust or stress TCP connection resources                                              |
| Typical traffic pattern | SYNs directed at selected ports/hosts, often systematically | Very high volume of SYNs, often creating many incomplete connections                    |
| Connection completion   | Usually intentionally incomplete                            | Many connections remain incomplete                                                      |
| Security impact         | Reconnaissance/discovery                                    | Availability/resource-exhaustion attack                                                 |
| Possible legitimate use | Authorized security scanning and testing                    | Legitimate load testing can resemble some aspects, but an actual SYN flood is malicious |

---

## Lab 17 — Security Investigation

### Answers

1. Identify the process using EDR, endpoint telemetry, process-to-network mappings, and operating-system connection information.
2. Determine whether destinations were internal, external, or both.
3. Determine whether the traffic targeted many ports or a limited set of ports.
4. Compare SYNs with SYN-ACKs, ACKs, RSTs, and connection states to determine completion rates.
5. Check whether the source is an approved security scanner or other authorized infrastructure.
6. Determine this from process identity, authorization, destination behavior, timing, traffic volume, connection outcomes, and supporting endpoint/network evidence. TCP SYN traffic alone is not enough to establish malicious behavior.

---

## Lab 18 — GRC Investigation

### Answers

1. Determine whether the activity was authorized.
2. Verify whether an approved security scanner or penetration-testing activity was involved.
3. Identify the control that detected the activity, such as IDS/IPS, SIEM, firewall monitoring, EDR, or network monitoring.
4. Determine whether availability, latency, connection success, or server resources were affected.
5. Verify which incident-response procedure was followed.
6. Confirm that the event, investigation, evidence, impact, actions, and outcome were documented.
7. Assess whether existing preventive and detective controls adequately addressed the risk.
8. Yes, if the investigation identifies control gaps, excessive impact, insufficient monitoring, unclear authorization, or other weaknesses.

---

## Lab 19 — TCP Flag Storytelling

### Sequence A

```text
SYN → SYN-ACK → ACK
```

**Interpretation:** Normal TCP three-way handshake; connection established.

**Confidence:** High.

**Additional evidence required:** Further packets to determine whether data was exchanged and how the connection ended.

---

### Sequence B

```text
SYN → RST
```

**Interpretation:** The connection attempt was reset or rejected; the port is likely closed.

**Confidence:** High for a reset response; exact cause requires more evidence.

**Additional evidence required:** Host/service state, packet capture, firewall logs, and application configuration.

---

### Sequence C

```text
SYN → SYN-ACK → RST
```

**Interpretation:** The target responded as though the port was open, but the initiating side reset the connection before completing the handshake.

**Confidence:** High.

**Additional evidence required:** Scanner/process identity, surrounding traffic, and endpoint/network logs.

---

### Sequence D

```text
FIN → ACK → FIN → ACK
```

**Interpretation:** Both sides gracefully closed their respective directions.

**Confidence:** High.

**Additional evidence required:** Additional packets if exact TCP state transitions need to be reconstructed.

---

### Sequence E

```text
Data → RST
```

**Interpretation:** Data was exchanged and the connection was then abruptly reset.

**Confidence:** High that a reset occurred.

**Additional evidence required:** Application logs, endpoint evidence, firewall/IDS logs, packet capture, and process information to determine why.

---

## Lab 20 — Final Integrated Scenario

### Conversation A — Client A → 443

```text
Connection state: ESTABLISHED during the successful handshake and data exchange.
Data transfer: Yes. Client A sent PSH-ACK application data and the server acknowledged it.
Termination: Graceful FIN/ACK termination.
```

### Conversation B — Client B → 22

```text
Likely port state: Closed or otherwise rejecting the connection.
Reason: The server responded to the SYN with RST, indicating that the connection attempt was reset rather than established.
```

### Conversation C — Client C → 443

```text
Likely activity: A connection attempt that was deliberately reset before completion, such as a SYN scan.
Why: The server sent SYN-ACK, indicating a response consistent with an open/listening port, and Client C immediately sent RST instead of completing the handshake.
```

---

## Lab 21 — Final Packet Analysis Challenge

Given:

```text
1. SYN
2. SYN-ACK
3. ACK
4. PSH-ACK
5. ACK
6. PSH-ACK
7. ACK
8. RST
```

### Answers

1. **Yes.** The SYN, SYN-ACK, and ACK indicate that the TCP connection was established.
2. **Yes.** PSH-ACK packets indicate application data was transferred.
3. **No.** There is no FIN/ACK termination sequence.
4. The connection ended with a **RST**, meaning it was abruptly reset.
5. Investigate endpoint/application logs, process information, firewall/IDS/load-balancer logs, packet capture context, connection states, timestamps, and the host that generated the RST to determine why the reset occurred.

---

# Lab Completion Checklist

* [ ] I can identify SYN.
* [ ] I can identify ACK.
* [ ] I can identify FIN.
* [ ] I can identify RST.
* [ ] I understand PSH.
* [ ] I understand URG.
* [ ] I understand LISTEN.
* [ ] I understand SYN-SENT.
* [ ] I understand SYN-RECEIVED.
* [ ] I understand ESTABLISHED.
* [ ] I understand FIN-WAIT.
* [ ] I understand CLOSE-WAIT.
* [ ] I understand LAST-ACK.
* [ ] I understand TIME-WAIT.
* [ ] I understand CLOSED.
* [ ] I can recognize the TCP three-way handshake.
* [ ] I can recognize graceful TCP termination.
* [ ] I can recognize reset behavior.
* [ ] I understand SYN scanning.
* [ ] I understand SYN flooding.
* [ ] I can read a basic TCP packet sequence.
* [ ] I practiced TCP analysis in Wireshark.
* [ ] I can investigate suspicious TCP behavior as a SOC analyst.
* [ ] I can explain TCP findings from a GRC perspective.

---

# Final Reflection

A TCP conversation containing SYN, SYN-ACK, and ACK shows that one host initiated a connection and the other accepted it, completing the three-way handshake.

Once the connection is established, data packets can flow between the two hosts, with ACKs confirming receipt.

If FIN and ACK packets then appear from both sides, they indicate that each direction of the connection is being closed normally.

The complete sequence therefore tells a story of:

```text
Connection Establishment
        ↓
Data Exchange
        ↓
Connection Termination
```

The exact application, purpose, and reason for the connection would still require additional information such as IP addresses, ports, application logs, endpoint evidence, and surrounding packet data.
