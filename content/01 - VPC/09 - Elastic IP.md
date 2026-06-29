---
title: "09 - Elastic IP (EIP)"
description: "What an Elastic IP is, its benefits, pricing model, and key constraints."
tags:
  - aws
  - vpc
  - networking
  - elastic-ip
---

By default, a public IPv4 address assigned to an EC2 instance is **not permanent**. If the instance is stopped and restarted, AWS may assign a different public IP address.

An **Elastic IP (EIP)** is a **static public IPv4 address** allocated to your AWS account that can be associated with supported AWS resources.

---

## Benefits

- Provides a **permanent** public IP address
- Can be **reassigned** to another resource when needed
- Useful for **failover and disaster recovery** — traffic can be shifted to a replacement instance without changing DNS records or firewall rules
- Allows DNS records and security rules to remain unchanged when replacing an instance

### Example — Zero-Downtime Instance Replacement

1. Launch a new EC2 instance
2. Reassociate the Elastic IP with the new instance
3. Clients continue using the same public IP — no changes required on their end

---

## Pricing

| Scenario | Charged? |
|---|---|
| EIP associated with a running resource | Generally **not charged** |
| EIP allocated but **not associated** with any resource | **Charged** (AWS discourages address hoarding) |
| Multiple EIPs beyond the default allowance | Additional charges apply |

---

## Key Notes

- Elastic IPs are **Regional** resources
- An EIP must be **allocated to your account** before it can be associated with an instance
- There is a default limit of **5 EIPs per Region** (can be increased via a Service Quota request)
