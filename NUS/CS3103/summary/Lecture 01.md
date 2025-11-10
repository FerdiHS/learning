# Lecture 01: Introduction

## Internet Protocol Layers (quick mental model)

- **Application:** protocols like HTTP, SMTP, DNS, DHCP create/consume messages.
- **Transport:** process-to-process delivery.
    - **TCP** (reliable, ordered, connection-oriented).
    - **UDP** (lightweight, no guarantees).
- **Network (IP):** best-effort packet delivery between hosts across networks; addressing + routing.
- **Link (e.g., Ethernet/Wi‑Fi):** hop-to-hop delivery inside a LAN using MAC addresses; carries IP as payload.
- **Physical:** bits on wire/fiber/radio.

> Mental demux chain: Ethernet frame → IP packet → UDP/TCP segment → application message.
> 

---

## How Internet Works

1. **Get an address: DHCP**
    
    Client broadcasts **DHCPDISCOVER**, receives **DHCPOFFER**, sends **DHCPREQUEST**, gets **DHCPACK** with: **IP**, **netmask**, **default gateway**, **DNS server**, **lease**.
    
2. **Find the gateway’s MAC: ARP**
    
    To send off-LAN, the host must deliver the first hop to the default gateway’s **MAC**: ARP **broadcast** asks “Who has X.X.X.X?”; router replies **unicast** with its MAC; host caches it.
    
3. **Resolve the name: DNS**
    
    Send a DNS query (usually **UDP**/53) to the configured resolver to map `www.example.com → A/AAAA`address(es).
    
4. **Fetch content: HTTP over TCP**
    
    Open a TCP connection to the server IP (via the gateway and Internet routing) and issue HTTP requests; receive responses; render.
    

---

## DNS

**Domain Name System**: distributed, hierarchical naming system + protocol that maps **names ↔ IPs** (A/AAAA) and other records (CNAME, MX, NS, TXT). Clients query a **recursive resolver** (often learned via DHCP). Typically **UDP/53**; uses **TCP** for large responses or zone transfers.

**Dynamic Host Configuration Protocol** automatically provides IP configuration to hosts. **Application layer** over **UDP**(server **67**, client **68**).

**DORA**:

- **D**ISCOVER (client → broadcast)
- **O**FFER (server → offers config)
- **R**EQUEST (client → selects/asks)
- **A**CK (server → confirms lease: IP, mask, gateway, DNS, lease time)

Renewal happens before lease expiry; NAK/DECLINE/RELEASE handle conflict/error paths.

- How UDP knows it’s DHCP?
    
    Via **well-known ports** — DHCP server **UDP/67**, DHCP client **UDP/68**. The 4‑tuple (src/dst IPs, src/dst ports) demuxes to the DHCP socket.
    

---

## ARP

**Address Resolution Protocol** resolves **IPv4 → Ethernet MAC** on a LAN.

- **Request:** broadcast “Who has 192.168.1.1? Tell 192.168.1.10.”
- **Reply:** unicast “192.168.1.1 is at 00:11:22:33:44:55.”
- Entries are cached; time out; gratuitous ARP can announce/refresh.
- How Ethernet knows it’s IP?
    
    via the **EtherType** field in the Ethernet frame. `0x0800` = IPv4, `0x86DD` = IPv6, `0x0806` = ARP.
    

---

## IP & Subnetting

- **IPv4 address** = 32 bits. Traditionally described as **network part** + **host part**; the **subnet mask / prefix** tells you where the split is (e.g., `/24` ≡ `255.255.255.0`).
- **CIDR (classless)** is modern practice: arbitrary prefixes (e.g., `10.1.32.0/21`) for flexible allocation and **route aggregation** (supernetting).
- **Subnetting**: “borrow” host bits to create subnets; each subnet reserves the **network** (all-zero host) and **broadcast**(all-ones host) addresses.

### Classful addressing (historical, but useful for intuition)

Default masks and first‑octet ranges:

| Class | First octet range | Default mask | Typical prefix |
| --- | --- | --- | --- |
| A | 0–127* | 255.0.0.0 | /8 |
| B | 128–191 | 255.255.0.0 | /16 |
| C | 192–223 | 255.255.255.0 | /24 |
- Notes: `0.x.x.x` historically “this network”; **127.x.x.x** reserved for loopback (e.g., 127.0.0.1). Practically, usable Class A first octets were 1–126.
- **Private IPv4** (non‑routable on the public Internet; used with NAT):
    - `10.0.0.0/8`
    - `172.16.0.0/12` (172.16–172.31)
    - `192.168.0.0/16`
- How IP knows it’s UDP?
    
    The IPv4 **Protocol** header field identifies the next layer. Common values: **17 = UDP**, **6 = TCP**, **1 = ICMP**. (IPv6 uses **Next Header**.)
    

---

## Glossary

- **RIP (Routing Information Protocol)** — distance‑vector **IGP (Interior Gateway Protocol)** (hop count metric). Simple; small networks; slow convergence.
- **OSPF (Open Shortest Path First)** — **link‑state IGP**. Floods **LSAs (Link‑State Advertisements)**; Dijkstra **SPF (Shortest Path First)**; supports areas; fast convergence.
- **IS‑IS (Intermediate System to Intermediate System)** — **link‑state IGP** widely used by providers; similar role to OSPF.
- **BGP (Border Gateway Protocol)** — inter‑domain **EGP (Exterior Gateway Protocol)** **path‑vector** protocol connecting Autonomous Systems; carries Internet‑wide routes; policy‑driven.
