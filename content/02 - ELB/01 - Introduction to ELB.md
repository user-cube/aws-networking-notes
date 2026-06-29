---
title: "01 - Introduction to Elastic Load Balancers"
description: "What ELBs are, the three types available, and when to use each."
tags:
  - aws
  - elb
  - networking
  - load-balancing
---

**Elastic Load Balancers (ELBs)** distribute incoming traffic across multiple backend resources, improving:

- **High Availability** — traffic is spread across multiple instances and Availability Zones
- **Fault Tolerance** — if one instance fails, traffic is automatically routed to healthy ones
- **Scalability** — backend capacity can grow or shrink without affecting clients

Instead of connecting directly to an EC2 instance, clients connect to the Load Balancer, which forwards requests to healthy targets. The application remains available even if individual instances fail.

---

## Types of Elastic Load Balancers

AWS provides three types of load balancers:

| Type | OSI Layer | Protocols | Best For |
|---|---|---|---|
| **Classic Load Balancer (CLB)** | 4 & 7 | HTTP, HTTPS, TCP | Legacy workloads only |
| **Application Load Balancer (ALB)** | Layer 7 | HTTP, HTTPS, WebSockets | Web apps, APIs, microservices |
| **Network Load Balancer (NLB)** | Layer 4 | TCP, UDP | High-performance, non-HTTP workloads |

---

## Core Architecture

Every ELB is composed of two main components:

### Listener

A Listener monitors incoming connections on a specific **protocol and port**.

| Example | Protocol | Port |
|---|---|---|
| HTTP listener | HTTP | 80 |
| HTTPS listener | HTTPS | 443 |

The Listener determines how incoming requests are processed and where they are forwarded.

### Target Group

A Target Group contains the backend resources that receive traffic.

Supported target types:

- EC2 Instances
- IP Addresses
- Lambda Functions

Health checks are configured at the Target Group level. The Load Balancer only forwards traffic to **healthy targets**.
