# 🛡️ Project 4 — Automated Security Remediation and Compliance with AWS Config and SSM Automation

This project showcases a key capability in operational excellence and security: **Automated Incident Response**. It demonstrates the implementation of a fully automated, event-driven mechanism to detect security non-compliance (specifically, an overly permissive Security Group rule) and execute immediate, corrective remediation actions using **AWS Systems Manager (SSM)**.

Leveraging SSM Automation Documents provides robust logging, built-in error handling, and superior integration with operational tooling, making this solution highly relevant for Cloud Security roles.

Technologies used:
* **AWS Config** — Continuous resource compliance monitoring.
* **AWS Systems Manager (SSM) Automation** — Executes the pre-defined remediation runbook.
* **Amazon SNS** — Notification service for security alerts.
* **IAM** — Enforcing the Principle of Least Privilege for the Automation Role.

## 🎯 Project Objective

To establish a proactive and auditable security enforcement system that automatically identifies and resolves common security misconfigurations using a managed operational runbook.

This project showcases skills in:
* **Operational Automation (SysOps)**
* **Runbook Creation and Management**
* **Continuous Compliance Monitoring**
* **Security Automation and Orchestration (SAO)**
* **Auditable Remediation**

## 📌 High-Level Architecture



> *Self-explanatory visual showing the flow:*
> 1.  **Non-Compliant Resource:** A new Security Group (SG) is created with SSH access open to the world (`0.0.0.0/0`).
> 2.  **AWS Config Trigger:** AWS Config detects the violation against the defined rule (`restricted-ssh`).
> 3.  **Remediation Action:** Config triggers a pre-defined **SSM Automation Document**.
> 4.  **Automated Fix:** The SSM runbook uses the `aws:executeScript` or native actions to modify the SG, removing the insecure rule.
> 5.  **Notification:** SSM Automation or Config sends an alert via Amazon SNS, notifying the security team of the automated, auditable remediation.

## 🛠 AWS Services Overview

| Service | Purpose |
| :--- | :--- |
| **AWS Config** | Continuously evaluates AWS resource configurations against the defined security compliance rule (e.g., `restricted-ssh`). |
| **SSM Automation** | Executes the remediation workflow defined in the **Automation Document** (runbook). Provides detailed execution logs and status. |
| **Amazon SNS** | Provides immediate push notification alerts to the security team following a successful remediation action. |
| **IAM** | Defines the service role for the SSM Automation document, adhering to the Principle of Least Privilege (e.g., permissions only for `ec2:RevokeSecurityGroupIngress`). |

## 📂 Repository Structure

cloud-portafolio/ └── project-4/ ├── README.md ├── diagram.png └── ssm/ └── Revoke_Insecure_SSH_Access.yaml # SSM Automation Document (Runbook)

## 🚀 Deployment and Validation Steps

The use of an SSM Automation Document makes the remediation process highly auditable and visible within the AWS Console.

### 1. Define the SSM Automation Document (The Runbook)

* Create an Automation Document (e.g., **`Revoke_Insecure_SSH_Access.yaml`**).
* The document uses steps like `aws:executeScript` or a specific API action to:
    * Accept the Security Group ID as an input parameter from AWS Config.
    * Call the `ec2:RevokeSecurityGroupIngress` action via the `aws:executeAwsApi` step to remove the rule open to `0.0.0.0/0`.
    * Include a notification step using SNS.

### 2. Configure AWS Config and the Remediation Action

* Set up the **AWS Config Rule** (e.g., `restricted-ssh`).
* Define the **Remediation Action** in Config, specifying:
    * The target service: **SSM Automation**.
    * The **Automation Document** to execute.
    * The **AutomationAssumeRole** (IAM role) with the necessary permissions.
    * The **Parameter Mapping** to pass the non-compliant `resourceId` (the Security Group ID) to the SSM document.

### 3. Test and Validate the Automation

* **Test Scenario:** Intentionally create a Security Group with an insecure rule (e.g., **TCP Port 22** access from **`0.0.0.0/0`**).
* **Validation:**
    * AWS Config detects the violation and triggers the SSM Automation.
    * Verify the execution status in the **SSM Automation Dashboard**—it should show **Success**.
    * Check the Security Group in the EC2 Console; the insecure rule must be removed.
    * Confirm the **SNS notification** was received, documenting the automated fix and the execution ID.

## 🧠 Skills Demonstrated

* **AWS SysOps & Automation:** Mastery in using managed services like SSM for operational tasks, demonstrating repeatable and auditable workflows.
* **Security Automation:** Building event-driven, closed-loop security response systems.
* **Configuration Management:** Using AWS Config to enforce and maintain a desired state for resource configurations.
* **Auditing and Traceability:** Utilizing SSM's robust logging and execution history for post-incident review.
* **Incident Response:** Implementing a rapid, automated first-response capability to security alerts.

## 🔐 Recommended Security Enhancements

1.  **Runbook Versioning and Testing:** Use version control for the Automation Document and deploy changes first to a staging environment before promoting.
2.  **Detailed Logging:** Configure the SSM Automation to log verbose output to CloudWatch Logs for forensic analysis.
3.  **Role Separation:** Use separate, least-privilege IAM roles for the **Config service** and the **SSM Automation execution**.
4.  **Integration with Ticketing:** Extend the Automation Document to automatically open and close a ticket in an external ITSM system (e.g., Jira, ServiceNow) upon detection and remediation.
5.  **Multi-Region Deployment:** Ensure the entire stack (Config, SSM, IAM roles) is deployed in all regions where resources are launched.

## 📬 Contact

LinkedIn: [Tu URL de LinkedIn]
GitHub: [Tu Usuario de GitHub]
