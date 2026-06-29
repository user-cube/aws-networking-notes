---
title: "02 - Classic Load Balancer (CLB)"
description: "Overview of the Classic Load Balancer, its limitations, and why it should not be used for new workloads."
tags:
  - aws
  - elb
  - networking
  - load-balancing
---

The **Classic Load Balancer (CLB)** was the first load balancer introduced by AWS. It is now considered a **legacy service** and is generally not recommended for new applications.

> [!warning] Do not use for new workloads
> AWS recommends using an **Application Load Balancer** or **Network Load Balancer** for all new applications. The CLB exists primarily to support older workloads that have not yet been migrated.

---

## Limitations

| Limitation | Detail |
|---|---|
| Single SSL/TLS certificate | Cannot host multiple HTTPS applications with different domain names and certificates on a single CLB |
| Limited routing | No content-based or path-based routing |
| Fewer features | Lacks the advanced capabilities of ALB and NLB |
