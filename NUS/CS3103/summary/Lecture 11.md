# Lecture 11: SDN (Software Defined Networking)

## Big picture & motivation

Traditional networks couple **control** (routing/learning logic) with **data forwarding** inside each device. Global changes are slow, device‑by‑device, and hard to reason about. **SDN decouples control from data**, centralizing logic so operators can program network behavior directly and consistently.

---

## Core SDN principles

- **Separation of planes:**
    - **Data plane** (switches/routers): fast packet forwarding by simple **match→action** rules.
    - **Control plane** (controller): computes policy, installs rules, reacts to events.
    - **Application plane**: apps express **intent/policy** (e.g., path constraints, ACLs, QoS).
- **Logical centralization:** one controller (or a cluster) provides a **global view** even if physically distributed.
- **Open interfaces:**
    - **Southbound (SBI)**: controller ↔ data plane (e.g., **OpenFlow**, P4Runtime, gNMI).
    - **Northbound (NBI)**: apps ↔ controller (REST/gRPC/SDKs, intent APIs).
- **Abstraction & virtualization:** treat the network as a single programmable system; slice/virtualize resources per tenant or app.

---

## OpenFlow — the classic SDN SBI

- **Flow tables (match→action):** entries match on header fields (in_port, eth, VLAN, IP, TCP/UDP, etc.) and apply actions.
- **Pipeline:** packet traverses tables; matching entries may **accumulate actions** in an **action set** that execute at pipeline end.
- **Common actions:** output to port(s) (e.g., specific port, **controller**, **in_port**, **normal**), push/pop VLAN/MPLS, set fields, go to table.
- **Table‑miss behavior:** send to controller, drop, or go to a next table (configurable via a default entry).
- **Key messages:**
    - **Packet‑In** (switch→controller) when no match or explicit send‑to‑controller.
    - **Flow‑Mod** (controller→switch) to add/modify/delete rules (with priority & timeouts).
    - **Packet‑Out** (controller→switch) to forward a specific packet immediately.
    - **Stats/Features** for monitoring; **Barrier** for sequencing/consistency.

---

## Why SDN helps

- **Faster evolution:** software changes in the controller/apps roll out instantly across many devices.
- **Global optimization:** traffic engineering, consistent ACLs/QoS, fast failover.
- **Lower cost / vendor flexibility:** commodity switches plus open interfaces.
- **Better debugging/visibility:** controller has network‑wide state, events, and counters.

**Trade‑offs / challenges:** controller availability/scale, consistency across distributed controllers, latency for control decisions, instrumentation and security of the SBI/NBI.

---

## Network Virtualization & NFV

- **Network virtualization:** build **logical topologies** (tenants/slices) on one physical fabric, with isolated address spaces and policies.
- **NFV (Network Functions Virtualization):** move middleboxes (FW, NAT, LB, IDS) into **software** on commodity servers (VMs/containers), chaining them via SDN‑steered paths.

---

## Case study (conceptual): Centralized WAN TE (e.g., B4‑style)

- Use SDN with commodity switches and a centralized controller to compute **bandwidth‑aware paths** across the WAN.
- Continuously re‑optimize based on measured demands; achieve high link utilization, fast reconfig on failures, and simplified ops.

---

## Security considerations

- **SBI protection:** encrypt/authenticate controller↔switch channel; restrict management plane access.
- **App safety:** permission models and intent conflict checks on the NBI.
- **Data‑plane invariants:** use verification (reachability/loop‑free/isolation) before rule install.