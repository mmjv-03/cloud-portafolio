# 🌐 Project 3 — Highly Available Multi-AZ Web Application with ALB, Auto Scaling, RDS, and AWS WAF

This project showcases a production-grade, secure, and highly available web application architecture on AWS.

It follows industry standards for fault tolerance, security, scalability, and operational excellence.

## 💻 Technologies Used

* **Amazon EC2** — Application compute layer
* **Auto Scaling Group (ASG)** — Elastic scaling
* **Application Load Balancer (ALB)** — Layer-7 load balancing
* **Amazon RDS (MySQL/PostgreSQL)** — Multi-AZ managed relational database
* **Amazon VPC** — Network segmentation across public/private subnets
* **AWS WAF** — Web Application Firewall for L7 security
* **AWS Systems Manager (SSM)** — Instance access + secure management
* **IAM** — Least-privilege access control
* **CloudWatch** — Monitoring, logs, and alarms

## 🎯 Project Objective

Deploy a secure, scalable, Multi-AZ web application using an Auto Scaling Group behind an Application Load Balancer, with a private RDS database and WAF protection.

### This project demonstrates skills in:

* High-availability AWS architecture
* Network segmentation with public/private subnets
* Load balancing + autoscaling
* Database security best practices
* AWS WAF for application-layer protection
* IAM least-privilege
* Infrastructure hardening & monitoring
* Professional AWS documentation

## 📌 High-Level Architecture

### Architecture Diagram

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/Diagram.png?raw=true)

## 🛠 AWS Services Overview

| Service | Purpose |
| :--- | :--- |
| **Amazon EC2** | Compute instances used for hosting the web application |
| **Auto Scaling Group** | Automatically scales instances based on traffic/load |
| **Application Load Balancer (ALB)** | Distributes traffic across EC2 instances |
| **Amazon RDS** | Multi-AZ managed database with backups & failover |
| **AWS WAF** | Protects ALB against OWASP attacks |
| **Amazon VPC** | Segmentation of public/private subnets |
| **IAM** | Secure roles & least-privilege policies |
| **AWS SSM** | Secure instance access without SSH keys |
| **CloudWatch** | Logs, monitoring, alarms |

## 📂 Repository Structure

cloud-portfolio/
- project-3/
- README.md
- diagram.png
- user-data.sh

---

## 🚀 Deployment Steps

### 1. Create a VPC with Public & Private Subnets

Recommended layout:

* 2 Public Subnets → ALB + NAT Gateways
* 2 Private Subnets → EC2 instances + RDS Multi-AZ
* 1 Internet Gateway
* 2 Route Tables (public & private)

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/step1-CreateVPC.png?raw=true)

### 2. Deploy the Application Load Balancer (ALB)

* Internet-facing
* Listener: HTTPS : 443
* TLS certificate from ACM
* Target group: EC2 instances (port 80)
* Health checks: `/`

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step2.0-ALB.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step2.1-Listeners.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step2.2-TargetGroup.png?raw=true)

### 3. Configure Auto Scaling Group (ASG)

Key settings:

* Minimum: 2 instances
* Maximum: 4–6 instances
* Desired capacity: 2
* Launch template with user-data script
* Instances deployed only in private subnets
* Attach ALB target group

User-data example in user-data.sh:

- #!/bin/bash
- sudo yum update -y
- sudo yum install -y httpd
- echo Project 3 — HA Web App > /var/www/html/index.html
- sudo systemctl start httpd
- sudo systemctl enable httpd

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step3-ASG.png?raw=true
)


### 4. Create the RDS Database (Multi-AZ)

Configuration:

* Engine: MySQL or PostgreSQL
* **Multi-AZ enabled**
* Instance in **private subnets only**
* **No public access**
* Security group allows inbound only from **EC2 SG**
* Automated backups and snapshots enabled

Database credentials stored in:
* AWS Secrets Manager **or**
* SSM Parameter Store

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step4-RDS.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step4.1-SecretRDS.png?raw=true)

### 5. Attach AWS WAF to the ALB

Enable security protection rules:

* AWS Managed Rules
* SQL Injection
* XSS
* Known Bad Actors
* Anonymous IP
* Bot Control (optional)

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step5-WAF.png?raw=true)

### 6. Configure SSM for Secure Access (No SSH Keys)

Enable:

* SSM Agent
* Session Manager
* IAM role allowing SSM access

Avoid SSH + key pairs entirely. This ensures zero external attack surface on EC2.

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step6-SSM.png?raw=true)

### 7. Monitoring & Logging

Configure:

* CloudWatch metrics (CPU, network, ALB metrics)
* ASG scaling policies
* CloudWatch alarms (5XX spikes, instance failures)
* ALB access logs → S3
* WAF logs → CloudWatch or S3


### 8. Test the Deployment

Verify:

* Load balancer returns application
* Autoscaling increases/decreases EC2s
* RDS is multi-AZ & reachable from EC2
* WAF is blocking malicious requests
* EC2 instances are **NOT** publicly accessible
* Only ALB has public exposure
* SSM Session Manager working correctly

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-3/screenshots/Step8-test.png?raw=true)

## 🌎 Public Load Balancer URL

www.cookieprueba.link

---

## 🧠 Skills Demonstrated

* ✔ High-Availability AWS Architecture
* ✔ Auto Scaling + Load Balancing
* ✔ Network Segmentation (Public/Private Subnets)
* ✔ RDS Multi-AZ Deployment
* ✔ AWS WAF & Application Security
* ✔ SSM Zero-Trust Instance Access
* ✔ IAM Least-Privilege Design
* ✔ Logging & Monitoring (CloudWatch)
* ✔ Secure Infrastructure Deployment
* ✔ Professional Cloud Documentation

---

## 🔐 Recommended Security Enhancements

These improvements strengthen security and align with AWS Well-Architected best practices:

### 1. RDS Encryption with KMS CMK

Enforce:
* At-rest encryption
* Key rotation
* Access control via KMS policies
* CloudTrail auditing of decrypt/encrypt APIs

### 2. ALB Access Logs + Centralized Logging

Store logs in S3 for:
* Forensics
* Traffic analysis
* Incident response

### 3. WAF Custom Rules

Add:
* Rate-limiting
* Geoblocking
* Custom regex rules
* Block known malicious IP classes

### 4. Security Group Hardened Rules

* ALB → allows inbound 443 only
* EC2 → ALB only
* RDS → EC2 only
* Strict egress rules

### 5. SSM Session Logging

Enable:
* Session recording
* Session logs to S3 or CloudWatch
* Ensures traceability of admin activity.

### 6. IAM Access Analyzer

Detects:
* Overly permissive policies
* Cross-account access
* Misconfigurations

### 7. AWS Config Rules

Rules for detecting:
* Public subnets exposure
* Security groups that allow `0.0.0.0/0`
* Unencrypted RDS
* Unencrypted EBS volumes

### 8. ALB TLS Policy Enforcement

Recommended:
* TLS 1.2 or TLS 1.3
* Remove weak ciphers

---

