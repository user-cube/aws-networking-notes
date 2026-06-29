# AWS Network Fundamentals Notes

Personal study notes on AWS networking concepts, services, and architecture patterns, built with [Quartz v5](https://quartz.jzhao.xyz/).

Published at: [aws-network-fundamentals-notes.ruicoelho.dev](https://aws-network-fundamentals-notes.ruicoelho.dev)

---

## Contents

### 01 — VPC

| Page | Summary |
|---|---|
| 01 - Introduction to VPC | What a VPC is, CIDR blocks, Default vs Custom VPC |
| 02 - Subnets | Subnet basics, reserved IPs |
| 03 - VPC Routing and Route Tables | VPC Router, route tables, longest prefix match, local route |
| 04 - Internet Gateway | How to make a subnet public, IGW constraints |
| 05 - Creating a Public Subnet | Step-by-step guide, auto-assign public IPv4, NAT at the IGW |
| 06 - NAT Gateway | Outbound-only internet access, HA design, pricing |
| 07 - Public vs Private Subnets | Decision guide, two-tier architecture pattern |
| 08 - DNS inside a VPC | Route 53 Resolver, Enable DNS Support, Enable DNS Hostnames |
| 09 - Elastic IP | Static public IPs, failover use case, pricing |
| 10 - Stateless vs Stateful Firewalls | Conceptual difference, ephemeral ports |
| 11 - Security Groups | Stateful firewall, allow-only rules, multiple SGs |
| 12 - Network ACLs | Stateless firewall, rule order, NACL associations |
| 13 - Elastic Network Interface | ENI attributes, primary vs secondary, failover, source/dest check |
| 14 - VPC Peering | Cross-VPC connectivity, no transitive routing, CIDR constraints |
| 15 - VPC Endpoints | Gateway endpoints (S3/DynamoDB), Interface endpoints, private DNS |
| 16 - Transit Gateway | Hub-and-spoke connectivity, TGW route tables, VPC Peering vs TGW |
| 17 - VPC Flow Logs | Network visibility, log format, destinations, use cases |

### 02 — Elastic Load Balancers

| Page | Summary |
|---|---|
| 01 - Introduction to ELB | ELB types overview, Listeners, Target Groups |
| 02 - Classic Load Balancer | Legacy service, limitations |
| 03 - Application Load Balancer | Layer 7 routing, SSL termination, SNI, health checks |
| 04 - Network Load Balancer | Layer 4, high performance, TCP/UDP, TLS passthrough |
| 05 - Deploying Load Balancers | Multi-AZ, cross-zone balancing, public vs private LB |

### 03 — Route 53

| Page | Summary |
|---|---|
| 01 - Introduction to Route 53 | DNS functions, hosted zones, record types, CNAME vs Alias, TTL |
| 02 - Routing Policies | Simple, Weighted, Latency, Failover, Geolocation, Geoproximity, Multi-Value, IP-Based |
| 03 - Health Checks | Endpoint, calculated, CloudWatch alarm health checks |

### 04 — Hybrid Connectivity

| Page | Summary |
|---|---|
| 01 - Site-to-Site VPN | IPSec tunnels, VGW, CGW, dual tunnels, BGP routing |
| 02 - Direct Connect | Dedicated link, VIFs, redundancy patterns, Direct Connect Gateway |

---

## Tech Stack

Built with [Quartz v5](https://quartz.jzhao.xyz/) and deployed to GitHub Pages via GitHub Actions.
