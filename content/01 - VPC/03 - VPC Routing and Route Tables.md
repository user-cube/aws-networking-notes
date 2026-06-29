---
title: "03 - VPC Routing and Route Tables"
description: "How the VPC Router works, route tables, longest prefix matching, and the local route."
tags:
  - aws
  - vpc
  - networking
  - routing
---

## The VPC Router

Every VPC contains a **VPC Router** — a logical routing component managed by AWS.

| Property | Detail |
|---|---|
| Interface per AZ | One interface attached to each Availability Zone in the VPC |
| IP address | Always the **first usable IP** in every subnet (subnet base address + 1) |

The VPC Router is responsible for:

- Enabling communication **between subnets** inside the same VPC
- Routing traffic **between the VPC and external networks** (internet, on-premises, other VPCs)

---

## Route Tables

AWS allows routing behaviour to be configured through **Route Tables**.

A Route Table determines where traffic is forwarded based on the destination IP address — exactly like a traditional router:

1. Inspect the destination IP of the packet
2. Find the best matching route
3. Forward traffic accordingly

**Longest prefix match** — when multiple routes match, AWS always selects the most specific one (the route with the longest prefix / largest subnet mask).

### Key Rules

| Rule | Detail |
|---|---|
| One route table per subnet | Each subnet is associated with exactly one route table at a time |
| Default association | New subnets are automatically associated with the VPC's **Main Route Table** |
| Sharing | Multiple subnets can share the same route table |

### Local Route

Every VPC route table includes a **Local Route** by default:

| Destination | Target |
|---|---|
| VPC CIDR (e.g. `10.0.0.0/16`) | `local` |

This route enables communication between **all subnets within the same VPC** and cannot be removed.
