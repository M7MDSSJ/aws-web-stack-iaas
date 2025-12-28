# EC2 Instances Overview

This document details the configuration and role of the EC2 instances running the different components of the web stack.

---

## 1. Tomcat Instances

- OS: **Ubuntu (latest LTS)**
- Instance Type: **t2.micro**
- Role: Hosts Apache Tomcat 10 serving the Java web application
- Managed by Auto Scaling Group with Launch Template
- Initialized via User Data script to install Java, Tomcat, and deploy app from S3
- Security Group: Tomcat SG (HTTP 8080 from ALB)

---

## 2. MySQL Instance

- OS: **Amazon Linux 2023**
- Instance Type: **t2.micro**
- Role: Runs MariaDB serving as the primary database
- Initialized via User Data script to install MariaDB, secure DB, and restore application data
- Security Group: Backend Services SG (Port 3306 accessible only from Tomcat SG)

---

## 3. Memcached Instance

- OS: **Amazon Linux 2023**
- Instance Type: **t2.micro**
- Role: Runs Memcached for caching data
- Initialized via User Data script to install and configure Memcached
- Security Group: Backend Services SG (Port 11211 accessible only from Tomcat SG)

---

## 4. RabbitMQ Instance

- OS: **Amazon Linux 2023**
- Instance Type: **t2.micro**
- Role: Runs RabbitMQ message broker for asynchronous communication
- Initialized via User Data script to install and configure RabbitMQ
- Security Group: Backend Services SG (Port 5672 accessible only from Tomcat SG)

---

## Notes

- All instances are launched in the **default VPC**
- SSH access allowed only for validation, using key pairs
- Instances use private DNS names registered via Route 53 Private Hosted Zone for backend services
