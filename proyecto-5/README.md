# 🛡️ Project 5 — Automated Security Incident Triage and Containment

This final project demonstrates the implementation of a **Serverless Security Operations Center (SOC)** workflow. It focuses on the automated detection, aggregation, and rapid response to high-severity security findings, specifically focusing on containing compromised EC2 instances.

This architecture is the gold standard for **Security Automation and Orchestration (SAO)**, leveraging the power of event-driven computing to achieve near real-time incident response.

Technologies used:
* **AWS Security Hub CSPM** — Centralized security posture and findings aggregation (ASFF format).
* **Amazon GuardDuty** — Intelligent threat detection (e.g., unauthorized access, suspicious API calls).
* **Amazon EventBridge** — Serverless event bus for real-time routing of critical security findings.
* **AWS Lambda** — Executes the automated containment logic (triage).
* **IAM** — Enforces the Principle of Least Privilege for all components.

## 🎯 Project Objective

To build an automated, auditable response mechanism that contains a highly critical security threat detected by GuardDuty, ensuring the immediate isolation of the affected resource.

This project showcases skills in:
* **Security Automation and Orchestration (SAO)**
* **Real-Time Incident Response (IR)**
* **Centralized Security Posture Management (CSPM)**
* **Event-Driven Architecture (EDA)**
* **AWS Security Specialty best practices**

## 📌 High-Level Architecture

![Diagrama de Arquitectura de Respuesta a Incidentes](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/Diagram.png?raw=true)

## 🛠 AWS Services Overview

| Service | Purpose |
| :--- | :--- |
| **GuardDuty** | Provides continuous monitoring for malicious activity and unauthorized behavior. |
| **Security Hub** | Aggregates, normalizes, and prioritizes findings from GuardDuty and other security services. |
| **EventBridge** | Filters the stream of Security Hub events and routes critical findings to the Lambda function for response. |
| **AWS Lambda** | Executes the automated code to perform the **containment action** (`ec2:ModifyInstanceAttribute` or `ec2:ReplaceSecurityGroup`). |
| **IAM** | Defines granular permissions for the Lambda function, allowing it only to query and modify EC2 Security Groups. |

## 📂 Repository Structure

- cloud-portafolio
- project-5
- README.md
- diagram.png

## 🚀 Deployment and Validation Steps

### 1. Enable Security Services

* Enable **AWS Security Hub** and **Amazon GuardDuty** in the target AWS region.
* Enable the **AWS Foundational Security Best Practices** standard within Security Hub.
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step1-GuardDuty.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step1-SH.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step1-standards.png?raw=true)

### 2. Prepare Resources

* **Create a "Quarantine" Security Group:** A Security Group without inbound and outbound rules. This will be used to isolate the compromised instance.
* **Create the Triage Lambda Function:** Write the Python code (`triage_handler.py`) to:
    * Parse the JSON event from EventBridge (the ASFF finding).
    * Extract the affected EC2 Instance ID.
    * Use Boto3 to attach the **Quarantine SG** to the instance, isolating it.
* **Create the Lambda IAM Role:** Attach a custom, minimum-privilege policy allowing only `ec2:DescribeInstances`, `ec2:ModifyInstanceAttribute`, and logging permissions.
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/step2-LambdaCode.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step2.1-lambdapolicy.png?raw=true)

### 3. Configure EventBridge Routing

* Create a new **EventBridge Rule** (Source: `aws.securityhub`).
* **Define the Event Pattern** to filter for high-severity findings from GuardDuty:
    ```json
    {
      "source": ["aws.securityhub"],
      "detail-type": ["Security Hub Findings - Imported"],
      "detail": {
        "findings": {
          "Severity": {
            "Label": ["CRITICAL", "HIGH"]
          },
          "ProductFields": {
            "aws/securityhub/ProductName": ["GuardDuty"]
          }
        }
      }
    }
    ```
* **Define the Target:** Set the target as the **Triage Lambda Function**.
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step3-eventbridgerule.png?raw=true)

### 4. Test and Validate the Automated Response

* **Simulation:** Use a GuardDuty testing tool (like the AWS-provided **GuardDuty Finding Generator**) to simulate a high-severity finding (e.g., a "Runtime Monitoring" threat) that targets an EC2 instance.
* **Validation:**
    * Verify that the critical finding appears in **Security Hub**.
    * Verify that the **EventBridge Rule** successfully triggered the Lambda.
    * Check the **CloudWatch Logs** of the Lambda to ensure the containment action executed successfully.
    * Confirm that the **EC2 instance's Security Group** has been switched to the **Quarantine SG**.
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step4-sampleindings.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step4.1-Findings.png?raw=true)
![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-5/screenshots/Step4-invocations.png?raw=true)

## 🧠 Skills Demonstrated

* **Incident Response & Containment:** Building automated workflows for immediate threat mitigation.
* **Security Event Orchestration:** Mastery in routing and transforming security data using EventBridge.
* **API Integration (Boto3):** Using Python to programmatically control AWS resources.
* **Compliance & Aggregation:** Utilizing Security Hub to manage security posture and normalize data (ASFF).
* **IAM Security Design:** Configuring complex, least-privilege roles for cross-service interaction.

## 📬 Contact

- LinkedIn: https://www.linkedin.com/in/victor-matos-188769242?utm_source=share_via&utm_content=profile&utm_medium=member_ios 
- GitHub: Victor Matos
