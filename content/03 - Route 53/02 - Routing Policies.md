---
title: "02 - Route 53 Routing Policies"
description: "All Route 53 routing policies explained — when to use each and how they behave."
tags:
  - aws
  - route53
  - dns
  - networking
---

A **Routing Policy** determines how Route 53 responds to DNS queries for a record. Different policies enable different traffic distribution and failover strategies.

---

## Simple

Returns a single value (or multiple values) for a record. When multiple values are returned, the client chooses one at random.

- No health checks
- No intelligent routing
- Use for a single resource with a known address

```
query: app.example.com → 1.2.3.4
```

---

## Weighted

Distributes traffic across multiple resources according to assigned weights.

```
app.example.com
  → 1.2.3.4  (weight 70)  — receives 70% of traffic
  → 5.6.7.8  (weight 30)  — receives 30% of traffic
```

Use cases:

- **A/B testing** — send a small percentage of traffic to a new version
- **Gradual migrations** — shift traffic incrementally between environments
- A weight of `0` removes the record from rotation without deleting it

---

## Latency-Based

Routes traffic to the AWS Region that provides the **lowest network latency** for the client.

```
Client in Europe  → eu-west-1 endpoint
Client in Asia    → ap-southeast-1 endpoint
```

> [!note] Latency ≠ geographic proximity
> Route 53 measures actual network latency, not physical distance. A client in one country may be routed to a Region in another country if it has lower latency.

---

## Failover

Routes traffic to a **primary** resource when it is healthy, and automatically switches to a **secondary** (standby) resource when the primary fails.

```
Primary:   app-primary.example.com  ← health check passing → traffic goes here
Secondary: app-dr.example.com       ← only used if primary is unhealthy
```

Requires health checks on the primary record. Commonly used for **Active-Passive** disaster recovery.

---

## Geolocation

Routes traffic based on the **geographic location of the client** (continent, country, or US state).

```
Clients from Germany    → EU endpoint
Clients from Brazil     → SA endpoint
All other clients       → Default endpoint
```

> [!important] Always configure a default record
> If no geolocation rule matches and there is no default record, Route 53 returns a "no answer" response.

Use cases: content localisation, regulatory compliance, regional restrictions.

---

## Geoproximity

Routes traffic based on the geographic location of resources and clients, with the ability to **shift traffic** by adjusting a **bias** value.

- Positive bias → expand the region's coverage area (more traffic)
- Negative bias → shrink coverage area (less traffic)

Requires **Route 53 Traffic Flow** (visual policy editor).

---

## Multi-Value Answer

Returns up to **8 healthy records** in response to a DNS query. The client selects one.

- Similar to Simple with multiple values, but supports **health checks**
- Unhealthy records are excluded from responses
- Not a replacement for a load balancer — the client handles the selection

---

## IP-Based

Routes traffic based on the **client's IP address** (CIDR ranges mapped to endpoints).

Use cases: route corporate users to internal endpoints, ISP-based routing.

---

## Summary Table

| Policy | Based On | Health Checks | Use Case |
|---|---|---|---|
| Simple | — | Optional | Single resource |
| Weighted | Assigned weight | Optional | A/B testing, gradual migration |
| Latency | Network latency | Optional | Route to lowest-latency Region |
| Failover | Health status | Required | Active-Passive DR |
| Geolocation | Client location | Optional | Content localisation, compliance |
| Geoproximity | Location + bias | Optional | Fine-grained geographic traffic shifting |
| Multi-Value | Health status | Optional | Basic client-side load balancing |
| IP-Based | Client CIDR | Optional | Corporate/ISP routing |
