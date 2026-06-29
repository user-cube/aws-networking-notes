---
title: "17 - VPC Flow Logs"
description: "What VPC Flow Logs capture, log levels, destinations, log format, and common use cases."
tags:
  - aws
  - vpc
  - networking
  - observability
  - flow-logs
---

**VPC Flow Logs** capture metadata about the IP traffic flowing through network interfaces in your VPC. They are the primary tool for **network visibility and troubleshooting** in AWS.

> [!note] Flow Logs capture metadata, not packet contents
> Flow Logs record information about connections (source, destination, port, protocol, bytes, action) but do **not** capture the actual payload of packets.

---

## Scope Levels

Flow Logs can be enabled at three levels:

| Level | Captures traffic for |
|---|---|
| **VPC** | All ENIs in the entire VPC |
| **Subnet** | All ENIs in a specific subnet |
| **ENI** | A single network interface |

---

## Log Destinations

Captured logs can be sent to:

| Destination | Use Case |
|---|---|
| **CloudWatch Logs** | Real-time monitoring, metric filters, alarms |
| **Amazon S3** | Long-term storage, cost-effective, Athena queries |
| **Amazon Data Firehose** | Near-real-time streaming to analytics services |

---

## Log Record Format

Each flow log record contains fields about a single network flow. Key fields:

| Field | Description |
|---|---|
| `srcaddr` | Source IP address |
| `dstaddr` | Destination IP address |
| `srcport` | Source port |
| `dstport` | Destination port |
| `protocol` | IANA protocol number (6 = TCP, 17 = UDP, 1 = ICMP) |
| `packets` | Number of packets transferred |
| `bytes` | Number of bytes transferred |
| `action` | `ACCEPT` or `REJECT` |
| `log-status` | `OK`, `NODATA`, or `SKIPDATA` |
| `start` / `end` | Unix timestamps of the flow |

### Example Record

```
2 123456789012 eni-abc12345 10.0.1.5 10.0.2.10 54321 443 6 10 2500 1625000000 1625000060 ACCEPT OK
```

---

## What Flow Logs Do Not Capture

Flow Logs do **not** record traffic for:

- Amazon DNS queries (traffic to the VPC+2 DNS resolver)
- Amazon DHCP traffic
- Instance metadata service (IMDS) traffic (`169.254.169.254`)
- Amazon Time Sync Service (`169.254.169.123`)
- Windows license activation traffic
- Traffic to and from the VPC Router

---

## Common Use Cases

| Use Case | How |
|---|---|
| **Security analysis** | Identify unexpected connections or port scans by looking for `REJECT` records |
| **Troubleshooting connectivity** | Determine whether traffic is reaching an instance and whether it is being accepted or rejected |
| **Network monitoring** | Track traffic volumes between services |
| **Compliance auditing** | Retain network traffic records for audit requirements |

> [!tip] Query Flow Logs with Athena
> Storing Flow Logs in S3 and querying with Amazon Athena is a cost-effective pattern for ad-hoc analysis of large volumes of network logs without needing a dedicated log management tool.
