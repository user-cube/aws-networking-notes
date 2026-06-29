---
title: "03 - Route 53 Health Checks"
description: "How Route 53 health checks work, types, calculated health checks, and integration with routing policies."
tags:
  - aws
  - route53
  - dns
  - networking
  - health-checks
---

**Route 53 Health Checks** continuously monitor the health of endpoints and can automatically remove unhealthy records from DNS responses.

Health checks are independent of DNS records — they are created separately and then associated with routing policy records.

---

## Types of Health Checks

### Endpoint Health Checks

Monitor a specific endpoint directly:

| Protocol | Detail |
|---|---|
| HTTP / HTTPS | Route 53 sends an HTTP request; expects a 2xx or 3xx response |
| TCP | Route 53 establishes a TCP connection |

Configuration options:

- **IP address or hostname** to monitor
- **Port** and **path** (for HTTP/HTTPS)
- **Interval**: 30 seconds (standard) or 10 seconds (fast — higher cost)
- **Failure threshold**: number of consecutive failures before marking as unhealthy (default: 3)

> [!important] Health checkers come from public AWS IPs
> Route 53 health checkers are located in multiple AWS Regions and connect from **public IP addresses**. If you are monitoring a resource inside a private subnet, you must use **CloudWatch alarms** instead (see below).

---

### Calculated Health Checks

Aggregate the results of multiple child health checks into a single parent health check.

- Define how many child checks must be healthy for the parent to be healthy (e.g. at least 2 of 3)
- Useful for monitoring multiple components of a service and combining them into one DNS health signal

---

### CloudWatch Alarm Health Checks

Route 53 monitors the state of a **CloudWatch Alarm** and marks the health check as healthy or unhealthy based on the alarm's state.

This is the recommended approach for **private resources** (inside a VPC) that are not reachable from Route 53's public health checkers.

```
Private EC2 → CloudWatch metric → CloudWatch Alarm → Route 53 Health Check
```

---

## Integration with Routing Policies

Health checks are used by:

| Routing Policy | Health Check Role |
|---|---|
| **Failover** | Required — triggers automatic failover when primary is unhealthy |
| **Weighted** | Optional — removes unhealthy records from rotation |
| **Latency** | Optional — excludes unhealthy endpoints |
| **Geolocation** | Optional — falls back to default if regional endpoint is unhealthy |
| **Multi-Value** | Optional — only returns healthy records |

> [!tip] Health checks make DNS resilient
> Combining routing policies with health checks transforms Route 53 from a simple DNS service into a basic traffic management and failover system — without needing a load balancer for DNS-level routing.
