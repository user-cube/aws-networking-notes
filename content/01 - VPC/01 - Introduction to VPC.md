---
title: "01 - Introduction to VPC"
description: "What a VPC is, CIDR blocks, VPC types, and the default VPC configuration."
tags:
  - aws
  - vpc
  - networking
---

A **Virtual Private Cloud (VPC)** is a logically isolated virtual network hosted on AWS.

A VPC gives you:

- **Isolation** — your resources are separated from other tenants in the cloud
- **Full control** over your network configuration, including:
  - Route Tables
  - Firewalls (Security Groups and Network ACLs)
  - Gateways

A VPC is scoped to a specific **AWS Region**. By default, VPCs are completely isolated from one another, though they can be connected through additional networking features such as VPC Peering or Transit Gateway.

---

## CIDR Blocks

Each VPC is assigned one or more **CIDR blocks**, which define the IP address range available for resources inside the VPC.

| Property | Detail |
|---|---|
| IPv4 CIDR range | `/16` (largest) to `/28` (smallest) |
| Secondary IPv4 CIDR | Can be added after creation |
| IPv6 CIDR | Optional — uses a `/56` block |
| Max CIDR blocks per VPC | 5 by default (can be increased) |

---

## Types of VPCs

| Type | Description |
|---|---|
| **Default VPC** | Automatically created by AWS in every Region; pre-configured and ready to use |
| **Custom VPC** | Created manually; allows full customisation of all networking settings |

---

## Default VPC Configuration

Every AWS Region contains exactly one **Default VPC**, pre-configured with the following:

| Component | Detail |
|---|---|
| IPv4 CIDR | `172.31.0.0/16` — 65,536 IP addresses |
| Subnets | One `/20` subnet per Availability Zone (~4,096 IPs each) |
| Internet Gateway | Attached by default |
| Default route | `0.0.0.0/0` → Internet Gateway |
| Network ACL | Default NACL created |
| Security Group | Default Security Group created |

> [!tip] Default VPC and internet access
> Resources deployed in the Default VPC **can be made accessible from the internet**, provided their Security Group and NACL rules allow it. This makes the Default VPC convenient for getting started quickly, but a Custom VPC is generally recommended for production workloads.
