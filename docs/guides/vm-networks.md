---
description: "VMware Workstation and VirtualBox network modes compared: bridged, NAT, host-only and internal, with gateways, DNS and DHCP."
---

# VMware Workstation and VirtualBox networks

## VMware Workstation

| Network | Internet | VM ↔ VM | Host ↔ VM | Gateway | DNS | DHCP |
|---|---|---|---|---|---|---|
| **Bridged** (VMnet0) | Yes | Yes | Yes | Real router | Real network's DNS | Real router |
| **NAT** (VMnet8) | Yes | Yes | Yes | `.2` | `.2` | `.254` (range `.128`–`.253`) |
| **Host-only** (VMnet1) | No | Yes | Yes | None | None | `.254` (range `.128`–`.253`) |
| **LAN Segment** | No | Yes | No | None | None | None |
| **Custom** (VMnet2–19) | Depends on config | Yes | Depends on config | — | — | — |

- The NAT and host-only subnets are **assigned randomly** at install time (192.168.x.0/24) and can be changed in the *Virtual Network Editor*.
- The host always has the `.1` address.

## VirtualBox

| Network | Internet | VM ↔ VM | Host ↔ VM | Gateway | DNS | DHCP |
|---|---|---|---|---|---|---|
| **Bridged Adapter** | Yes | Yes | Yes | Real router | Real network's DNS | Real router |
| **NAT** | Yes | No | Port forwarding | `10.0.2.2` | `10.0.2.3` | Built-in (VM gets `10.0.2.15`) |
| **NAT Network** | Yes | Yes | Port forwarding | `10.0.2.1` | `10.0.2.1` | `10.0.2.3` (VMs from `.4`) |
| **Host-only Adapter** | No | Yes | Yes | None | None | `192.168.56.100` (range `.101`–`.254`) |
| **Internal Network** | No | Yes | No | None | None | None |

- The subnets are **fixed**: `10.0.2.0/24` for NAT and NAT Network, and `192.168.56.0/24` for host-only.
- In host-only mode the host has `192.168.56.1`.
- For several VMs that need Internet access and must see each other, use **NAT Network**, not NAT.
