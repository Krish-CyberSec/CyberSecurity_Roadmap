# Interview Questions: IP Addresses

## 1. What is an IP address?

An **Internet Protocol (IP) address** is a unique logical address assigned to a device connected to a network.

It allows devices to identify each other and exchange information.

Think of it as the **digital address** of a device.

Just like your home has a postal address, every device connected to a network requires an IP address so data knows where to go.

---

## 2. Why do computers need IP addresses?

Computers need IP addresses because they allow devices to communicate with each other over a network.

Without an IP address:
- Devices cannot send or receive data.
- Routers would not know where to deliver information.
- Internet communication would not be possible.

**Example:**
When you open a website, your computer sends a request from your IP address to the website's IP address. The server then knows where to send the response.

---

## 3. What is the difference between an IP address and a MAC address?

| IP Address | MAC Address |
|------------|-------------|
| Logical address | Physical (hardware) address |
| Assigned by network or ISP | Assigned by the manufacturer |
| Can change | Usually permanent |
| Used for communication across networks | Used for communication within a local network |
| Works at Layer 3 (Network Layer) | Works at Layer 2 (Data Link Layer) |

### Easy analogy

- **IP Address** = Your home's mailing address
- **MAC Address** = Your fingerprint or house's unique serial number

---

## 4. What is IPv4?

**IPv4 (Internet Protocol Version 4)** is the fourth version of the Internet Protocol and is still the most widely used.

### Features

- 32-bit address
- Approximately **4.3 billion** unique addresses
- Written as four decimal numbers separated by dots

**Example**

```
192.168.1.10
```

### Limitation

Because the Internet has billions of connected devices, IPv4 addresses have largely been exhausted.

---

## 5. What is IPv6, and why was it introduced?

**IPv6 (Internet Protocol Version 6)** is the newer version of the Internet Protocol.

It was introduced because the world ran out of enough IPv4 addresses.

### Features

- 128-bit address
- Provides an enormous number of unique addresses
- Better security support
- Improved routing efficiency
- Designed for future Internet growth

**Example**

```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

### IPv4 vs IPv6

| IPv4 | IPv6 |
|------|------|
| 32-bit | 128-bit |
| ~4.3 billion addresses | Almost unlimited addresses |
| Dot-decimal notation | Hexadecimal notation |
| Older standard | Modern standard |

---

## 6. What is the difference between a public IP and a private IP?

### Public IP Address

A public IP address is globally unique and accessible over the Internet.

It is assigned by an Internet Service Provider (ISP).

**Example**

```
8.8.8.8
```

### Private IP Address

A private IP address is used inside local networks such as homes, schools, or offices.

It cannot be reached directly from the Internet.

Common private IP ranges:

```
10.0.0.0 – 10.255.255.255

172.16.0.0 – 172.31.255.255

192.168.0.0 – 192.168.255.255
```

### Comparison

| Public IP | Private IP |
|-----------|------------|
| Accessible from Internet | Only inside local network |
| Globally unique | Can be reused in different networks |
| Assigned by ISP | Assigned by router or DHCP server |

---

## 7. Why can't private IP addresses be used directly on the Internet?

Private IP addresses are reserved for internal networks.

Many different organizations use the same private IP ranges, so they are **not globally unique**.

If routers on the Internet accepted private IP addresses, they would not know which network the packet belongs to.

Instead, routers use **Network Address Translation (NAT)** to convert private IP addresses into a public IP before sending traffic to the Internet.

### Example

Your laptop:

```
192.168.1.20
```

Router (Public IP):

```
103.45.100.10
```

The router translates your private IP into its public IP so websites can respond correctly.

---

## 8. How does a security analyst use IP addresses during an investigation?

Security analysts use IP addresses to identify and investigate suspicious activity.

Common uses include:

- Identifying the source of attacks
- Tracking malicious login attempts
- Investigating phishing campaigns
- Finding communication with malicious servers
- Detecting unusual network behavior
- Correlating firewall, VPN, and server logs
- Performing geolocation analysis
- Blocking malicious IP addresses

### Example

If hundreds of failed login attempts originate from the same IP address, it may indicate a brute-force attack.

---

## 9. What information can and can't an IP address tell you?

### An IP address can tell you:

- Approximate geographic location
- Internet Service Provider (ISP)
- Country
- Region or city (approximate)
- Network ownership
- Whether the IP is public or private

### An IP address cannot tell you:

- Exact home address
- Person's name
- Phone number
- Passwords
- Device contents
- Exact GPS location

Additional information requires cooperation from the ISP and, in many cases, legal authorization.

---

## 10. Explain IP addresses to someone with no technical background.

Imagine you want to send a letter to your friend.

Your friend has a home address, so the postal service knows where to deliver the letter.

The Internet works in a similar way.

Every computer, phone, or smart device has an IP address that acts like its digital home address.

When you visit a website:

1. Your device sends a request from its IP address.
2. The website receives the request.
3. The website sends the response back to your IP address.
4. Your browser displays the webpage.

Without IP addresses, devices would not know where to send or receive information.

---

# Quick Interview Summary

- **IP Address:** Logical address used to identify devices on a network.
- **MAC Address:** Physical hardware address of a network interface.
- **IPv4:** 32-bit addressing system with about 4.3 billion addresses.
- **IPv6:** 128-bit addressing system created to solve IPv4 exhaustion.
- **Public IP:** Reachable over the Internet.
- **Private IP:** Used only inside local networks.
- **NAT:** Converts private IP addresses into public IP addresses.
- **Security Analysts:** Use IP addresses to investigate attacks, analyze logs, and identify suspicious activity.
```
