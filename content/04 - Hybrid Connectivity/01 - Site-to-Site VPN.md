---
title: "01 - Site-to-Site VPN"
description: "How AWS Site-to-Site VPN works, its components, redundancy design, and limitations."
tags:
  - aws
  - networking
  - vpn
  - hybrid-connectivity
---

**AWS Site-to-Site VPN** creates an **encrypted IPSec tunnel** between your on-premises network and your AWS VPC over the public internet. It is the fastest way to establish hybrid connectivity.

---

## Components

| Component | Description |
|---|---|
| **Virtual Private Gateway (VGW)** | The AWS side of the VPN — attached to a VPC |
| **Customer Gateway (CGW)** | Represents the on-premises VPN device in AWS (IP address + routing config) |
| **VPN Connection** | The IPSec tunnel between the VGW and the CGW |

```
On-premises network
  └── Customer Gateway device (firewall/router)
            │ IPSec tunnel (over internet)
            ▼
      Virtual Private Gateway
            │
            ▼
          VPC
```

---

## Redundancy — Two Tunnels per Connection

Every Site-to-Site VPN connection provides **two tunnels**, each terminating in a different AWS Availability Zone. This provides resilience against a single tunnel or AZ failure.

```
On-premises VPN device
  ├── Tunnel 1 → AWS endpoint in AZ-A
  └── Tunnel 2 → AWS endpoint in AZ-B
```

> [!important] Configure both tunnels on your device
> AWS strongly recommends configuring both tunnels on your Customer Gateway device. If only one tunnel is active and it fails, connectivity is lost until the tunnel recovers.

---

## Routing Modes

| Mode | Description |
|---|---|
| **Static routing** | You manually specify the CIDR ranges to advertise over the tunnel |
| **Dynamic routing (BGP)** | Routes are automatically exchanged using Border Gateway Protocol — recommended for flexibility and failover |

---

## Limitations

| Limitation | Detail |
|---|---|
| Bandwidth | Up to ~1.25 Gbps per tunnel |
| Latency | Variable — traffic travels over the public internet |
| Encryption overhead | IPSec encryption adds some processing overhead |
| Reliability | Dependent on internet connectivity quality |

> [!tip] VPN as a Direct Connect backup
> A common pattern is to use **Direct Connect** as the primary connection and **Site-to-Site VPN** as a failover path. This provides both high performance and resilience.

---

## VPN with Transit Gateway

Instead of attaching a VPN to a VPC's Virtual Private Gateway, you can attach it directly to a **Transit Gateway**. This allows the on-premises network to reach **multiple VPCs** through a single VPN connection.

```
On-premises
  └── VPN connection
        └── Transit Gateway
              ├── VPC A
              ├── VPC B
              └── VPC C
```
