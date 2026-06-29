---
title: "15 - VPC Endpoints"
description: "Gateway and Interface endpoints — accessing AWS services privately without an internet gateway or NAT."
tags:
  - aws
  - vpc
  - networking
  - vpc-endpoints
---

By default, traffic from a VPC to AWS services (S3, DynamoDB, SSM, etc.) exits the VPC and travels over the public internet — even if it stays within AWS's network. **VPC Endpoints** allow resources inside a VPC to reach AWS services **privately**, without requiring an Internet Gateway, NAT Gateway, or public IP.

There are two types of VPC Endpoints:

| Type | Mechanism | Services |
|---|---|---|
| **Gateway Endpoint** | Route Table entry | S3, DynamoDB only |
| **Interface Endpoint** | Elastic Network Interface (ENI) in your subnet | Most other AWS services |

---

## Gateway Endpoints

A Gateway Endpoint is a **route table target** — you add a route that points traffic for the service's prefix list to the endpoint, rather than to the internet.

```
Private Subnet → Route Table
  Destination: pl-xxxxxxxx (S3 prefix list)
  Target: vpce-xxxxxxxx (Gateway Endpoint)
```

Characteristics:

| Property | Detail |
|---|---|
| Cost | **Free** — no hourly charge |
| Supported services | S3 and DynamoDB only |
| Availability | Highly available within the Region |
| Scope | VPC-level — the route is added to selected route tables |
| DNS | No private DNS name — uses public S3/DynamoDB endpoints |

> [!tip] Always use a Gateway Endpoint for S3 and DynamoDB
> They are free, easy to configure, and eliminate the need for a NAT Gateway just to access S3 or DynamoDB from private subnets — which can result in significant cost savings.

---

## Interface Endpoints (powered by AWS PrivateLink)

An Interface Endpoint creates an **ENI with a private IP address** in your subnet. That ENI acts as the entry point for traffic to the target AWS service.

```
Private Subnet
  └── Interface Endpoint ENI (private IP: 10.0.1.45)
        └── AWS Service (SSM, SNS, SQS, Secrets Manager, etc.)
```

Characteristics:

| Property | Detail |
|---|---|
| Cost | Hourly charge + data processing fee |
| Supported services | Most AWS services (SSM, Secrets Manager, SNS, SQS, CloudWatch, ECR, and many more) |
| Availability | Per-AZ — deploy one per AZ for resilience |
| DNS | Creates **private DNS names** — existing service calls work without code changes |
| Security | Controlled via **Endpoint Policies** (resource-based policies) and Security Groups |

### Private DNS

When you enable private DNS for an Interface Endpoint, AWS creates a private hosted zone in Route 53 that resolves the public service endpoint (e.g. `ssm.eu-west-1.amazonaws.com`) to the private IP of your ENI. This means no code changes are needed — existing SDK or CLI calls automatically use the private path.

---

## Endpoint Policies

Both Gateway and Interface endpoints support **Endpoint Policies** — IAM-style resource policies that control which principals can use the endpoint and which actions are allowed.

Example — restrict an S3 Gateway Endpoint to a specific bucket:

```json
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": [
      "arn:aws:s3:::my-private-bucket",
      "arn:aws:s3:::my-private-bucket/*"
    ]
  }]
}
```

---

## Without vs With VPC Endpoints

```
Without endpoint:
  EC2 (private subnet) → NAT Gateway → Internet Gateway → S3

With Gateway Endpoint:
  EC2 (private subnet) → Gateway Endpoint → S3
  (stays within AWS network, no NAT Gateway needed)
```
