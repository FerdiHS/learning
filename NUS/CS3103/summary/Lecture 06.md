# Lecture 06: Wireless LAN

A concise, exam‑ready Markdown summary of Wi‑Fi architecture, MAC (CSMA/CA), key procedures (scan/auth/assoc), frame fields, reliability, rate control, PHY/feature evolution (n/ac/ax), and security (WPA2/WPA3).

---

## Big picture

- **Wireless is shared & half‑duplex** → no collision **detection** like Ethernet; instead Wi‑Fi uses **CSMA/CA** with **ACKs** and **retries**.
- **Two senses of carrier**: **physical** (energy detect / preamble decode) and **virtual** (NAV from Duration/ID field). Both must be idle before transmit.
- **Infrastructure vs ad‑hoc**: most deployments are **STA ↔ AP** (infrastructure/BSS); ad‑hoc (IBSS) is rare.

---

## Architecture & channels

- **STA** (client), **AP** (access point), **BSS** (one AP + stations), **ESS** (many BSSs with common SSID), **BSSID** (AP’s MAC used as BSS ID), **SSID** (network name), **DS** (distribution system behind APs).
- **Bands**: 2.4 GHz (crowded; typically 1/6/11 as non‑overlapping 20 MHz), 5 GHz (many 20/40/80/160 MHz channels, DFS on some), **6 GHz** for Wi‑Fi 6E (more contiguous spectrum).
- **PHYs**: DSSS/CCK (11b), OFDM (11a/g), MIMO‑OFDM (11n), wider channels & higher modulation (11ac), **OFDMA + MU‑MIMO + BSS Coloring** (11ax).

---

## MAC: CSMA/CA with inter‑frame spaces

1. **Sense**: wait for medium idle for **DIFS** (Distributed Inter‑Frame Space).
2. **Backoff**: pick random **backoff** in slots within **CW** (contention window); count down while idle, freeze on busy.
3. **Transmit** when backoff hits 0.
4. **ACK**: receiver returns **ACK** after **SIFS** (Short IFS).
5. **Failure**: if no ACK, sender **retries** with bigger CW (binary exponential backoff).

**IFS family**: `SIFS < PIFS < DIFS < EIFS` (ordering).

**NAV (virtual carrier sense)**: stations set NAV from frame **Duration/ID** to defer for the announced time.

**Hidden/exposed nodes & RTS/CTS**:

- **Hidden node**: A and C cannot sense each other; both collide at B (AP). Mitigate with **RTS/CTS** handshake (small RTS, CTS from AP reserves the medium).
- **Exposed node**: needless deferral due to overhearing; RTS/CTS may help in some topologies.

**QoS (802.11e/EDCA)**: 4 access categories (VO, VI, BE, BK) with different **AIFS**, **CWmin/max**, **TXOP** limits → prioritization and bursts.

---

## 802.11 MAC frame — key fields

Generic MAC frame (not all fields present in every subtype):

- **Frame Control (2 B)** — bit fields:
    
    `ver(2) | type(2) | subtype(4) | ToDS | FromDS | MoreFrag | Retry | PwrMgt | MoreData | Protected | Order`
    
    *Type*: mgmt/control/data. *Protected* indicates encrypted body (WEP/TKIP/CCMP/GCMP). *To/From DS* determine address layout.
    
- **Duration/ID (2 B)** — sets **NAV** (virtual carrier sense).
- **Addr1/Addr2/Addr3 (6 B each)** — receiver, transmitter, BSSID (roles depend on ToDS/FromDS).
- **Seq Control (2 B)** — sequence number + fragment number.
- **Addr4 (6 B)** — present when both **ToDS=1** and **FromDS=1** (WDS/mesh).
- **QoS Control (2 B)** — QoS TID/ack policy/ECI.
- **HT/VHT/HE Control (optional)** — high‑throughput control fields.
- **FCS (4 B)** — CRC‑32.

**Management frames**: **Beacon**, **Probe Req/Resp**, **Auth**, **Assoc/Disassoc**, **Reassoc**, **Deauth**.

