---
title: "12 - Network ACLs (NACLs)"
description: "How NACLs work, rule evaluation order, NACL associations, and what traffic NACLs cannot filter."
tags:
  - aws
  - vpc
  - networking
  - security
  - nacl
---

A **Network Access Control List (NACL)** operates at the **subnet level** and filters traffic entering and leaving a subnet.

---

## Key Characteristics

| Property | Detail |
|---|---|
| Type | **Stateless** |
| Scope | Entire subnet |
| Rule types | Both **Allow** and **Deny** |
| Return traffic | Must be **explicitly allowed** with outbound rules |
| Intra-subnet traffic | **Not filtered** — NACLs do not apply to traffic between resources within the same subnet |

---

## Rule Evaluation

Rules are evaluated in **ascending numerical order**. The first matching rule determines whether traffic is allowed or denied — evaluation stops immediately.

> [!important] Rule order matters
> A Deny rule with a lower number will block traffic even if an Allow rule with a higher number would have permitted it. Always plan rule numbering carefully.

Example — allowing HTTPS inbound while blocking a specific IP:

| Rule # | Type | Protocol | Port | Source | Action |
|---|---|---|---|---|---|
| 100 | Inbound | TCP | 443 | `203.0.113.5/32` | **DENY** |
| 200 | Inbound | TCP | 443 | `0.0.0.0/0` | ALLOW |

In this example, `203.0.113.5` is blocked even though rule 200 would allow all HTTPS traffic.

---

## NACL Associations

- Every subnet must be associated with **exactly one NACL**
- A single NACL can be associated with **multiple subnets**
- When a subnet is created, it is automatically associated with the **default NACL** of the VPC

---

## Traffic NACLs Cannot Filter

NACLs do **not** filter traffic to AWS infrastructure services. The following are always reachable regardless of NACL rules:

- Amazon DNS (`169.254.169.253` / VPC+2 address)
- Amazon DHCP
- EC2 Instance Metadata Service (IMDS — `169.254.169.254`)
- Amazon Time Sync Service
- Windows License Activation
- Other AWS-reserved addresses used by the VPC Router

---

## Security Groups vs NACLs — Quick Reference

| | Security Groups | NACLs |
|---|---|---|
| Scope | Resource | Subnet |
| State | Stateful | Stateless |
| Rule types | Allow only | Allow + Deny |
| Return traffic | Automatic | Must be explicit |
| Rule evaluation | All rules combined | In order — first match wins |
| Intra-subnet traffic | Applies | Does not apply |
