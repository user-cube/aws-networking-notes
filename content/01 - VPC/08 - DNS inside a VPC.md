---
title: "08 - DNS inside a VPC"
description: "How DNS works inside a VPC, Amazon Route 53 Resolver, public DNS hostnames, and the two VPC DNS settings."
tags:
  - aws
  - vpc
  - networking
  - dns
---

Every AWS resource that has a private IP address automatically receives a **private DNS hostname**, allowing services to communicate by name rather than IP address.

---

## Amazon Route 53 Resolver

DNS resolution inside a VPC is handled by the **Amazon Route 53 Resolver**, available at two addresses:

- `169.254.169.253` — link-local address (always reachable)
- The **second IP address of the VPC CIDR block** (reserved by AWS for DNS)

| VPC CIDR | Amazon DNS Address |
|---|---|
| `10.0.0.0/16` | `10.0.0.2` |
| `172.31.0.0/16` | `172.31.0.2` |
| `192.168.0.0/16` | `192.168.0.2` |

---

## VPC DNS Settings

Two settings control DNS behaviour in a VPC:

### Enable DNS Support

| Value | Behaviour |
|---|---|
| `true` (default) | The AWS-provided DNS resolver is available inside the VPC |
| `false` | Resources cannot use the AWS DNS service |

When enabled, resources can resolve internal AWS hostnames and services can communicate using DNS names instead of IP addresses.

### Enable DNS Hostnames

| Value | Behaviour |
|---|---|
| `true` | Resources with a public IP also receive a **public DNS hostname** |
| `false` (default for custom VPCs) | Only private DNS records are created |

> [!note] Default VPC
> In the **Default VPC**, both `Enable DNS Support` and `Enable DNS Hostnames` are already set to `true`.

> [!important] Both settings are required for public DNS hostnames
> `Enable DNS Hostnames` only works if `Enable DNS Support` is also `true`.

---

## Why DNS Inside a VPC Matters

- Eliminates the need to hardcode IP addresses in application configuration
- Enables **service discovery** — services can refer to each other by stable DNS names
- Internal DNS names automatically update if an instance is replaced
