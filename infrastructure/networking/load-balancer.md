# Application Load Balancer Configuration

This document describes the configuration of the **Application Load Balancer (ALB)** used to securely expose the application and distribute traffic across Tomcat instances.

---

## Overview

- Type: **Application Load Balancer**
- Scheme: **Internet-facing**
- Network: **Default VPC**
- Purpose:
  - Terminate HTTPS
  - Route traffic to the application layer
  - Improve availability and scalability

The ALB is the **only publicly accessible component** in the architecture.

---

## Listener Configuration

### HTTPS Listener
- Port: **443**
- Protocol: **HTTPS**
- SSL Certificate:
  - Issued via **AWS Certificate Manager (ACM)**
- Default Action:
  - Forward traffic to Tomcat Target Group

All client traffic is encrypted in transit.

---

## Target Group Configuration

- Target type: **Instance**
- Protocol: **HTTP**
- Port: **8080**
- Health check:
  - Protocol: HTTP
  - Port: 8080
  - Path: `/`
- Registered targets:
  - Tomcat EC2 instances (via Auto Scaling Group)

Only healthy instances receive traffic.

---

## Listener Rules

- Default rule forwards all requests to the Tomcat target group
- No path-based or host-based routing is used

The setup is intentionally simple and production-aligned.

---

## Security Integration

- ALB Security Group:
  - Allows inbound HTTPS (443) from the internet
  - Allows outbound traffic to Tomcat Security Group
- Tomcat instances do not accept public traffic directly

---

## DNS Integration

- ALB DNS name is mapped to a **custom domain**
- Domain is registered on **Namecheap**
- DNS records point the domain to the ALB

This allows users to access the application via a friendly domain name.

---

## Role in the Architecture

The ALB provides:
- Secure HTTPS entry point
- Load distribution across instances
- Health-based traffic routing
- Tight integration with Auto Scaling

---

## Notes

- HTTP (80) listener is not enabled
- TLS termination happens at the ALB
- Backend communication uses HTTP within the VPC
