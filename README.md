# AWS Web Stack Deployment (IaaS)

## 📌 Project Overview

This project demonstrates a **production-style web stack deployment on AWS using the Infrastructure as a Service (IaaS) model**.  
A Java web application is deployed on **Apache Tomcat**, backed by **MySQL**, **Memcached**, and **RabbitMQ**, with high availability and secure access using AWS managed services.

The project focuses on:
- Realistic cloud architecture
- Manual infrastructure design (no IaC tools)
- Security, scalability, and service isolation
- Clear documentation and reproducibility

---

## 🧱 Architecture Summary

- **Application Tier**
  - Apache Tomcat 10
  - Java 17
  - Deployed on EC2 (Ubuntu)
  - Managed by Auto Scaling Group
- **Backend Services**
  - MySQL (MariaDB)
  - Memcached
  - RabbitMQ
  - Each service runs on a dedicated EC2 instance (Amazon Linux 2023)
- **Traffic Management**
  - Application Load Balancer (ALB)
  - HTTPS (443) listener
  - Forwards traffic to Tomcat on HTTP (8080)
- **Security**
  - ACM SSL certificate attached to ALB
  - Strict Security Group rules
  - IAM roles and users with least privilege
- **DNS**
  - Public domain registered on Namecheap
  - ALB endpoint mapped to the domain
  - Route 53 Private Hosted Zone for internal service discovery
- **Artifact Management**
  - Java application built locally using Maven
  - Artifact uploaded to S3
  - Pulled by Tomcat instances using IAM Role

---

## 🖼 Architecture Diagram

![Architecture Diagram](architecture/architecture-diagram.png)

A detailed explanation of the architecture can be found in:  
`architecture/architecture-overview.md`

---

## ⚙️ Technology Stack

| Layer | Technology |
|-----|-----------|
| Cloud Provider | AWS |
| Compute | EC2 (t2.micro) |
| Load Balancing | Application Load Balancer |
| Scaling | Auto Scaling Group |
| Application Server | Apache Tomcat 10 |
| Language | Java 17 |
| Database | MySQL (MariaDB) |
| Caching | Memcached |
| Messaging | RabbitMQ |
| DNS | Route 53 (Public & Private Hosted Zones) |
| Certificate | AWS Certificate Manager (ACM) |
| Artifact Storage | Amazon S3 |
| OS | Ubuntu / Amazon Linux 2023 |

---

## 🔁 Auto Scaling Configuration (Tomcat)

- **Minimum instances:** 1  
- **Desired instances:** 2  
- **Maximum instances:** 4  

Traffic is automatically distributed across healthy instances using the ALB target group.

---

## 🔐 Security Overview

- ALB allows **HTTPS (443)** from the internet
- Tomcat instances allow **HTTP (8080)** only from ALB Security Group
- Backend services (MySQL, Memcached, RabbitMQ):
  - Accessible **only from Tomcat Security Group**
  - No public exposure
- SSH access via key pair for validation
- IAM:
  - IAM User for AWS CLI access
  - IAM Role attached to Tomcat instances for S3 access

---

## 🧪 Instance Initialization

Each EC2 instance is bootstrapped using **User Data scripts**:
- Tomcat installation and setup
- Database initialization and data restoration
- Cache and message broker configuration

Scripts are stored in:  
`user-data/`

---

## 🚀 Application Deployment Flow

1. Build Java application locally using Maven
2. Upload generated artifact to S3 via AWS CLI
3. Tomcat EC2 instances pull the artifact from S3
4. Application is deployed automatically on instance launch
5. ALB routes HTTPS traffic to healthy Tomcat instances

Detailed steps are documented in:  
`deployment-flow/`

---

## 📂 Repository Structure

This repository is organized to clearly separate concerns:

- `architecture/` → Diagrams and design explanation  
- `infrastructure/` → AWS components configuration  
- `user-data/` → EC2 bootstrap scripts  
- `application/` → Build & deployment process  
- `security/` → IAM, key pairs, best practices  
- `deployment-flow/` → Step-by-step deployment guide  
- `screenshots/` → Proof of configuration and running services  

---

## 🎯 Purpose of This Project

- Demonstrate real-world AWS architecture skills
- Practice IaaS-based deployments
- Build a strong DevOps / Cloud portfolio project
- Prepare for technical interviews and system design discussions

---

## 📌 Notes

- Infrastructure is deployed in the **default VPC**
- No Infrastructure-as-Code tools were used intentionally
- Focus is on understanding AWS core services and interactions
