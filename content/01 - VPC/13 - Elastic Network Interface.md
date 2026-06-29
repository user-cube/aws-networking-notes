---
title: "13 - Elastic Network Interface (ENI)"
description: "What an ENI is, its attributes, how it enables advanced network patterns, and key use cases."
tags:
  - aws
  - vpc
  - networking
  - eni
---

An **Elastic Network Interface (ENI)** is a virtual network card that can be attached to an EC2 instance. It is the fundamental building block of networking in AWS — every EC2 instance has at least one ENI.

---

## ENI Attributes

Each ENI can have:

| Attribute | Detail |
|---|---|
| Primary private IPv4 address | Always present — from the subnet's CIDR range |
| Secondary private IPv4 addresses | One or more additional private IPs |
| Public IPv4 address | Optional — one per private IP |
| Elastic IP (EIP) | One per private IPv4 address |
| IPv6 addresses | One or more, if the VPC supports IPv6 |
| MAC address | Fixed for the lifetime of the ENI |
| Security Groups | One or more attached Security Groups |
| Source/destination check | Can be disabled (required for NAT instances) |

---

## Primary vs Secondary ENIs

Every EC2 instance has a **primary ENI** (eth0), created automatically at launch and always attached to the instance. It cannot be detached while the instance is running.

**Secondary ENIs** (eth1, eth2, ...) can be:

- Attached to an instance at launch or at any time while running
- Detached and reattached to a different instance
- Used to enable multiple network interfaces on a single instance

---

## ENI Lifecycle

ENIs are **independent resources** — they exist separately from instances.

- An ENI created manually persists after the instance it was attached to is terminated
- An ENI created automatically by AWS at launch is terminated with the instance (unless configured otherwise)

---

## Key Use Cases

### Multiple IP Addresses on One Instance

A single instance can handle traffic for multiple private IPs or multiple public IPs — useful for hosting multiple SSL certificates or multiple services on the same host.

### Network Appliances

Instances acting as firewalls, proxies, or NAT devices typically have multiple ENIs — one facing each network segment.

### Low-Latency Failover

An ENI (with its private IP and Elastic IP) can be rapidly moved from a failed instance to a standby instance. Since the IP addresses move with the ENI, clients see no change.

```
Primary instance fails
    → Detach ENI from failed instance
    → Attach ENI to standby instance
    → Same private IP, same EIP, same MAC address
```

### Licensing Based on MAC Address

Some software licenses are tied to a MAC address. Since an ENI's MAC address is fixed, it can be moved between instances to keep the license valid.

---

## Source/Destination Check

By default, AWS verifies that an ENI is either the source or the destination of every packet it handles. Traffic that is neither sourced from nor destined for the ENI is dropped.

This check must be **disabled** for instances that act as routers or NAT devices, since they forward traffic that is not addressed to them.
