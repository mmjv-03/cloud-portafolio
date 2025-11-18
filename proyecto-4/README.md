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
* **Operational Automation**
* **Runbook Creation and Management**
* **Continuous Compliance Monitoring**
* **Security Automation and Orchestration**
* **Auditable Remediation**

## 📌 High-Level Architecture

![Nombre descriptivo](https://github.com/mmjv-03/cloud-portafolio/blob/main/proyecto-4/Diagram.png?raw=true)

## 🛠 AWS Services Overview

| Service | Purpose |
| :--- | :--- |
| **AWS Config** | Continuously evaluates AWS resource configurations against the defined security compliance rule (e.g., `restricted-ssh`). |
| **SSM Automation** | Executes the remediation workflow defined in the **Automation Document** (runbook). Provides detailed execution logs and status. |
| **Amazon SNS** | Provides immediate push notification alerts to the security team following a successful remediation action. |
| **IAM** | Defines the service role for the SSM Automation document, adhering to the Principle of Least Privilege (e.g., permissions only for `ec2:RevokeSecurityGroupIngress`). |

## 📂 Repository Structure

- cloud-portafolio
- project-4
- EADME.md
- iagram.png

---

## 🚀 Deployment and Validation Steps

The process is designed to mimic a real-world security incident response cycle using a highly auditable, managed AWS solution.

### 1. Select the AWS Managed Automation Document

* **Document Used:** `AWSConfigRemediation-RemoveUnrestrictedSourceIngressRules`
* **Rationale:** This managed document is pre-built by AWS to specifically handle the removal of overly permissive ingress rules, promoting operational efficiency and reliability.
* **Repository Content Note:** Since a managed document is used, the repository does **not** contain a custom YAML runbook.

### 2. Configure AWS Config and the Automated Remediation Action

* **Configure the AWS Config Rule:** Set up the managed rule (e.g., `restricted-ssh` or `vpc-security-group-open-to-all`) to monitor EC2 Security Groups for the `0.0.0.0/0` source rule.
* **Define the Remediation Action:**
    * **Target Service:** Select **SSM Automation**.
    * **Automation Document:** Select the AWS managed document: `AWSConfigRemediation-RemoveUnrestrictedSourceIngressRules`.
    * **Automation Assume Role (IAM Role):** Specify the ARN of the custom, least-privilege IAM role created for this execution.
    * **Parameter Mapping:**
        * `AutomationAssumeRole` is set to the ARN of the execution role.
        * `SecurityGroupId` is dynamically mapped to the **`Resource ID`** parameter provided by AWS Config.
    * **Remediation Type:** Configure the action as **Automático** to ensure immediate enforcement without manual intervention.

### 3. Test and Validate the Automation (The Security Incident)

* **Test Scenario:** Intentionally create a Security Group with an insecure rule (e.g., **TCP Port 22** access from **`0.0.0.0/0`**).
* **Validation:**
    * **Compliance Check:** AWS Config detects the violation and marks the SG as **NON-COMPLIANT**.
    * **Automated Execution:** Within minutes, the **SSM Automation** is triggered automatically (due to the "Automatic" setting).
    * **Verification:** Check the **SSM Automation Execution History** for the successful execution status.
    * **Final State:** The insecure rule must be removed from the Security Group, and Config updates the resource status back to **COMPLIANT**.

 ---

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
