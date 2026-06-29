---
title: "04 - Network Load Balancer (NLB)"
description: "NLB characteristics, performance, health checks, SSL handling, and when to use it."
tags:
  - aws
  - elb
  - networking
  - load-balancing
  - nlb
---

The **Network Load Balancer (NLB)** operates at **Layer 4 (Transport Layer)** of the OSI model.

Supported protocols:

- TCP
- UDP

Unlike an ALB, the NLB does **not** inspect HTTP requests. It forwards packets based purely on network-level information (IP address and port).

---

## Key Characteristics

| Property | Detail |
|---|---|
| Performance | Extremely high — capable of handling **millions of requests per second** |
| Latency | Very low latency |
| Client IP preservation | Forwards the **original client IP address** to backend targets |
| Use case | Non-HTTP workloads, gaming, IoT, real-time streaming |

> [!tip] When to choose NLB over ALB
> Choose an NLB when you need maximum performance, when the protocol is not HTTP/HTTPS, or when you need to preserve the original client IP at the instance level.

---

## Health Checks

Health checks can be performed using:

- TCP
- HTTP
- HTTPS

Only healthy targets receive traffic.

---

## SSL/TLS Handling

Unlike the ALB, an NLB generally **forwards encrypted traffic directly to backend instances** without terminating it at the load balancer.

```
Client
  │ HTTPS/TLS
  ▼
NLB  ← passes traffic through without decrypting
  │ HTTPS/TLS
  ▼
EC2 Instances  ← SSL certificate installed here
```

This preserves end-to-end encryption by default, but requires certificates to be managed on each backend instance.
