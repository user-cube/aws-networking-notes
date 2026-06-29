---
title: "02 - AWS Direct Connect"
description: "What Direct Connect is, connection types, virtual interfaces, redundancy, and when to use it over VPN."
tags:
  - aws
  - networking
  - direct-connect
  - hybrid-connectivity
---

**AWS Direct Connect** provides a **dedicated physical network connection** between your on-premises data centre and AWS. Traffic does not travel over the public internet — it uses a private, dedicated link.

---

## Key Benefits Over VPN

| Property | Site-to-Site VPN | Direct Connect |
|---|---|---|
| Path | Public internet | Dedicated private link |
| Bandwidth | Up to ~1.25 Gbps | 1 Gbps to 100 Gbps |
| Latency | Variable | Consistent and low |
| Encryption | IPSec (built-in) | Not encrypted by default |
| Setup time | Minutes | Weeks to months |
| Cost | Low | Higher (port + data transfer) |

> [!warning] Direct Connect is not encrypted by default
> The physical link is private and dedicated, but traffic is not encrypted. For sensitive data, combine Direct Connect with a **Site-to-Site VPN over Direct Connect** to add encryption.

---

## How It Works

Direct Connect uses a **Direct Connect Location** — a colocation facility where AWS has physical infrastructure. Your network connects to that facility either directly or through a network provider.

```
Your data centre
  └── Network provider / cross-connect
        └── Direct Connect Location
              └── AWS network
                    └── VPC (via Virtual Interface)
```

---

## Connection Types

| Type | Description | Bandwidth |
|---|---|---|
| **Dedicated Connection** | Physical port allocated exclusively to your account | 1, 10, or 100 Gbps |
| **Hosted Connection** | Ordered through an AWS Direct Connect Partner; shared infrastructure | 50 Mbps to 10 Gbps |

---

## Virtual Interfaces (VIFs)

A single Direct Connect connection is divided into logical channels called **Virtual Interfaces (VIFs)**:

| VIF Type | Access To |
|---|---|
| **Private VIF** | Resources inside a VPC (via Virtual Private Gateway or Transit Gateway) |
| **Public VIF** | AWS public services (S3, DynamoDB, etc.) using their public IPs |
| **Transit VIF** | Multiple VPCs via a Transit Gateway |

---

## Redundancy

A single Direct Connect connection is a single point of failure. For production workloads:

| Pattern | Resilience |
|---|---|
| Two Direct Connect connections to different locations | Protects against a location failure |
| Direct Connect (primary) + Site-to-Site VPN (backup) | Protects against a Direct Connect failure with lower-cost failover |

> [!tip] AWS recommends dual connections
> For maximum resilience, AWS recommends two dedicated connections, each to a different Direct Connect Location, from different network providers.

---

## Direct Connect Gateway

A **Direct Connect Gateway** allows a single Direct Connect connection to reach VPCs in **multiple AWS Regions**, without requiring separate connections per Region.

```
On-premises
  └── Direct Connect
        └── Direct Connect Gateway
              ├── VPC in eu-west-1
              ├── VPC in us-east-1
              └── VPC in ap-southeast-1
```
