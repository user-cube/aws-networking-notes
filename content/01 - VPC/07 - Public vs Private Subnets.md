---
title: "07 - Public vs Private Subnets"
description: "When to use public vs private subnets, common examples, and a typical two-tier architecture."
tags:
  - aws
  - vpc
  - networking
  - subnets
---

When designing a network architecture, the first question should be:

> **Does this service need to be accessible from the internet?**

---

## Decision Guide

| Answer | Subnet Type | Examples |
|---|---|---|
| **Yes** | Public Subnet | Web servers, public APIs, bastion hosts, internet-facing load balancers |
| **No** | Private Subnet | Databases, internal application servers, cache clusters, backend services |

Keeping internal services inside private subnets significantly improves the security posture of the infrastructure.

---

## Common Two-Tier Architecture

```
Internet
    │
    ▼
┌──────────────────────┐
│     Public Subnet    │
│      Web Server      │  ← publicly accessible
└──────────────────────┘
           │
           ▼
┌──────────────────────┐
│    Private Subnet    │
│       Database       │  ← only reachable from the web server
└──────────────────────┘
```

In this design:

- The **Web Server** is publicly accessible from the internet
- The **Database** is isolated inside a private subnet
- Only the Web Server is permitted to communicate with the Database (enforced via Security Groups)

This is one of the most widely used patterns in AWS architectures.
