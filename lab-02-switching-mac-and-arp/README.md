# Lab 02 — Switching: MAC Address Learning & ARP

## Objective
Demonstrate how a Layer 2 switch dynamically learns MAC addresses, and how ARP resolves an IP address to a MAC address on a local network.

## Topology
3 × PCs connected to a Cisco 2960 switch (ports Fa0/1–Fa0/3) with copper straight-through cabling. Single subnet **192.168.1.0/24**. File: `secod_PT.pkt`.

| Device | IP address | Switch port | MAC (learned) |
|--------|-----------|-------------|----------------|
| PC0 | 192.168.1.x *(confirm)* | Fa0/1 | 0001.c775.d265 |
| PC2 | 192.168.1.x *(confirm)* | Fa0/2 | 0090.0c8c.22db |
| PC3 | 192.168.1.30 | Fa0/3 | 0001.6436.1c85 |

## What I did & observed
1. **Connectivity:** pinged across hosts (`ping 192.168.1.20`, `ping 192.168.1.30`) — 4/4 replies, 0% loss.
2. **MAC learning:** ran `show mac address-table` (empty) → `clear mac address-table dynamic` → re-ran after pings and watched the switch dynamically relearn all three MAC addresses against their correct ports (Fa0/1–Fa0/3).
3. **ARP resolution:** on PC0, ran `arp -d` (clear cache) → `arp -a` returned *"No ARP Entries Found"* → `ping 192.168.1.30` → `arp -a` now showed `192.168.1.30 → 0001.6436.1c85 (dynamic)`.

## Key insight
On the ARP test the **first** ping reply was ~5 ms while the rest were ~4 ms. That extra time is **ARP resolving the destination MAC before the first packet could be sent** — a live demonstration of ARP working underneath ICMP.

## What I learned
- A switch learns by **source MAC** (records MAC + incoming port) and forwards by **destination MAC**. An unknown destination MAC is **flooded** out every port except the one it arrived on; once the target replies, the switch learns it and future traffic is unicast.
- **ARP maps IP → MAC** within a broadcast domain: the request is a broadcast, the reply is a unicast, and the result is cached in the ARP table.
- **MAC (Layer 2) = local delivery; IP (Layer 3) = end-to-end.** ARP is the bridge between them.

## How I'd verify / troubleshoot
- `show mac address-table` on the switch
- `arp -a` on the host (and `arp -d` to clear and re-test)
- Packet Tracer **Simulation mode** or **Wireshark** to watch the ARP request/reply exchange

---
*Part of my [Network Labs](../README.md) portfolio.*
