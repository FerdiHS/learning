# Lecture 10: TCP Congestion Control

## Loss Based Algorithms

- **Capacity Estimation [Capacity Probing]**: determine how much capacity is available in the network and estimate how many packets it can have in transit.
- **Self clocking**: use the **arrival of an ack** as a signal that one of its packet has been removed from the network (or reached the destination).
- Lost packet indicates congestion which trigger congestion control mechanism.
- At router use appropriate queuing discipline and buffer management.
- At end hosts use controlled rate of sending packets.

### Capacity Probing

- Iteratively increase the $cwnd$.
- When congestion is detected, reset $cwnd$ to small value.

### Slow Start Algorithm

- Initially $cwnd$ sets to 1 segment by the sender.
- Effectively doubles the $cwnd$ every RTT.
- When a loss is detected through timeout:
    - $max \big(2 \text{mss}, \frac{1}{2} \min (cwnd, \text{advertised } rwnd)\big)$ is saved in $ssthresh$ (slow start threshold).
    - $cwnd$ is set to 1 MSS.

### Fast Retransmit and Fast Recovery

- After 3 duplicate ACKs (DUPACK), sender enters fast retransmit state.
- Retransmit the package lost.
- Steps:
    1. When the third DUP ACK is received, set $sstresh = \max( \frac{cwnd}{2}, 2 \times \text{MSS})$.
    2. Retransmit the lost segment and set $cwnd = ssthresh + 3 \times \text{MSS}$.
    3. For each additional DUP ACK received, $cwnd = cwnd + \text{MSS}$.
    4. Transmit a segment, if allowed by new value of $cwnd$ and receiver’s advertised window.
    5. When nextACK arrives that asks new data, set $cwnd = ssthresh$.

### TCP Fairness

**Fairness goal**: if $K$ TCP sessions share same bottleneck link of bandwith $R$, each should have average rate of $\frac{R}{K}$.

TCP is fair under idealized assumptions:

- Same RTT.
- Fixed number of sessions only in congestion avoidance.

### TCP Cubic

- Remember the last loss window **$wmax$** and grow the window as a **cubic function of time** since the last loss: growth is **fast when far from $wmax$**, and **gentle near $wmax$** so you don’t overshoot. The curve is centered at a time **$K$**, when the window would re-reach $wmax$.
- After loss, record $wmax$, cut the window, then **initially ramp up quickly** toward $wmax$ and **approach it slowly** near the top.
- Increase TCP’s sending rate until packet loss occurs at some router’s output: the **bottleneck link**.

---

## Delay Based Algorithms

### Basic Idea

- Normal RTT means transmitting at the capacity.
- Longer RTT means there is a congestion.
- Smaller RTT means the bandwidth is underutilized.

---

## Network Assisted (ECN)

Routers **mark** (instead of drop) by setting **ECN bits** in the IP header; the receiver reflects this with **ECE** on TCP ACKs so the sender backs off. This couples IP (marking) and TCP (C/E control bits).