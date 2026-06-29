---
title: "05 - Creating a Public Subnet"
description: "Step-by-step process to make a subnet public, and how AWS handles public/private IP translation."
tags:
  - aws
  - vpc
  - networking
  - subnets
---

## Steps to Create a Public Subnet

1. **Create an Internet Gateway (IGW)**
2. **Attach the IGW** to the VPC
3. **Create a custom Route Table**
4. **Add a default route** — destination `0.0.0.0/0`, target: the IGW
5. **Associate the Route Table** with the subnet

> [!important] Auto-assign Public IPv4
> By default, resources inside a VPC receive only a **private IP address**. To allow inbound internet access, you must either:
> - Enable **Auto-assign Public IPv4 Address** on the subnet, so all new resources get a public IP automatically
> - Manually assign a **public IP or Elastic IP** to individual instances

---

## Public and Private IP Address Translation

When a public IP is assigned to a resource, AWS maintains a **permanent one-to-one mapping** between the public and private addresses for as long as the public IP remains assigned.

When a client connects to the public IP:

1. AWS performs **Network Address Translation (NAT)** at the Internet Gateway
2. Incoming traffic is translated from the public IP → instance's private IP
3. Outbound responses are translated back: private IP → public IP

> [!note] The instance only sees its private IP
> The OS inside the instance is never directly aware of its public IP — that translation happens transparently at the IGW level.
