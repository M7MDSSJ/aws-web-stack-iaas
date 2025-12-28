# Architecture Overview

This project implements a **multi-tier AWS web architecture** using the **IaaS model**, designed for security, scalability, and clear service separation. All resources are deployed in the **default VPC**.

---

## Architecture Design

### Entry Point
- Users access the application via a **custom domain** registered on Namecheap
- DNS points to an **internet-facing Application Load Balancer (ALB)**
- All external traffic uses **HTTPS (443)** secured by **ACM**

---

### Load Balancing
- ALB listens on **443 (HTTPS)**
- Traffic is forwarded to a target group on **8080 (HTTP)**
- ALB is the **only public-facing component**

---

### Application Layer (Tomcat)
- Runs on **EC2 (Ubuntu, t2.micro)**
- **Apache Tomcat 10 + Java 17**
- Managed by an **Auto Scaling Group**
  - Min: 1 | Desired: 2 | Max: 4
- Instances are created using a **Launch Template**
- Instances are not directly accessible from the internet

---

### Backend Services
Each backend service runs on its own **Amazon Linux 2023 EC2 instance**:

| Service | Port | Purpose |
|------|------|--------|
| MySQL (MariaDB) | 3306 | Persistent data storage |
| Memcached | 11211 | Application caching |
| RabbitMQ | 5672 | Message broker |

- Backend services share a **private security group**
- Accessible **only from Tomcat instances**
- No public exposure

---

### Internal Service Discovery
- **Route 53 Private Hosted Zone** is used
- Backend services are referenced by **private DNS names**
- DNS names are injected into the application via environment variables
- Avoids hardcoded IP addresses

---

### Artifact Deployment
- Application is built locally using **Maven**
- Artifact is uploaded to **Amazon S3**
- Tomcat instances pull the artifact from S3 using an **IAM Role**
- Application is deployed automatically during instance initialization

---

### Security Model
- Strict **Security Group** rules between layers
- **IAM User** for AWS CLI access
- **IAM Role** for EC2–S3 access
- **SSH key pair** for controlled instance access
- Principle of least privilege applied

---

### Scalability & Reliability
- ALB distributes traffic across healthy instances
- ASG replaces unhealthy Tomcat instances automatically
- Architecture can be extended to Multi-AZ and IaC easily

---

### Diagram
Visual representation of the architecture is available at:

`architecture/architecture-diagram.png`