**Control frames**: **RTS/CTS**, **ACK**, **Block ACK**, **PS‑Poll**, **CF‑End**.

**Data frames**: carry MSDUs; QoS subtypes add TID.

---

## Discovery, auth, association, roaming

- **Scanning**:
    
    **Passive** — listen for **Beacons** (contain SSID, supported rates, TIM/DTIM, capabilities, channel, etc.).
    
    **Active** — send **Probe Request** (SSID‑specific or wildcard); APs reply with **Probe Response**.
    
- **Authentication**: legacy **Open System** (trivial); shared‑key auth deprecated (WEP). Enterprise uses **802.1X/EAP**above this stage.
- **Association**: **Assoc Request/Response** exchange capabilities and get an **AID**; **Reassoc** when roaming to a new AP in same ESS.
- Enhancements: **802.11k/v/r** (radio measurements, BSS transitions, fast BSS transition) for smoother roaming.

---

## Reliability & aggregation

- **Per‑frame ACKs** (SIFS‑spaced) give positive feedback; retries on loss.
- **Block ACK** (11e/11n): acknowledge a **bitmap** of MPDUs in a burst → fewer control frames.
- **Aggregation**: **A‑MSDU** (aggregate MSDUs into one MPDU payload) and **A‑MPDU** (aggregate MPDUs under one PHY frame).
- **Fragmentation** exists but is uncommon in modern high‑MTU LANs.

---

## Rate adaptation

- Sender adapts MCS based on **ACK success**, **SNR**, and history; algorithms include ARF/minstrel.
- **Fallback** on errors to a more robust MCS; **probe** higher MCS periodically.

---

## PHY evolution highlights

- **802.11n (HT)**: MIMO (up to 4 streams), **40 MHz** channels, **short guard**, **A‑MPDU/A‑MSDU**, **Block ACK** → big throughput jump.
- **802.11ac (VHT)**: **80/160 MHz** channels, higher constellation (up to 256‑QAM), **MU‑MIMO (downlink)**, beamforming improvements.
- **802.11ax (HE; Wi‑Fi 6/6E)**: **OFDMA** (resource units for UL/DL multi‑user), **BSS Coloring** (spatial reuse), **UL & DL MU‑MIMO**, **Target Wake Time (TWT)** for power save, improved scheduling/efficiency.
- (Context) **802.11be (Wi‑Fi 7)**: multi‑link ops (MLO), 320 MHz channels, higher‑order QAM.

---

## Security

- **WEP**: broken. Do not use.
- **WPA/WPA2‑PSK**: **4‑way handshake** derives **PTK** from **PMK** using nonces (ANonce/SNonce) and MACs; distributes **GTK** for broadcast. **WPA2‑Enterprise (802.1X/EAP)** uses per‑user credentials and RADIUS to derive PMK.
- **WPA3**: replaces PSK with **SAE** (dragonfly) for stronger password auth & forward secrecy; mandates **PMF (802.11w)** for management frame protection.
- Always prefer **AES‑CCMP/GCMP** ciphers; avoid TKIP.

---

## Power save

- **TIM/DTIM** in beacons signal buffered frames at the AP; stations wake to receive.
- **U‑APSD** (WMM‑Power Save): trigger‑based delivery aligned with app priorities; **TWT** (11ax) schedules wake windows.

---

## Common pitfalls & exam cues

- **Why ACKs in Wi‑Fi?** No collision detection; ACKs provide reliability & help drive retransmission.
- **DIFS vs SIFS:** SIFS is shortest → ACK/CTS/Block‑ACK go out **before** contending traffic (priority).
- **ToDS/FromDS bits** decide which addresses appear and in what roles (Addr1‑4).
- **RTS/CTS** helps with **hidden nodes**, but adds overhead; use adaptively.
- **2.4 GHz planning:** prefer **1/6/11** for 20 MHz; watch for adjacent‑channel interference.
- **NAV** enforces virtual carrier sense from Duration/ID.