---
title: "16 - Transit Gateway"
description: "How Transit Gateway centralises VPC and on-premises connectivity, replacing complex peering meshes."
tags:
  - aws
  - vpc
  - networking
  - transit-gateway
---

**AWS Transit Gateway (TGW)** is a regional network hub that connects multiple VPCs and on-premises networks through a single gateway. Instead of creating individual peering connections between every pair of VPCs, all VPCs connect to the Transit Gateway — which handles routing between them.

---

## The Problem It Solves

With VPC Peering, connecting many VPCs requires a full mesh of individual connections:

```
4 VPCs with peering — 6 connections required:
  VPC A ←→ VPC B
  VPC A ←→ VPC C
  VPC A ←→ VPC D
  VPC B ←→ VPC C
  VPC B ←→ VPC D
  VPC C ←→ VPC D
```

With Transit Gateway — **4 attachments, 1 hub**:

```
VPC A ─┐
VPC B ─┤── Transit Gateway
VPC C ─┤
VPC D ─┘
```

The number of connections grows linearly instead of quadratically.

---

## Attachments

A Transit Gateway connects to other networks through **attachments**:

| Attachment Type | Connects To |
|---|---|
| VPC attachment | An AWS VPC (one subnet per AZ recommended) |
| VPN attachment | A Site-to-Site VPN connection |
| Direct Connect Gateway attachment | An AWS Direct Connect connection |
| Transit Gateway Peering attachment | Another Transit Gateway (same or different Region) |

---

## Route Tables

A Transit Gateway has its own **route tables**, independent of VPC route tables.

Each attachment is associated with a TGW route table that determines where traffic from that attachment can go. This enables network segmentation:

```
Production VPCs    →  TGW Route Table A  →  can reach each other + on-premises
Development VPCs   →  TGW Route Table B  →  can reach internet only, not production
```

> [!tip] Network segmentation with multiple route tables
> Using separate TGW route tables for different environments (prod, dev, shared services) is a common pattern for enforcing traffic isolation without deploying multiple Transit Gateways.

---

## Key Characteristics

| Property | Detail |
|---|---|
| Scope | Regional — one TGW per Region (can peer across Regions) |
| Scalability | Supports thousands of VPCs and on-premises connections |
| Transitive routing | ✅ Supported — unlike VPC Peering |
| Multicast | Supported (optional) |
| Bandwidth | Up to 50 Gbps per VPC attachment |

---

## VPC Peering vs Transit Gateway

| | VPC Peering | Transit Gateway |
|---|---|---|
| Transitive routing | ❌ No | ✅ Yes |
| Number of connections | Grows as n(n-1)/2 | Grows linearly |
| Cross-account | ✅ Yes | ✅ Yes |
| Cross-region | ✅ Yes | ✅ Yes (via peering) |
| On-premises connectivity | ❌ No | ✅ Yes (VPN / Direct Connect) |
| Cost | Lower for few VPCs | More cost-effective at scale |
| Complexity | Low | Higher (route tables, attachments) |

---

## Pricing

- Charged per **attachment** (hourly)
- Charged per **GB of data processed**

For large-scale environments, the operational simplicity and reduced number of connections typically outweigh the cost compared to managing a full peering mesh.
