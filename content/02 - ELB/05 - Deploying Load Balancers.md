---
title: "05 - Deploying Load Balancers"
description: "Multi-AZ deployment requirements, cross-zone load balancing, and public vs private load balancers."
tags:
  - aws
  - elb
  - networking
  - load-balancing
  - high-availability
---

## Multi-AZ Deployment

A Load Balancer must be deployed across **at least two subnets in different Availability Zones**.

AWS creates a **Load Balancer Node** in every selected Availability Zone. Each node receives client requests and forwards them to backend targets. Deploying across multiple AZs increases availability and resilience.

---

## Cross-Zone Load Balancing

By default, each Load Balancer node only routes traffic to targets **in its own Availability Zone**. This can cause uneven load distribution if the number of instances varies between AZs.

### Without Cross-Zone Load Balancing

```
AZ-A (2 instances)          AZ-B (8 instances)
LB Node A                   LB Node B
  → instance-a1 (50%)         → instance-b1 (12.5%)
  → instance-a2 (50%)         → instance-b2 (12.5%)
                               → instance-b3 (12.5%)
                               ...
```

Instances in AZ-A receive a disproportionately high load.

### With Cross-Zone Load Balancing Enabled

```
AZ-A (2 instances)          AZ-B (8 instances)
LB Node A                   LB Node B
  → distributes evenly across all 10 instances (10% each)
```

Every Load Balancer node can send traffic to any registered target regardless of AZ, resulting in even distribution and better resource utilisation.

---

## Public vs Private Load Balancers

### Public Load Balancer

- Deployed in **public subnets**
- Has a **public IP address**
- Accepts traffic directly from the internet

Typical use cases: public websites, REST APIs, customer-facing applications

### Private Load Balancer

- Deployed in **private subnets**
- Has **no public IP address**
- Accessible only from within the VPC or through connected private networks (VPN, Direct Connect, Transit Gateway, VPC Peering)

Typical use cases: internal applications, microservices, backend APIs

> [!tip] Common pattern
> A typical architecture combines both: a **Public ALB** accepts internet traffic and forwards it to a **Private NLB or directly to private EC2 instances**, keeping backend services isolated from the internet.
