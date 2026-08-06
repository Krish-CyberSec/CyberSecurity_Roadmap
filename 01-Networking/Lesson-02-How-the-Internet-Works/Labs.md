# Hands-on Lab

## Lab-01

```
nslookup google.com
```

1. Which DNS server answered?
   
   ```
   dns.google
   ```

2. Which IP address was returned?

   ```
          2404:6800:4013:813::66
          2404:6800:4013:813::65
          2404:6800:4013:813::71
          2404:6800:4013:813::8a
          192.178.158.100
          192.178.158.113
          192.178.158.139
          192.178.158.102
          192.178.158.101
          192.178.158.138
   ```

## Lab-02

```
ping google.com
```

   ```
   the hostname `google.com` is automatically converted into an IP address before any ICMP (Internet Control Message Protocol) packets are sent.

## Why does this happen?

Computers communicate using **IP addresses**, not domain names. Domain names like `google.com` are easy for humans to remember, but network devices use numerical IP addresses to identify each other.

Before `ping` can send packets, the operating system performs **DNS (Domain Name System) resolution** to translate the hostname into an IP address.

## How it works

1. You run:

   ```bash
   ping google.com
   ```

2. The operating system looks for the IP address by checking:
   - The local hosts file (e.g., `/etc/hosts` on Linux/macOS)
   - The DNS cache (if available)
   - The configured DNS server(s)

3. The DNS server returns an IP address, for example:

   ```
   google.com → 142.250.xxx.xxx
   ```

4. `ping` sends ICMP Echo Request packets to the resolved IP address.

### Example

```bash
$ ping google.com
PING google.com (142.250.183.206): 56 data bytes
```

Notice that the resolved IP address appears in parentheses.

### Process Flow

```text
ping google.com
        │
        ▼
   DNS Resolution
        │
        ▼
google.com → 142.250.183.206
        │
        ▼
ICMP Echo Request sent to 142.250.183.206
```

## Lab-03


```
tracert google.com
```

### output

<img width="847" height="416" alt="image" src="https://github.com/user-attachments/assets/24e1df64-a158-4e8a-8ed2-cc522f44af85" />


1. How many hops?

    17 hops (the destination is reached at hop 17)

2. Which hop belongs to your router?

   Hop 1 (192.168.1.1) — this is the local router.

3. Which hop likely belongs to your ISP?

   Hop 2 (10.102.231.1) — this is the first router inside the ISP's network. Hops 3–5 are also likely part of your ISP before the traffic enters Google's network.


## Lab-04 (Challenge)


Visit:

```
https://www.nslookup.io
```

Look up:

```
google.com

```

Observe:

1. IP addresses
2. DNS records  
3. CDN information


output:

<img width="1195" height="617" alt="image" src="https://github.com/user-attachments/assets/6b653d59-2728-4fce-9484-4828e1774878" />

   

   
