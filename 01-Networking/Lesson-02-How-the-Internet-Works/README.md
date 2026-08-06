# Lesson 02 - How the Internet Works

## Module Information

- **Module:** Networking for Cybersecurity
- **Difficulty:** Beginner
- **Estimated Study Time:** 90–120 Minutes
- **Status:** Completed
- **Prerequisites:** Lesson 01 - What is a Computer Network?

---

# Learning Objectives

After completing this lesson, you should be able to:

- Explain what happens when a user types a website URL into a browser.
- Understand why computers use IP addresses instead of domain names.
- Explain the purpose of DNS.
- Understand the role of routers and Internet Service Providers (ISPs).
- Explain the complete journey of a request from a user's computer to a web server.
- Understand why this process is important in cybersecurity.

---

# Introduction

Every day we open websites like Google, YouTube, LinkedIn, or GitHub within seconds.

Most people simply type a website name into their browser and never think about what happens behind the scenes.

However, before a webpage appears on your screen, several devices and services work together to deliver the requested information.

Understanding this communication process is one of the most important networking concepts because almost every cyberattack begins somewhere along this path.

This lesson explains that journey from the moment a user enters a website address until the webpage is displayed.

---

# What Happens When You Type "google.com"?

When a user enters:

https://google.com

the browser does **not** immediately connect to Google's servers.

Instead, it follows a sequence of steps to locate the server and establish communication.

The high-level process is:

User

↓

Browser

↓

Browser Cache

↓

Operating System Cache

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

Each component has a specific responsibility in delivering the request successfully.

---

# Step 1 - User Enters a URL

The process starts when a user enters a website address into the browser.

Example:

https://google.com

This address is called a URL (Uniform Resource Locator).

Humans prefer domain names because they are easy to remember.

Computers, however, communicate using IP addresses.

---

# Step 2 - Browser Cache

Before contacting the Internet, the browser checks whether it already knows Google's IP address.

If the information exists in its cache, it uses the stored address instead of performing another lookup.

This makes browsing faster.

---

# Step 3 - Operating System Cache

If the browser does not have the required information, the operating system checks its own DNS cache.

If found, communication continues immediately.

Otherwise, the request moves to the DNS server.

---

# Step 4 - DNS Server

The DNS (Domain Name System) translates the human-readable domain name into an IP address.

Example:

google.com

↓

142.xxx.xxx.xxx

Without DNS, users would need to remember numerical IP addresses instead of website names.

---

# Step 5 - Router

Once the IP address is known, the computer sends the request to the default gateway (router).

The router's responsibility is to forward the request toward the correct destination.

---

# Step 6 - Internet Service Provider (ISP)

The router forwards the packet to the Internet Service Provider.

Examples include:

- Airtel
- Jio
- BSNL
- ACT

The ISP connects users to the global Internet.

---

# Step 7 - Internet

The packet travels across multiple routers before reaching Google's infrastructure.

Every router that forwards the packet is called a hop.

This is why the tracert command displays multiple hops.

---

# Step 8 - Google Server

After reaching Google's server, the server processes the request and prepares the webpage.

---

# Step 9 - Response

The response travels back through the Internet, ISP, router, and finally reaches the user's browser, where the webpage is displayed.

---

# Why Is This Important in Cybersecurity?

Security professionals must understand this communication path because attackers often exploit different stages of it.

Examples include:

- DNS Spoofing
- DNS Tunneling
- Man-in-the-Middle (MITM)
- Rogue Access Points
- Route Hijacking
- Packet Sniffing

Without understanding how communication works, investigating attacks becomes significantly more difficult.

---

# Real-World Example

Imagine opening LinkedIn.

The browser first determines the server's IP address through DNS.

The request then passes through your router, ISP, and multiple Internet routers before reaching LinkedIn's servers.

Within milliseconds, LinkedIn processes the request and sends the webpage back to your device.

This entire communication process happens every time you access a website.

---

# Key Takeaways

- Computers communicate using IP addresses.
- Humans use domain names.
- DNS translates domain names into IP addresses.
- Routers forward packets toward their destination.
- ISPs connect users to the Internet.
- Multiple routers (hops) are involved in packet delivery.
- Understanding this communication path is essential for cybersecurity professionals.

---

# Commands Used

nslookup google.com

ping google.com

tracert google.com

---

# Summary

Whenever a user enters a website address, the browser performs several steps before displaying the webpage.

The browser checks cached information, queries the DNS server if necessary, forwards the request through the router and ISP, sends it across the Internet, and finally receives a response from the destination server.

Understanding this process forms the foundation for learning DNS, HTTP, HTTPS, routing, and many cybersecurity concepts in later lessons.
