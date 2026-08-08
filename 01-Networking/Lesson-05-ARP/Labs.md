# Hands-on Lab

## Lab 1

Display the ARP Cache.

```cmd
arp -a
```

Observe:

- IP Address
- Physical Address
- Entry Type


<img width="708" height="403" alt="image" src="https://github.com/user-attachments/assets/9771c0a8-3806-4101-a17f-0a2b45c46ef9" />


---

## Lab 2

Ping another device on your local network.

```cmd
ping <IP Address>
```

Then execute:

```cmd
arp -a
```

Observe whether a new ARP entry has been added.


<img width="676" height="616" alt="image" src="https://github.com/user-attachments/assets/67fe11c7-9880-4740-a396-a2feed5055a9" />

---

## Lab 3

Capture ARP traffic using Wireshark.

Apply the filter:

```
arp
```

Identify:

- ARP Request
- ARP Reply

---
