# Launch Template Configuration

This document outlines the configuration of the **Launch Template** used to provision Tomcat EC2 instances within the Auto Scaling Group.

---

## Overview

- Defines instance settings used by the ASG for Tomcat servers
- Ensures consistency across all launched instances
- Includes instance type, AMI, security groups, IAM role, and user data

---

## Key Settings

| Setting           | Value                          |
|-------------------|--------------------------------|
| AMI               | Ubuntu Server (latest LTS)     |
| Instance Type     | t2.micro                       |
| Key Pair          | Configured for SSH access      |
| Security Groups   | Tomcat Security Group          |
| IAM Role         | Allows S3 access for artifact  |
| User Data         | Script to install & configure Tomcat, and deploy app |

---

## User Data

- Bootstraps the instance on launch
- Updates OS packages
- Installs OpenJDK 17 and Tomcat 10
- Pulls application artifact from S3 using IAM Role
- Starts the Tomcat service ready for production

---

## Benefits

- Automates instance setup
- Guarantees environment consistency
- Enables smooth scaling and recovery
- Secures S3 access without hardcoded credentials

---

## Notes

- The AMI is kept up-to-date regularly
- The launch template can be versioned for updates
- All instances launched inherit the exact configuration
