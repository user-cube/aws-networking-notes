---
title: "11 - Security Groups"
description: "How Security Groups work, their stateful behaviour, default rules, and how multiple Security Groups interact."
tags:
  - aws
  - vpc
  - networking
  - security
  - security-groups
---

A **Security Group** operates at the **resource level** and acts as a virtual firewall for individual AWS resources.

Resources that support Security Groups:

- EC2 Instances
- Application Load Balancers
- RDS Databases
- Elastic Network Interfaces (ENIs)
- And others

---

## Key Characteristics

| Property | Detail |
|---|---|
| Type | **Stateful** |
| Scope | Individual resource |
| Rule types | **Allow only** — there are no Deny rules |
| Return traffic | Automatically permitted for allowed connections |

---

## Default Behaviour

| Scenario | Behaviour |
|---|---|
| No inbound rules | All inbound traffic is **denied** |
| Inbound rule added | Only the specified traffic is allowed |
| Default outbound rule | **All outbound traffic allowed** (a catch-all allow-all rule exists by default) |

The default outbound allow-all rule can be removed if stricter outbound filtering is required.

---

## Multiple Security Groups

A resource can have **multiple Security Groups** attached simultaneously. AWS evaluates the **union of all Allow rules** across every attached Security Group — if any Security Group permits the traffic, it is allowed.

> [!tip] Use multiple Security Groups for modularity
> Rather than one large Security Group per instance, consider smaller purpose-built groups (e.g. one for SSH access, one for web traffic). This makes rules easier to audit and reuse across resources.

---

## Important Limitation

Security Groups contain **Allow rules only**. It is not possible to explicitly deny traffic with a Security Group. To block specific traffic, use a **Network ACL** instead.
