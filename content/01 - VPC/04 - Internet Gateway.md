---
title: "04 - Internet Gateway"
description: "What an Internet Gateway is, how it makes subnets public, and its key constraints."
tags:
  - aws
  - vpc
  - networking
  - internet-gateway
---

When a subnet is created, it is **private by default**. To make it public, an **Internet Gateway (IGW)** must be attached to the VPC.

---

## Key Characteristics

| Property | Detail |
|---|---|
| Region-resilient | Serves all Availability Zones in the Region — no per-AZ setup required |
| One per VPC | A VPC can have **at most one** Internet Gateway attached at a time |
| One VPC per IGW | An Internet Gateway can be attached to **only one** VPC |
| Default VPC | The Default VPC always has an IGW attached |

---

## Making a Subnet Public

Attaching an IGW to a VPC is not enough on its own. A subnet becomes public when **all three** of the following are in place:

1. An Internet Gateway is **attached to the VPC**
2. The subnet's **Route Table** has a route pointing internet-bound traffic (`0.0.0.0/0`) to the IGW
3. The resources in the subnet have **Security Group rules** (and NACL rules) that allow the relevant traffic

> [!warning] Without an Internet Gateway, every subnet in the VPC is private
> Even if a route table entry for `0.0.0.0/0` exists, traffic will not reach the internet unless an IGW is attached.
