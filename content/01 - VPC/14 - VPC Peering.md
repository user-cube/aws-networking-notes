---
title: "14 - VPC Peering"
description: "How VPC Peering works, its constraints, routing requirements, and when to use it."
tags:
  - aws
  - vpc
  - networking
  - vpc-peering
---

**VPC Peering** creates a direct, private network connection between two VPCs. Traffic between peered VPCs travels over the AWS internal network — it never traverses the public internet.

Peering can be established between VPCs in:

- The **same AWS account**
- **Different AWS accounts**
- **Different AWS Regions** (inter-region peering)

---

## How It Works

```
VPC A (10.0.0.0/16)  ←──── Peering Connection ────→  VPC B (172.16.0.0/16)
```

Once a peering connection is established and accepted, you must add routes to each VPC's Route Tables to enable communication.

| VPC | Route Table entry | Target |
|---|---|---|
| VPC A | `172.16.0.0/16` | Peering connection |
| VPC B | `10.0.0.0/16` | Peering connection |

Security Groups and NACLs still apply — peering only provides the network path, not automatic access.

---

## Key Constraints

### No Overlapping CIDR Blocks

The CIDR blocks of the two VPCs **must not overlap**. If they do, the peering connection cannot be created.

| VPC A CIDR | VPC B CIDR | Peering possible? |
|---|---|---|
| `10.0.0.0/16` | `172.16.0.0/16` | ✅ Yes |
| `10.0.0.0/16` | `10.0.0.0/24` | ❌ No — overlapping |

### No Transitive Routing

VPC Peering is **not transitive**. If VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A **cannot** reach VPC C through VPC B.

```
VPC A ←──→ VPC B ←──→ VPC C

VPC A cannot reach VPC C via VPC B.
A direct peering connection between A and C is required.
```

This is the most important limitation of VPC Peering — it does not scale well as the number of VPCs grows. For many VPCs, a **Transit Gateway** is the better solution.

---

## When to Use VPC Peering

| Scenario | Recommendation |
|---|---|
| 2–3 VPCs that need to communicate | VPC Peering — simple, low cost |
| Many VPCs with complex connectivity | Transit Gateway — avoids the mesh of peering connections |
| Cross-account access to a shared service | VPC Peering or PrivateLink |

---

## Pricing

- **Same Region:** No data transfer charge for traffic between peered VPCs
- **Different Regions:** Standard inter-region data transfer rates apply
