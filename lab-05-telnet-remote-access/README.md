Lab 05 — Remote Management with Telnet (vty)

Objective

Configure remote CLI access to a router and a switch over the network using Telnet on the vty lines, and connect to them from a PC.

Topology


PC0 — Switch0 — Router0 (2911) — PC1
Subnets: 192.168.20.0/24 and 192.168.30.0/24
Managed devices: R1 (192.168.20.254), SW1 (192.168.20.100)
File: lab_05.pkt



What I configured


Enabled Telnet on the vty lines (line vty 0 4 / 0 15) with a login password.
Set an enable secret so privileged mode is reachable remotely.
Connected from a PC with telnet <device-ip> and authenticated.


What I learned


Remote privileged mode requires an enable secret. On the first Telnet session, enable returned % No password set and refused access. Cisco blocks entry to privileged EXEC over a vty session unless an enable password/secret exists — a deliberate security control (the console behaves differently). After configuring enable secret, remote enable worked.
Telnet is cleartext — use SSH in production. Telnet sends everything, including passwords, unencrypted; anyone capturing the traffic can read it. It's fine for learning the vty/remote-access concept, but real environments use SSH (encrypted). Lab 01 set up the SSH building blocks; this lab demonstrates the remote-access workflow itself.
A first connection attempt timed out and a retry succeeded — a reminder to confirm Layer-3 reachability (ping the management IP) before assuming a configuration problem.


Evidence

Telnet into R1 — note the % No password set lesson, then success after setting the secret:
see image

Telnet into SW1:
see images

How I'd verify / troubleshoot


show running-config → confirm line vty, login, the password, and enable secret.
Ping the management IP first (Layer-3 reachability) before troubleshooting the vty config.
show users on the device → see active remote sessions.


Production note

For anything beyond a lab, I would replace Telnet with SSH: set ip domain-name, crypto key generate rsa, create a local user with login local, and set transport input ssh on the vty lines — turning cleartext remote access into an encrypted session.


Part of my Network Labs portfolio.