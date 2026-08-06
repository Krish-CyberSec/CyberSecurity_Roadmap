# Interview Questions

## 1. What happens when you type `google.com` into a browser?

When a user enters:

```text
https://google.com
```

the browser does **not** immediately connect to Google's servers.

Instead, it follows a sequence of steps to locate the server and establish communication.

### Process

```text
User
   ↓
Browser
   ↓
Browser Cache
   ↓
Operating System DNS Cache
   ↓
DNS Server
   ↓
Router
   ↓
Internet Service Provider (ISP)
   ↓
Internet
   ↓
Google Server
   ↓
Response
   ↓
Browser displays the webpage
```

Each component has a specific responsibility in delivering the request successfully.

---

## 2. Why do computers need DNS?

Computers communicate using **IP addresses**, not domain names. Humans remember names like **google.com**, but computers need an IP address such as **142.250.x.x** to locate a server.

The **Domain Name System (DNS)** translates a domain name into its corresponding IP address so the browser knows where to send the request.

Without DNS, users would have to remember IP addresses for every website they visit.

---

## 3. What is the role of a router?

A **router** connects your local network (home, school, or office) to other networks, including the Internet.

Its main responsibilities are:

- Forward data packets to the correct destination.
- Connect multiple devices to the Internet.
- Select the best available path for network traffic.
- Assign local IP addresses (when acting as a DHCP server).

---

## 4. What does an ISP do?

An **Internet Service Provider (ISP)** gives users access to the Internet.

An ISP is responsible for:

- Connecting your home or office to the Internet.
- Routing your traffic to other networks.
- Providing public IP addresses.
- Managing Internet infrastructure and bandwidth.

Examples include Airtel, Jio, BSNL, Verizon, and Comcast.

---

## 5. Why doesn't your browser always contact the DNS server?

The browser first checks whether it already knows the IP address.

It searches:

1. Browser DNS cache
2. Operating system DNS cache
3. Router cache

If the IP address is found, the browser skips contacting the DNS server, making websites load faster and reducing network traffic.

---

## 6. What is a cache?

A **cache** is temporary storage that keeps frequently used data so it can be accessed quickly.

Examples include:

- Browser cache (web pages, images, CSS, JavaScript)
- DNS cache (recent domain-to-IP mappings)
- CPU cache (frequently used instructions)

Benefits:

- Faster performance
- Reduced network traffic
- Lower server load

---

## 7. Why is DNS important for cybersecurity?

DNS plays a major role in Internet security because attackers often target it.

Common threats include:

- DNS spoofing (DNS cache poisoning)
- Phishing attacks
- Malicious domain redirection
- Malware communication

Security technologies like **DNSSEC** help verify that DNS responses are authentic and have not been altered.

---

## 8. Explain the journey of a packet from your laptop to Google.

The packet typically follows this path:

```text
Laptop
   ↓
Wi-Fi / Ethernet
   ↓
Router
   ↓
ISP
   ↓
Internet Backbone
   ↓
Google Network
   ↓
Google Server
```

The server processes the request and sends the response back through the same or a similar route until the webpage appears in the browser.

---

## 9. What does `tracert` show?

`tracert` (Windows) or `traceroute` (Linux/macOS) displays the path packets take from your computer to a destination server.

It shows:

- Every router (hop) along the path
- The IP address or hostname of each hop
- The round-trip time (latency) to each hop

Example:

```text
1  192.168.1.1
2  10.102.231.1
3  103.44.18.97
...
17  142.250.29.139
```

---

## 10. What are network hops?

A **network hop** is one stop a packet makes as it travels between routers on its way to the destination.

For example:

```text
Laptop
   ↓
Router (Hop 1)
   ↓
ISP Router (Hop 2)
   ↓
ISP Core Router (Hop 3)
   ↓
Google Router (Hop 4)
   ↓
Google Server (Hop 5)
```

Each router that forwards the packet counts as **one hop**.

More hops do not always mean slower performance, but additional hops can increase latency.
