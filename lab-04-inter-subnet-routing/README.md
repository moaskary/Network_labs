Lab 04 — Inter-Subnet Routing with a Router

Objective

Connect two separate IPv4 subnets with a router and prove that hosts on different networks can communicate only through a Layer-3 device. This is the direct follow-on to Lab 03, where cross-subnet traffic failed with a switch alone.

Topology

A router joins two switched LANs, with one router interface in each subnet acting as that subnet's default gateway.


R1 (Cisco 2911)

192.168.1.254 — gateway for 192.168.1.0/24 (toward SW1)
192.168.2.254 — gateway for 192.168.2.0/24 (toward SW2)



SW1 → PC0, PC1 (192.168.1.0/24)
SW2 → PC2, PC3 (192.168.2.0/24)
File: lab_04.pkt


Show Image

What I configured


An IP address on each router interface — the per-subnet default gateway.
Each PC's default gateway set to its local router interface.
No static or dynamic routing required — both networks are directly connected to R1, so it routes between them automatically.


Results

SourceDestinationTypeResultPC0 (192.168.1.x)PC1 (192.168.1.x)same subnet✅ successPC0 (192.168.1.x)PC2 (192.168.2.x)cross subnet✅ successPC1 (192.168.1.x)PC3 (192.168.2.x)cross subnet✅ successPC2 (192.168.2.x)PC3 (192.168.2.x)same subnet✅ success

Key concepts demonstrated


A router forwards between directly-connected networks automatically. show ip route on R1 shows two C (connected) routes — one per subnet — which is all it needs.
Hosts ARP for the gateway, not the remote destination. When PC0 pings a host in 192.168.2.0/24, it sees the destination is on another network and sends the frame to its default gateway's MAC. PC0's ARP table proves it: it contains 192.168.1.254 → 0002.4a48.4801 (the router's interface MAC). The destination IP is unchanged — only the frame's destination MAC is the router.
The same router MAC appears on SW1's uplink. show mac address-table on SW1 lists 0002.4a48.4801 on Gig0/1 — the switch learned the router's interface on the uplink port. One MAC, three viewpoints (router interface, host's gateway ARP entry, switch's uplink).
TTL proves a router hop. A cross-subnet ping reply arrives with TTL 127 (R1 decremented it by 1); a same-subnet reply stays at 128.


Evidence

SW1 MAC table — router MAC learned on Gig0/1:
Show Image

PC0 ARP table — the gateway's MAC is cached for remote traffic:
Show Image

How I'd verify / troubleshoot


show ip route on R1 — confirm both connected networks appear.
arp -a on a host — confirm the gateway's MAC is learned.
Confirm each PC has the correct default gateway (a missing/wrong gateway = cross-subnet fails).
Ping the local gateway first, then a remote host — isolates a Layer-2 problem from a Layer-3 one.



Part of my Network Labs portfolio.