---
title: "03 - Application Load Balancer (ALB)"
description: "ALB routing capabilities, SSL termination, health checks, and when to use it."
tags:
  - aws
  - elb
  - networking
  - load-balancing
  - alb
---

The **Application Load Balancer (ALB)** operates at **Layer 7 (Application Layer)** of the OSI model.

Supported protocols:

- HTTP
- HTTPS
- WebSockets

> [!important] ALB is HTTP/HTTPS only
> If your application uses protocols other than HTTP or HTTPS (e.g. raw TCP or UDP), use a **Network Load Balancer** instead.

---

## Advanced Routing

An ALB can route traffic based on:

| Criteria | Example |
|---|---|
| URL path | `/api/*` → API service, `/static/*` → CDN |
| Host name | `app.example.com` → App, `admin.example.com` → Admin |
| HTTP headers | Route based on `User-Agent` or custom headers |
| Query string parameters | `?version=v2` → new backend |
| HTTP method | `GET` vs `POST` to different targets |
| Source IP address | Route by client IP range |

This enables patterns such as:

- Routing different URL paths to different microservices
- Hosting multiple websites behind a single load balancer
- Implementing API versioning

---

## Additional Features

| Feature | Detail |
|---|---|
| HTTP redirects | Redirect HTTP → HTTPS automatically |
| Custom HTTP responses | Return fixed responses without hitting a backend |
| Health checks | Continuously monitor targets; unhealthy targets are removed automatically |
| SSL/TLS termination | HTTPS connection is decrypted at the ALB |
| Multiple certificates | Supports SNI — multiple domains and certificates on a single ALB |

---

## SSL/TLS Termination

SSL/TLS certificates are installed on the ALB. The encrypted HTTPS connection is terminated at the load balancer, and requests are forwarded to backend instances over HTTP (or HTTPS if end-to-end encryption is required).

```
Client
  │ HTTPS
  ▼
ALB  ← SSL certificate installed here
  │ HTTP (or HTTPS for end-to-end encryption)
  ▼
EC2 Instances
```

> [!tip] End-to-end encryption
> If end-to-end encryption is required, install certificates on the backend EC2 instances and configure the ALB to forward requests over HTTPS. This adds complexity but ensures the traffic is encrypted at every hop.
