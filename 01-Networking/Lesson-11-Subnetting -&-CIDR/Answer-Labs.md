# Lab 01 - Understand Host Bits

| CIDR | Host Bits |
|---|---:|
| `/24` | 8 |
| `/25` | 7 |
| `/26` | 6 |
| `/27` | 5 |
| `/28` | 4 |

---

# Lab 02 - Calculate Total and Usable Addresses

| CIDR | Host Bits | Total Addresses | Usable Hosts |
|---|---:|---:|---:|
| `/24` | 8 | 256 | 254 |
| `/25` | 7 | 128 | 126 |
| `/26` | 6 | 64 | 62 |
| `/27` | 5 | 32 | 30 |
| `/28` | 4 | 16 | 14 |

---

# Lab 03 - Find Network and Broadcast Addresses

## Exercise 1

Network: `192.168.50.64`  
First Host: `192.168.50.65`  
Last Host: `192.168.50.94`  
Broadcast: `192.168.50.95`

## Exercise 2

Network: `192.168.10.144`  
First Host: `192.168.10.145`  
Last Host: `192.168.10.158`  
Broadcast: `192.168.10.159`

## Exercise 3

Network: `192.168.50.128`  
First Host: `192.168.50.129`  
Last Host: `192.168.50.158`  
Broadcast: `192.168.50.159`

## Exercise 4

Network: `172.16.35.192`  
First Host: `172.16.35.193`  
Last Host: `172.16.35.206`  
Broadcast: `172.16.35.207`

---

# Lab 04 - Block Size

| CIDR | Subnet Mask | Block Size |
|---|---|---:|
| `/25` | `255.255.255.128` | 128 |
| `/26` | `255.255.255.192` | 64 |
| `/27` | `255.255.255.224` | 32 |
| `/28` | `255.255.255.240` | 16 |
| `/29` | `255.255.255.248` | 8 |

---

# Lab 05 - Subnetting in a Different Octet

IP: `172.16.50.10/20`

Subnet Mask: `255.255.240.0`

Interesting Octet: `3rd octet`

Block Size: `16`

Network Address: `172.16.48.0`

Broadcast Address: `172.16.63.255`

Usable Host Range: `172.16.48.1 - 172.16.63.254`

---

# Lab 06 - Host Requirement

| Hosts Needed | Smallest Prefix | Total Addresses | Usable Hosts |
|---:|---|---:|---:|
| 50 | `/26` | 64 | 62 |
| 100 | `/25` | 128 | 126 |
| 25 | `/27` | 32 | 30 |
| 10 | `/28` | 16 | 14 |
| 5 | `/29` | 8 | 6 |

---

# Lab 07 - VLSM Design

Starting Network: `192.168.50.0/24`

| Department | Hosts Needed | Prefix | Network | First Host | Last Host | Broadcast |
|---|---:|---|---|---|---|---|
| Web Servers | 60 | `/26` | `192.168.50.0` | `192.168.50.1` | `192.168.50.62` | `192.168.50.63` |
| Application | 25 | `/27` | `192.168.50.64` | `192.168.50.65` | `192.168.50.94` | `192.168.50.95` |
| Database | 10 | `/28` | `192.168.50.96` | `192.168.50.97` | `192.168.50.110` | `192.168.50.111` |
| Management | 5 | `/29` | `192.168.50.112` | `192.168.50.113` | `192.168.50.118` | `192.168.50.119` |

Unused Address Space:

`192.168.50.120 - 192.168.50.255`

---

# Lab 08 - VLSM Security Architecture

Starting Network: `192.168.100.0/24`

| Department | Hosts Needed | Prefix | Network | First Host | Last Host | Broadcast |
|---|---:|---|---|---|---|---|
| Engineering | 100 | `/25` | `192.168.100.0` | `192.168.100.1` | `192.168.100.126` | `192.168.100.127` |
| HR | 50 | `/26` | `192.168.100.128` | `192.168.100.129` | `192.168.100.190` | `192.168.100.191` |
| Finance | 20 | `/27` | `192.168.100.192` | `192.168.100.193` | `192.168.100.222` | `192.168.100.223` |
| Security | 10 | `/28` | `192.168.100.224` | `192.168.100.225` | `192.168.100.238` | `192.168.100.239` |

Remaining Address Space:

`192.168.100.240 - 192.168.100.255`

1. Largest network: Engineering
2. Network allocated first: Engineering `/25`
3. Address space remaining: 16 addresses
4. Networks non-overlapping: Yes
5. Can these subnets be used as security zones: Yes
6. Additional security controls:
   - Firewalls
   - ACLs
   - VLANs
   - Routing controls
   - IDS/IPS
   - Network monitoring
   - Access controls
   - Logging

---

# Lab 09 - Packet Tracer Application

Network 1: `192.168.1.0/24`

Network 2: `192.168.2.0/24`

Command:

`show ip route`

Network prefixes:

- `192.168.1.0/24`
- `192.168.2.0/24`

Subnet Mask:

`255.255.255.0`

Directly Connected Routes:

- `192.168.1.0/24`
- `192.168.2.0/24`

The router treats them as different networks because they have different network prefixes.

---

# Lab 10 - Security Investigation Scenario

Source: `10.40.20.55`

Destination: `10.20.10.10`

1. Source network: `10.40.0.0/16` - Guest Network
2. Destination network: `10.20.0.0/16` - Server Network
3. Should Guest devices normally communicate with Servers: No
4. Network controls to investigate:
   - Firewall rules
   - ACLs
   - Router configuration
   - VLAN configuration
   - Routing tables
   - Network segmentation
5. Could routing or firewall configuration be involved: Yes
6. Logs to request:
   - Firewall logs
   - Router logs
   - IDS/IPS logs
   - Authentication logs
   - DHCP logs
   - DNS logs
   - Endpoint logs
   - NetFlow/network traffic logs

---

# Lab 11 - GRC Architecture Review

Answer: No.

Different subnets alone do not prove that Guest users are isolated from Production systems.

Consider:

- Subnetting
- Routing
- Firewall
- ACL
- VLAN
- Monitoring
- Change Management

Different subnets provide logical separation, but routing and security controls must prevent unauthorized communication between them.

---

# Lab 12 - Final Subnetting Challenge

## A

IP: `192.168.10.77/26`

Network: `192.168.10.64`

Broadcast: `192.168.10.127`

Usable Range: `192.168.10.65 - 192.168.10.126`

## B

IP: `192.168.10.145/28`

Network: `192.168.10.144`

Broadcast: `192.168.10.159`

Usable Range: `192.168.10.145 - 192.168.10.158`

## C

IP: `172.16.35.200/28`

Network: `172.16.35.192`

Broadcast: `172.16.35.207`

Usable Range: `172.16.35.193 - 172.16.35.206`

## D

60 usable hosts:

`/26`

## E

10 usable hosts:

`/28`

---

# Final Reflection

Subnetting divides one large IP network into multiple smaller logical networks called subnets. This provides network segmentation and allows organizations to separate employees, servers, databases, and guest devices into different networks. Firewalls, ACLs, VLANs, and routing rules can control communication between these networks.

Subnetting is useful for cybersecurity because it can isolate compromised systems, limit lateral movement, reduce the attack surface, improve access control, and make network monitoring easier.

Subnetting alone is not a security mechanism. It becomes a security benefit when combined with firewalls, VLANs, ACLs, routing rules, monitoring, and access controls.
