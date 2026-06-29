---
title: "10 - Stateless vs Stateful Firewalls"
description: "The conceptual difference between stateless and stateful firewalls, and how it applies to NACLs and Security Groups."
tags:
  - aws
  - vpc
  - networking
  - firewalls
  - security
---

AWS provides two main firewall mechanisms that operate at different layers:

| Firewall | Layer | Type |
|---|---|---|
| **Security Groups** | Resource level (EC2, RDS, ENI, etc.) | Stateful |
| **Network ACLs (NACLs)** | Subnet level | Stateless |

Both filter network traffic but behave very differently.

---

## Stateless Firewalls

A stateless firewall **treats every packet independently**, with no memory of previous packets.

- Inbound and outbound rules are evaluated separately for every packet
- Allowing inbound traffic does **not** automatically allow the return traffic
- **Both directions must be explicitly configured**

### Example — HTTPS request to a server

```
Client → Server  (TCP port 443)
Server → Client  (response on an ephemeral port, typically 1024–65535)
```

With a stateless firewall:

| Direction | Rule required |
|---|---|
| Inbound | Allow TCP 443 |
| Outbound | Allow TCP 1024–65535 (ephemeral ports) |

If the outbound rule is missing, the response never reaches the client even though the inbound was allowed.

---

## Stateful Firewalls

A stateful firewall **tracks established connections**. Once a request is allowed in one direction, the response is automatically permitted — no separate rule needed.

- Much simpler to configure
- Return traffic is always allowed for permitted connections

### Same Example — HTTPS request to a server

| Direction | Rule required |
|---|---|
| Inbound | Allow TCP 443 |
| Outbound | *(no rule needed — return traffic is automatic)* |

---

## Summary

| Property | Stateless | Stateful |
|---|---|---|
| Tracks connection state | No | Yes |
| Return traffic | Must be explicitly allowed | Automatically allowed |
| AWS implementation | Network ACLs | Security Groups |
| Rule complexity | Higher | Lower |
