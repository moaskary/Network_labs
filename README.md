# Network Labs — Hands-On Portfolio

Hands-on networking labs documented as I work through an intensive, Cisco CCNA-aligned study plan. Each lab is built in **Cisco Packet Tracer**, with packet-level analysis in **Wireshark**. The goal is to *demonstrate* networking concepts, not just describe them.

**Background:** B.Sc. Computer Science (2023) · Cisco Networking Academy CCNA 1 & 2 · currently preparing for CompTIA Network+ (N10-009) and Cisco CCNA (200-301).

## Labs

| # | Lab | Topics | Status |
|---|-----|--------|--------|
| 01 | [Switch Configuration & Hardening](./lab-01-switch-config-and-hardening/README.md) | IOS CLI, modes, hostname, banner, `enable secret`, console/VTY passwords, password encryption | ✅ Complete |
| 02 | [Switching: MAC Learning & ARP](./lab-02-switching-mac-and-arp/README.md) | MAC address table, source learning, flooding, ARP request/reply, broadcast domains | ✅ Complete |
| 03 | [IPv4 Addressing & Reachability](./lab-03-ipv4-addressing-and-reachability/README.md) | IPv4 classes, binary conversion, subnet masks, same-subnet vs cross-subnet reachability | ✅ Complete |
| 04 | [Inter-Subnet Routing with a Router](./lab-04-inter-subnet-routing/README.md) | Routing between subnets, default gateways, connected routes, ARP-for-gateway, TTL | ✅ Complete |
| 05 | [Remote Management with Telnet (vty)](./lab-05-telnet-remote-access/README.md) | vty lines, Telnet remote access, enable secret for remote privileged mode, Telnet vs SSH | ✅ Complete |

## Tools
Cisco Packet Tracer · Wireshark · macOS Terminal

## Repository layout
```
network-labs/
├─ README.md                 (this file)
├─ lab-01-switch-config-and-hardening/
│   ├─ README.md
│   ├─ lab_01.pkt
│   └─ screenshots/
├─ lab-02-switching-mac-and-arp/
│   ├─ README.md
│   ├─ lab_02.pkt
│   └─ screenshots/
├─ lab-03-ipv4-addressing-and-reachability/
│   ├─ README.md
│   ├─ lab_03.pkt
│   └─ screenshots/
├─ lab-04-inter-subnet-routing/
│   ├─ README.md
│   ├─ lab_04.pkt
│   └─ screenshots/
└─ lab-05-telnet-remote-access/
    ├─ README.md
    ├─ lab_05.pkt
    └─ screenshots/
```
