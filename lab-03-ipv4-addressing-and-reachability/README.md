Lab 03 — IPv4 Addressing & Subnet Reachability

Objective

Address hosts with IPv4, identify address classes, convert addresses and masks to binary, and demonstrate why two hosts on different subnets cannot communicate through a switch alone.

Environment


Cisco Packet Tracer
2 × PC, 1 × Cisco 2960 switch
File: lab_03.pkt


Part 1 — Addressing & binary

Converted 10 IPv4 addresses and their masks to binary and identified each address class. Class is set by the first octet: A = 1–126, B = 128–191, C = 192–223. Examples:

AddressMaskMask (binary, last active octet)Class192.168.1.10255.255.255.000000000 (/24)C10.0.5.23255.0.0.000000000 (/8)A172.16.40.200255.255.0.000000000 (/16)B192.168.100.55255.255.255.12810000000 (/25)C172.20.15.99255.255.240.011110000 (/20)B

(Full 10-address worked set is in the screenshot below.)

Part 2 — Same-subnet vs cross-subnet reachability

Configured two PCs on a single switch and tested connectivity:

SourceDestinationSame subnet?Result192.168.1.10 /24192.168.1.20 /24Yes✅ 0% loss192.168.1.10 /24192.168.2.20 /24No❌ 100% loss (timeout)

Key insight

Both PCs are on the same physical switch, yet the cross-subnet ping fails. A switch operates at Layer 2 and only moves frames within one network. When 192.168.1.10 targets 192.168.2.20, it sees the destination is on a different subnet, so it forwards the packet to its default gateway (192.168.1.1) — but there is no router there, so the request times out.

A switch connects hosts within a network; a router is required to move traffic between networks. This is the motivation for routing (and inter-VLAN routing) later in the plan.

What I learned


IPv4 classes and their first-octet ranges; how the mask sets the network-vs-host boundary.
Fast binary ↔ decimal conversion for both addresses and subnet masks.
A host decides "local vs remote" by comparing the destination against its own subnet, then sends remote traffic to its default gateway.
Two subnets on the same switch still need a router to communicate.


Evidence

Binary & class practice (10 addresses):
P&C practice.png

Static IP configuration:
Screenshot1.png , Screenshot2.png

Same subnet — reachable:
Screenshot1.png , Screenshot2.png

Different subnet — unreachable without a router:
Screenshot3.png , Screenshot4.png