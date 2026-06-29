---
title: "02 - Subnets"
description: "Subnet basics, reserved IP addresses, and key considerations for subnet design."
tags:
  - aws
  - vpc
  - networking
  - subnets
---

A **subnet** is a range of IP addresses within a VPC.

Key characteristics:

- A subnet exists within a **single Availability Zone (AZ)** — it does not span multiple AZs
- It can be either **public** or **private**
- It must be fully contained within the VPC's CIDR block
- IPv4 subnet sizes range from `/16` to `/28`

---

## Reserved IP Addresses

AWS **reserves 5 IP addresses** in every subnet — the first four and the last one.

For example, in `192.168.10.0/24`:

| IP Address | Purpose |
|---|---|
| `192.168.10.0` | Network address |
| `192.168.10.1` | VPC Router |
| `192.168.10.2` | Amazon DNS |
| `192.168.10.3` | Reserved for future AWS use |
| `192.168.10.255` | Broadcast address (reserved, not used) |

> [!important] Account for reserved addresses when sizing subnets
> A `/28` subnet has 16 total IPs — only 11 are usable after AWS reserves 5.

---

## Additional Considerations

- Subnets within the same VPC **cannot overlap**
- **IPv6** is supported — a subnet can be configured as IPv6-only (no IPv4)
- Resources can be configured to **automatically receive a public IPv4 address** on launch, in addition to their private IP
