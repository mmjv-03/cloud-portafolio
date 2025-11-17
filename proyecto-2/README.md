# 🛡️ Project 2 — Private EC2 Architecture Using VPC, NAT Gateway & AWS Systems Manager (SSM)

This project demonstrates a secure, production-oriented AWS architecture where EC2 instances operate strictly in private subnets with no public exposure, and all administrative access is performed through AWS Systems Manager Session Manager (no SSH required).

It follows cloud security best practices for segmentation, controlled egress, auditability, and zero-trust operational access.

---

## 🧰 Technologies Used

| Service | Purpose |
|--------|---------|
| Amazon VPC | Network isolation & segmentation |
| Private & Public Subnets | Layered architecture (private workloads, public networking) |
| Internet Gateway (IGW) | Outbound connectivity for public subnets |
| NAT Gateway | Secure outbound internet access for private EC2 |
| EC2 (Private Only) | Backend compute layer with no public IP |
| AWS Systems Manager (SSM) | Secure access without SSH keys |
| IAM | Role-based access control & least privilege |
| CloudWatch Logs | Centralized logging, SSM session audit |
| Security Groups & NACLs | Traffic boundary enforcement |

---

## 🎯 Project Objective

Deploy a secure, controlled and auditable compute environment that demonstrates:

- Private EC2 instances with zero public exposure  
- NAT-based egress instead of public IPs  
- Administrative access via SSM Session Manager  
- Least-privilege IAM roles for EC2 and operators  
- Fully segmented VPC with public & private tiers  
- Production-ready logging and security controls  

This project showcases skills in:

- AWS network architecture  
- Zero-trust access design  
- IAM & SSM session management  
- Cloud security best practices  
- Infrastructure segmentation and isolation  
- Documentation & cloud governance  

---

## 📌 High-Level Architecture

(Including VPC, Private Subnets, NAT, SSM)

**Architecture Diagram**  
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/Diagram-VPC.png?raw=true)


---

## 📂 Repository Structure

cloud-portafolio/
- project-2/
- README.md
- diagram.png
- images/ # Screenshots for documentation

---


---

## 🚀 Deployment Steps

### 1. Create the VPC
- CIDR example: **10.0.0.0/16**
- Enables isolated network and subnet mapping.  

---

### 2. Create Public and Private Subnets
Recommended:
- 2 public subnets (different AZs)
- 2 private subnets (different AZs)

- Public: for NAT gateway  
- Private: for EC2 instances  

---

### 3. Internet Gateway (IGW)
Attach the IGW to the VPC to allow outbound internet from public subnets.  

---

### 4. Create NAT Gateway
- Placed in a public subnet.  
- Enables EC2 instances in private subnets to access the internet securely.

Example use cases:

- OS updates  
- Downloading dependencies  
- API calls  


---

### 5. Configure Route Tables

- **Public Route Table** → `0.0.0.0/0` → **Internet Gateway**  
- **Private Route Table** → `0.0.0.0/0` → **NAT Gateway**  

Ensures private workloads never get inbound public traffic.  

- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/Step1-2-3-4-5-VPC.png?raw=true)

---

### 6. Create IAM Role for EC2 (Instance Profile)

Must allow:

- SSM connectivity (`ssm:*`, `ssmmessages:*`, `ec2messages:*`)
- CloudWatch logging
- Optional S3 read/write for agent logs

Attach the role to the EC2 instance.  
- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/step6-Role-ec2.png?raw=true)

---

### 7. Launch EC2 Instance in Private Subnet

Important settings:

- Disable public IP  
- Attach IAM instance role  
- Attach EC2 Security Group  
- SSM Agent-enabled AMI (Amazon Linux 2, Ubuntu 20+, etc.)  

- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/Step7-Launch-EC2-Instance-in-Private-Subnet.png?raw=true
)

---

### 8. Connect Using AWS Systems Manager

No SSH key pairs, no port 22, no public exposure.

- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/Step8.1-Connect-Using-AWS-Systems-Manager.png?raw=true)
- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/step8.2-Connect-Using-AWS-Systems-Manager.png?raw=true)

### 9. Configure CloudWatch Logging

**Enable:**

- SSM session logs  
- System logs from EC2 (optional CloudWatch agent)  
- Audit tracking through CloudTrail  

- ![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-2/screenshots/Step9-Configure-CloudWatch-Logging.png?raw=true)

---

## 🧠 Skills Demonstrated

- ✔ AWS Architecture  
- ✔ Secure VPC Design  
- ✔ Zero-Trust Access (SSM over SSH)  
- ✔ IAM Least-Privilege Role Creation  
- ✔ Segmentation with Public/Private Subnets  
- ✔ NAT Gateway Egress Security  
- ✔ VPC Route Table Configuration  
- ✔ Centralized Logging & Observability  
- ✔ Professional IT Documentation  

---

## 🔐 Recommended Security Enhancements

To achieve enterprise-grade security baselines, consider implementing:

### 1. VPC Endpoints (Interface Endpoints)
Create PrivateLink endpoints for:
- SSM
- EC2 Messages
- CloudWatch Logs
- S3

**Benefits:**
- Eliminates NAT costs for AWS API calls  
- Traffic stays inside AWS backbone  
- Reduced attack surface

---

### 2. Restrictive Security Groups
**Examples:**
- Inbound: only from same SG or specific app SGs  
- Outbound: restrict to HTTPS (443)  
- No `0.0.0.0/0` inbound rules  
- No SSH port (22) exposed anywhere

---

### 3. Network ACL Hardening
- Deny public ephemeral ports  
- Allow only necessary ranges  
- Additional subnet-level filtering

---

### 4. Session Manager Logging & MFA Enforcement
- Send all session logs to CloudWatch or S3  
- Log every command  
- Require MFA for starting SSM sessions

---

### 5. CloudTrail + GuardDuty
**Provides:**
- API activity auditing  
- Anomaly detection  
- Malicious behavior alerts

---

### 6. Config Rules
Detects:
- Public subnets without IGW  
- Overly permissive SGs  
- IAM misconfigurations  
- Missing encryption  
- Unattached IAM roles

---

### 7. Disable IMDSv1
Force EC2 to use **IMDSv2** to prevent SSRF-style metadata attacks.

---

### 8. Add a Bastion-Alternative Layer (Optional)
Use:
- SSM port forwarding  
- SSM tunneling  
- SSM Run Command  

Instead of maintaining bastion hosts.

---

### 9. Encryption Everywhere
- EBS volumes with KMS  
- CloudWatch Logs with KMS  
- S3 logs with KMS  

Improves compliance and forensic readiness.

---

## 📸 Evidence (Suggested Screenshots)

Include in `/images/` folder:

- VPC configuration  
- Subnets list  
- Route tables (public/private)  
- NAT Gateway  
- IAM instance role  
- EC2 instance with no public IP  
- SSM Start Session screenshot  
- CloudWatch logs showing SSM events

---

## 🌎 Result

A fully secured, private compute environment demonstrating production-grade AWS architecture and cloud security best practices.
