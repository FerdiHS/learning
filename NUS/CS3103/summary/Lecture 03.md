# Lecture 03: Framing, Switching, VLAN

## Ethernet framing (Layer 2 on wired LANs)

**Purpose:** turn a bitstream on the wire into **frames** with addressing, type, and integrity.

**Canonical Ethernet II frame (without VLAN):**

- **Preamble (7B)** `55…55` + **SFD (1B)** `D5` (clock sync & start marker)
- **Dest MAC (6B)** → **Src MAC (6B)**
- **EtherType (2B)** (e.g., `0x0800` IPv4, `0x86DD` IPv6, `0x0806` ARP)
- **Payload (46–1500B)** (MTU 1500 for Ethernet)
- **FCS/CRC‑32 (4B)** integrity check
- **Inter‑Frame Gap (IFG ~12B)** spacing between frames

**With 802.1Q VLAN tag:** an extra **4B** appears between Src MAC and EtherType:

- **TPID (Tag Protocol ID)** = `0x8100` (or `0x88A8` for provider QinQ outer tag)
- **TCI (Tag Control Information, 16 bits):**
    - **PCP** (3b): 802.1p priority (0–7)
    - **DEI** (1b): Drop Eligible Indicator (congestion preference)
    - **VID** (12b): VLAN ID (1–4094; 0 reserved)

**Bit layout for 802.1Q TCI (16 bits):**

```
15           13 12  11                                     0
+--------------+---+----------------------------------------+
|    PCP (3)   |DEI|               VLAN ID (12)            |
+--------------+---+----------------------------------------+

```

**Notes:**

- **Native VLAN** on a trunk may be untagged. Mismatched native VLANs cause leaks.
- **Jumbo frames** (>1500B payload) are non‑standard but widely supported in DCs.

---

## Layer‑2 switching (bridging) & loop handling

**Learning & forwarding:**

- Switch builds a **MAC (CAM) table**: maps **source MAC → ingress port** when frames arrive.
- On transmit: if **dest MAC known**, forward out that port; if **unknown unicast**/**broadcast**/**certain multicast**, **flood**out all other ports in the VLAN.

**Collision vs broadcast domains:** full‑duplex switching removes collisions; each VLAN is its **own broadcast domain**.

**Store‑and‑forward vs cut‑through:** store‑and‑forward checks FCS (safer); cut‑through lowers latency but can forward bad frames.

**Loop prevention:** Layer‑2 has no TTL → accidental loops melt the LAN. Use **Spanning Tree**:

- **STP (802.1D)** / **RSTP (802.1w)** / **MSTP (802.1s)**: elect **Root Bridge**, compute a loop‑free tree per VLAN/instance; some ports block.
- Link aggregation (**LACP/802.1AX**) bonds links for capacity/redundancy (seen by STP as one logical link).

---

## VLANs (802.1Q): segmentation & trunks

**Why VLANs?**

- Segment a switch fabric into **multiple L2 broadcast domains** for security, performance, and policy.

**Ports:**

- **Access port**: carries a single VLAN; frames **untagged** on the wire; switch assigns ingress frames to that VLAN.
- **Trunk port**: carries **multiple VLANs** using **802.1Q tags**; optionally a **native VLAN** untagged.

**Inter‑VLAN routing:** requires L3 forwarding — either **router‑on‑a‑stick** (sub‑interfaces per VLAN) or an **L3 switch (SVI)**.

**Provider scenarios:**

- **QinQ (802.1ad)**: customer C‑tag **inside** a provider S‑tag; outer **TPID 0x88A8**.

**Common pitfalls:** native VLAN mismatches, unintentional VLAN trunks, orphan ports outside STP domain.

---

## MPLS — Multi‑Protocol Label Switching

**Why MPLS?**

- **Faster/simple forwarding** by labels, **Traffic Engineering (TE)**, and scalable **VPNs** (L2/L3) over an IP core.
- Sits **between L2 and L3** (often called “Layer 2.5”); payload can be IPv4/IPv6/Ethernet/PPP, etc.

**Encapsulation (Ethernet):** EtherType `0x8847` (unicast) / `0x8848` (multicast), followed by **one or more label stack entries**before the payload.

**MPLS label stack entry (32 bits):**

```
31                         12 11   9 8       0
+---------------------------+-------+---------+
|         Label (20)        |  TC   |S|  TTL  |
+---------------------------+-------+---------+

```

- **Label (20b)**: local‑significance value
- **TC** (3b): Traffic Class (QoS/ECN)
- **S** (1b): Bottom‑of‑stack (1 = last label)
- **TTL** (8b): hop count

**Data‑plane behavior:**

- **Ingress LER**: classifies packets into **FECs** and **pushes** label(s).
- **Transit LSRs**: **swap** labels using LFIB; optional **PHP** (penultimate hop pop).
- **Egress LER**: **pops** and forwards native payload (IP/Ethernet…).

**Control‑plane options:**

- **LDP**: distributes labels along the IGP paths (best‑effort).
- **RSVP‑TE**: signals **explicit LSPs** with bandwidth/constraints for TE.
- **BGP‑LU / SR‑MPLS** (beyond basics): inter‑domain labels / segment routing.

**MPLS VPNs (high level):**

- **L3VPN (RFC 4364):** VRFs at edges; **MP‑BGP** distributes **routes + labels**; two labels typically used (service + transport).
- **L2VPN/VPLS:** emulate L2 circuits (point‑to‑point or multipoint) across an MPLS core.

**QoS:** map IP DSCP↔MPLS **TC**; carry ECN; police/shape per LSP for predictable TE.

---

## Glossary

- **CAM/MAC table** — switch’s learned mapping of MAC→port per VLAN.
- **Unknown unicast** — frame to an unlearned dest MAC; switch floods.
- **Trunk** — inter‑switch (or switch‑router) link carrying multiple VLANs using tags.
- **SVI** — Switch Virtual Interface used for inter‑VLAN routing on L3 switches.
- **QinQ (802.1ad)** — double‑tagging: provider outer S‑tag + customer inner C‑tag.
- **LER/LSR** — Label Edge/Label Switch Router (ingress/egress vs transit in MPLS).
- **FEC** — Forwarding Equivalence Class; packets that share the same forwarding treatment.
- **PHP** — Penultimate Hop Popping: second‑to‑last LSR removes the top label.
- **LFIB** — Label Forwarding Information Base: label→action table in MPLS.