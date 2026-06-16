# Lab 01 — Switch Configuration & Hardening

## Objective
Bring up a Cisco Catalyst switch from factory defaults and apply baseline configuration and management-plane security using the IOS command line.

## Environment
- Cisco Packet Tracer
- 1 × Cisco 2960 switch (IOS 15.0)
- File: `first_PT.pkt`

## What I configured
- Navigated the IOS modes: user EXEC → privileged EXEC (`enable`) → global config (`configure terminal`) → line config
- Set the **hostname** (`SW1`) and a login **MOTD banner**
- Secured privileged EXEC with **`enable secret`** (stored as a Type 5 / MD5 hash)
- Set **console** and **VTY (0–15)** line passwords with `login`
- Applied **`service password-encryption`** so the plaintext line passwords are not stored in clear text
- Saved the configuration and verified it persisted across a `reload`
- Used **`do show running-config`** to verify from inside config mode

## Configuration applied (verified, secrets redacted)
```cisco
service password-encryption
!
hostname SW1
!
enable secret 5 $1$****        ! Type 5 (MD5 hash) — one-way
!
banner motd ^CHello World^C
!
line con 0
 password 7 ****               ! Type 7 — reversible (service password-encryption)
 login
!
line vty 0 4
 password 7 ****
 login
line vty 5 15
 password 7 ****
 login
!
end
```
> **Security note:** the real password hashes are redacted here. Even hashed/encrypted secrets shouldn't be committed to a public repository — Type 7 is trivially reversible and Type 5 (MD5) is crackable offline. Redacting them is the correct habit for any public portfolio.

## What I learned
- **`enable secret` is a one-way hash (Type 5)**, shown as `5 $1$...` in the config. The line passwords are **Type 7 (reversible)**, shown as `7 ...` because `service password-encryption` is enabled — a live example of "hashed vs encrypted" in my own output.
- `service password-encryption` only weakly protects the console / VTY / `enable password` entries; it does **not** affect `enable secret`. Best practice is to rely on `enable secret` and skip `enable password` entirely (I did).
- **Running-config (RAM) vs startup-config (NVRAM):** unsaved changes are lost on reload — `copy running-config startup-config` makes them persistent.

## How I'd verify / troubleshoot
- `show running-config` — confirm all settings were applied
- `show running-config | include secret` — confirm the password is stored as a hash
- `reload` and re-check — prove the configuration persisted

## Next step (Day 6)
Add real remote management: a management IP on a VLAN interface, `ip domain-name`, `crypto key generate rsa`, a local user, and `transport input ssh` on the VTY lines — turning password-only access into encrypted SSH administration.

---
*Part of my [Network Labs](../README.md) portfolio.*
