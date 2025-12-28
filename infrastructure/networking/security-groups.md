# Security Groups Configuration

This document describes the **security group design** used in the AWS web stack.  
Security groups are used to strictly control traffic flow between the internet, application layer, and backend services.

---

## Overview

The architecture uses **three security groups**:

1. Application Load Balancer (ALB)
2. Tomcat Application Servers
3. Backend Services (MySQL, Memcached, RabbitMQ)

This design ensures **layer isolation** and minimizes the attack surface.

---

## 1. ALB Security Group

### Inbound Rules
- HTTPS (443) from `0.0.0.0/0`

### Outbound Rules
- HTTP (8080) to Tomcat Security Group

**Purpose:**  
Allows secure public access while restricting traffic to the application layer only.

---

## 2. Tomcat Security Group

### Inbound Rules
- HTTP (8080) from ALB Security Group
- SSH (22) from trusted IP (for validation)

### Outbound Rules
- MySQL (3306) to Backend Security Group
- Memcached (11211) to Backend Security Group
- RabbitMQ (5672) to Backend Security Group

**Purpose:**  
Ensures Tomcat is reachable only through the ALB and can communicate with backend services.

---

## 3. Backend Services Security Group

Applied to:
- MySQL
- Memcached
- RabbitMQ

### Inbound Rules
- MySQL (3306) from Tomcat Security Group
- Memcached (11211) from Tomcat Security Group
- RabbitMQ (5672) from Tomcat Security Group

### Outbound Rules
- All traffic allowed (internal communication)

**Purpose:**  
Protects backend services from direct access and allows communication only from the application layer.

---

## Traffic Flow Summary

1. Internet → ALB (HTTPS 443)
2. ALB → Tomcat (HTTP 8080)
3. Tomcat → Backend Services (internal ports only)

No backend component is publicly exposed.

---

## Security Principles Applied

- Least privilege access
- No public access to application or backend instances
- Clear separation between layers
- Security groups referenced by **group ID**, not IP addresses

---

## Notes

- All security groups are created in the **default VPC**
- No inbound rules allow public access to EC2 instances
- SSH access is limited and used only for validation
