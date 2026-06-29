---
title: "06 - NAT Gateway"
description: "How NAT Gateway enables outbound internet access for private subnets without exposing them to inbound connections."
tags:
  - aws
  - vpc
  - networking
  - nat
---

An **Internet Gateway** makes resources directly reachable from the internet. But many scenarios require instances to **access the internet without accepting unsolicited inbound connections**.

Common examples:

- Downloading software updates
- Installing packages
- Contacting external APIs

At the same time, external users should not be able to initiate connections to those servers. This is the purpose of a **NAT Gateway**.

---

## Architecture

![VPC with public and private subnets across two Availability Zones, each with a NAT Gateway](/diagrams/vpc-nat-gateway.png)

---

## How It Works

> [!important] A NAT Gateway does not replace an Internet Gateway
> A VPC must still have an Internet Gateway attached. The NAT Gateway routes traffic through it.

### Deployment Steps

1. Create an **Internet Gateway** and attach it to the VPC
2. Configure a **public subnet** (with a route to the IGW)
3. Deploy the **NAT Gateway** inside the public subnet
4. Update the **Route Table of the private subnet** — add a default route `0.0.0.0/0` pointing to the NAT Gateway

### Traffic Flow

```
Private instance
    → NAT Gateway (public subnet)
        → Internet Gateway
            → Internet

Internet (response)
    → Internet Gateway
        → NAT Gateway
            → Private instance
```

With this configuration:

- ✅ Private resources can **initiate** outbound connections
- ✅ Responses from the internet are allowed back
- ❌ External hosts **cannot initiate** new inbound connections to private resources

---

## AWS Managed Service

The NAT Gateway is a **fully managed** AWS service. AWS handles:

- Provisioning
- Maintenance
- Availability
- Scaling

You only need to create and configure it.

---

## High Availability

> [!warning] A NAT Gateway is NOT Region-resilient
> Unlike an Internet Gateway, a NAT Gateway is deployed in a **single Availability Zone**. If that AZ fails, the NAT Gateway fails too.

To achieve high availability across multiple AZs:

- Deploy **one NAT Gateway per Availability Zone**
- Each private subnet should route to the NAT Gateway **in the same AZ** — this avoids cross-AZ dependencies and unnecessary data transfer charges

```
AZ-A                          AZ-B
┌─────────────────────┐       ┌─────────────────────┐
│  Public Subnet      │       │  Public Subnet       │
│  ┌──────────────┐  │       │  ┌──────────────┐   │
│  │  NAT GW (A)  │  │       │  │  NAT GW (B)  │   │
│  └──────────────┘  │       │  └──────────────┘   │
│                     │       │                      │
│  Private Subnet     │       │  Private Subnet      │
│  (routes to NAT-A)  │       │  (routes to NAT-B)   │
└─────────────────────┘       └─────────────────────┘
```

---

## Additional Notes

| Property | Detail |
|---|---|
| Elastic IP | A NAT Gateway **requires** an Elastic IP (EIP) |
| Bandwidth | Starts at **5 Gbps**, automatically scales up to **100 Gbps** |

---

## Pricing

| Dimension | Charged |
|---|---|
| Usage | Per hour the NAT Gateway is running |
| Data processing | Per GB of data processed |

> [!warning] NAT Gateway costs can add up
> Unlike an Internet Gateway (which has no hourly charge), a NAT Gateway incurs both an hourly fee and a data processing fee. For cost-sensitive workloads, consider whether outbound internet access from private subnets is actually required.
