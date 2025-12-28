# Auto Scaling Configuration

This document explains how **Auto Scaling** is used to maintain availability and handle load for the Tomcat application servers.

---

## Overview

- Auto Scaling is applied **only to the Tomcat layer**
- Managed using an **Auto Scaling Group (ASG)**
- Instances are created using a **Launch Template**
- Health is monitored through the **Application Load Balancer**

---

## Auto Scaling Group Settings

- Minimum capacity: **1**
- Desired capacity: **2**
- Maximum capacity: **4**
- VPC: **Default VPC**
- Load balancer integration:
  - Tomcat Target Group

The ASG ensures the application remains available even if instances fail.

---

## Instance Lifecycle

1. ASG launches a new EC2 instance using the launch template
2. User Data script installs and configures Tomcat
3. Application artifact is pulled from S3
4. Instance registers with the ALB target group
5. Health checks determine traffic eligibility

Unhealthy instances are terminated and replaced automatically.

---

## Health Checks

- Type: **ELB health checks**
- Based on target group health status
- Instances failing health checks are removed

This ensures traffic is routed only to healthy application servers.

---

## Scaling Behavior

- Scaling is capacity-based (static limits)
- No dynamic scaling policies are configured
- Designed for:
  - Simplicity
  - Predictable behavior
  - Learning-focused architecture

---

## Benefits

- High availability
- Fault tolerance
- Zero manual intervention on instance failure
- Seamless integration with ALB

---

## Notes

- Backend services are not auto-scaled
- ASG uses the same AMI and configuration for all instances
- New instances are production-ready at launch
