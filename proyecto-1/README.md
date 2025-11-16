# 🌐 Project 1 — Static Website Hosting on AWS with S3, CloudFront, ACM, and Route 53

This project demonstrates a fully deployed, secure, and production-ready static website architecture on AWS.  
It follows industry best practices for availability, performance, and cloud security.

Technologies used:

- **Amazon S3** — Static hosting  
- **Amazon CloudFront** — Global CDN  
- **AWS Certificate Manager (ACM)** — SSL/TLS certificates  
- **Amazon Route 53** — DNS & domain management  
- **IAM + OAC** — Secure access control  
- **Versioning** — Content protection

---

## 🎯 Project Objective

Deploy a scalable, secure, and cost-effective static website using fully managed AWS services.

This project showcases skills in:

- Cloud architecture  
- AWS security  
- DNS & SSL certificate management  
- Serverless design  
- Documentation and best-practice implementation  

---

## 📌 High-Level Architecture (Including Route 53 & ACM)

![Architecture Diagram](Diagram.png)


---

## 🛠 AWS Services Overview

| Service | Purpose |
|---------|---------|
| **Amazon S3** | Static file hosting. Bucket remains private using OAC. |
| **CloudFront** | Global CDN delivering low-latency content over HTTPS. |
| **AWS Certificate Manager (ACM)** | Provisioning and managing SSL certificates. DNS validation. |
| **Route 53** | DNS management, domain routing, Alias records to CloudFront. |
| **IAM** | Secure access control via OAC and policies. |
| **S3 Versioning** | Protects against accidental deletions/overwrites. |

---

## 📂 Repository Structure

cloud-portafolio/
- project-1/
  - README.md
  - diagram.png
  - index.html


---

## 🚀 Deployment Steps

### 1. Create the S3 bucket
- Unique bucket name (can match your domain).
- Disable public access (mandatory).
- Enable versioning.
- Upload `index.html`.

---

### 2. Request SSL certificate with ACM
- **Region: us-east-1 (N. Virginia)** — CloudFront requires this.
- Add domains:
  - `yourdomain.com`
  - `www.yourdomain.com`
- Choose **DNS validation**.

---

### 3. Validate the certificate in Route 53
- ACM provides DNS CNAME records.
- Route 53 can auto-create them.
- Status will change to **Issued** when ready.

---

### 4. Configure Origin Access Control (OAC)
OAC ensures S3 bucket remains private while CloudFront can read it.

- Create OAC
- Attach OAC to CloudFront origin
- CloudFront updates bucket policy automatically

---

### 5. Create the CloudFront distribution
Recommended configuration:

- Origin: S3 bucket with OAC
- Viewer protocol: **Redirect HTTP to HTTPS**
- Custom certificate: Select ACM certificate
- Default root object: `index.html`
- Enable compression
- Leave caching defaults or configure later

---

### 6. Configure domain using Route 53
In your Hosted Zone:

- **A (Alias)** → CloudFront distribution  
- **AAAA (Alias)** → CloudFront  
- For `www`:
  - CNAME → root domain or CloudFront URL

---

### 7. Test the deployment
Verify:

- HTTPS is working  
- CloudFront is serving content  
- DNS resolves correctly  
- Certificate is valid  
- S3 bucket is not publicly accessible  

---

## 🌎 Live URL

> Add your website link here once deployed.

---

## 🧠 Skills Demonstrated

✔ AWS Architecture  
✔ Cloud Security Best Practices  
✔ SSL/TLS Certificate Management (ACM)  
✔ DNS & Domain Management (Route 53)  
✔ CDN Optimization (CloudFront)  
✔ Serverless Web Hosting  
✔ Professional Cloud Documentation  

---

## 📬 Contact

LinkedIn: *your-link*  
GitHub: *your-username*
