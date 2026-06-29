---
title: "01 - Introduction to Route 53"
description: "What Route 53 is, its core functions, hosted zones, and record types."
tags:
  - aws
  - route53
  - dns
  - networking
---

**Amazon Route 53** is AWS's highly available and scalable **DNS (Domain Name System)** service. It translates human-readable domain names (e.g. `app.example.com`) into IP addresses that computers use to connect to resources.

Route 53 performs three main functions:

| Function | Description |
|---|---|
| **Domain registration** | Purchase and manage domain names directly through AWS |
| **DNS hosting** | Host DNS zones and answer queries for your domains |
| **Health checking** | Monitor the health of endpoints and route traffic accordingly |

---

## Hosted Zones

A **Hosted Zone** is a container for DNS records for a specific domain. There are two types:

| Type | Description |
|---|---|
| **Public Hosted Zone** | Answers DNS queries from the public internet |
| **Private Hosted Zone** | Answers DNS queries only from within one or more VPCs |

A hosted zone is created automatically when you register a domain through Route 53, or can be created manually for domains registered elsewhere.

---

## DNS Record Types

| Record | Purpose | Example |
|---|---|---|
| **A** | Maps a hostname to an IPv4 address | `app.example.com → 1.2.3.4` |
| **AAAA** | Maps a hostname to an IPv6 address | `app.example.com → 2001:db8::1` |
| **CNAME** | Maps a hostname to another hostname | `www.example.com → app.example.com` |
| **Alias** | AWS-specific — maps to AWS resources | `app.example.com → ALB DNS name` |
| **MX** | Mail server records | — |
| **TXT** | Text records — often used for domain verification | — |
| **NS** | Name server records — delegates a zone | — |
| **SOA** | Start of Authority — zone metadata | — |

### CNAME vs Alias

| | CNAME | Alias |
|---|---|---|
| Can point to | Any hostname | AWS resources (ALB, CloudFront, S3, etc.) |
| Works at zone apex (`example.com`) | ❌ No | ✅ Yes |
| DNS query charge | Yes | **No charge** for queries to AWS resources |
| TTL | Set manually | Managed by Route 53 |

> [!tip] Prefer Alias over CNAME for AWS resources
> Alias records are free, work at the zone apex, and automatically track the IP addresses of the target AWS resource.

---

## TTL (Time to Live)

TTL controls how long DNS resolvers cache a record before querying Route 53 again.

| TTL | Effect |
|---|---|
| High TTL (e.g. 24h) | Less Route 53 traffic, but DNS changes propagate slowly |
| Low TTL (e.g. 60s) | More Route 53 traffic, but changes propagate quickly |

> [!important] Plan TTL changes in advance
> Before making a DNS change (e.g. migrating to a new IP), lower the TTL well in advance so that cached records expire quickly once you make the switch.
