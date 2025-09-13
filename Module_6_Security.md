# 📘 Module 6: AWS Security

Security is one of the **most critical pillars** of AWS Cloud. AWS provides a **shared responsibility model**, identity and access control, account management, compliance support, application security, and additional security services to protect workloads.

This module explains all aspects of AWS Security in **simple terms**, with **hands-on examples, comparisons, and best practices**. The file is structured to be directly commit-ready for your GitHub repository.

---

## 📑 Table of Contents

* [Introduction](#introduction)
* [🔐 Topic A: Shared Responsibility Model](#-topic-a-shared-responsibility-model)

    * [What it is](#what-it-is)
    * [Responsibilities breakdown (detailed)](#responsibilities-breakdown-detailed)
    * [Examples and real-world use cases](#examples-and-real-world-use-cases)
* [👤 Topic B: AWS Identity and Access Management (IAM)](#-topic-b-aws-identity-and-access-management-iam)

    * [Key concepts](#key-concepts)
    * [Types of policies and where to use them](#types-of-policies-and-where-to-use-them)
    * [Sample policies and trust policy examples](#sample-policies-and-trust-policy-examples)
    * [Step-by-step: common IAM tasks (CLI + Console)](#step-by-step-common-iam-tasks-cli--console)
  
* [🏢 Topic C: AWS Organizations](#-topic-c-aws-organizations)

    * [Core concepts](#core-concepts)
    * [Service Control Policies (SCPs) explained](#service-control-policies-scps-explained)
    * \[Account design patterns and use-cases]
    * \[Step-by-step: organize accounts and apply SCPs]
* [✅ Topic D: Compliance](#-topic-d-compliance)

    * \[Compliance programs overview]
    * \[Customer responsibilities for compliance]
    * \[Tools to assist compliance]
    * \[Practical example: HIPAA/PCI/GDPR considerations]
* [🛡️ Topic E: Application Security](#-topic-e-application-security)

    * \[Key services and how they work together]
    * \[Examples: WAF + CloudFront, Shield, Inspector]
    * \[Secrets management & secure coding basics]
    * \[Application security checklist]
* [🧰 Topic F: Additional Security Services](#-topic-f-additional-security-services)

    * \[GuardDuty, CloudTrail, Macie, Config, Security Hub, KMS]
    * \[Common workflows & incident response example]
* [💻 Hands-on Labs (practical exercises)](#-hands-on-labs-practical-exercises)

    * \[Lab 1: Secure IAM user and enable MFA]
    * \[Lab 2: Create role for EC2 to access S3]
    * \[Lab 3: Enable GuardDuty & respond to a finding]
    * \[Lab 4: Configure WAF basic rules on CloudFront]
    * \[Lab 5: Store and rotate a secret in Secrets Manager]
* [🔁 Summary & Key Points for Revision](#-summary--key-points-for-revision)
* [✅ Quick Revision Checklist](#-quick-revision-checklist)
* [📚 Suggested Next Steps / Further Learning](#-suggested-next-steps--further-learning)

---

## Introduction

This module covers AWS security from fundamentals to practical, real-world patterns. Each topic includes simple explanations, examples, step-by-step commands where useful, and recommended best practices for production-ready environments. Use this as a guide while studying, building, or documenting AWS security architecture for your projects.

---

## 🔐 Topic A: Shared Responsibility Model

### What it is

AWS and the customer **share the responsibility** of security in the cloud. AWS is responsible for protecting the underlying cloud infrastructure (the "security of the cloud"). The customer is responsible for securing their data and resources that run on the cloud (the "security in the cloud").

### Responsibilities breakdown (detailed)

Below is a more granular breakdown to help you understand who is responsible for what:

| Layer             | AWS (Security *of* the cloud)                                           | Customer (Security *in* the cloud)                                              |
| ----------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Physical          | Data center facilities, physical access control, hardware lifecycle     | N/A                                                                             |
| Network (infra)   | Physical network, hypervisor networking, DDoS mitigation at infra layer | VPC configuration, Security Groups, NACLs, route tables                         |
| Compute           | Hypervisor, host OS, virtualization security                            | Guest OS patching, instance configuration, container image hardening            |
| Storage           | Storage hardware, base-level availability                               | Data classification, encryption (customer-managed keys), S3 bucket policies     |
| Identity & Access | Underlying identity service availability and security                   | IAM users/roles/policies, MFA, credential rotation                              |
| Application       | Infrastructure that supports managed services (e.g., RDS control plane) | App code security, input validation, dependency management                      |
| Configuration     | Secure-by-default at infra level where feasible                         | Secure configuration of services (e.g., RDS encryption, S3 block public access) |

### Examples and real-world use cases

* **Example 1 — EC2 Instance:**

    * AWS: ensures the hypervisor and physical host are secure and patched.
    * You: must patch the OS, disable unnecessary services, configure firewalls, and harden SSH access.

* **Example 2 — Amazon S3:**

    * AWS: provides durable storage, availability, and infrastructure security.
    * You: must set the correct S3 bucket policies, enable encryption at rest (SSE), configure access logging, and avoid public exposure unless intentional.

* **Example 3 — Managed Database (RDS):**

    * AWS: manages the underlying host, storage reliability, and network availability.
    * You: configure database-level authentication, enforce SSL/TLS, manage users, and backup/restore strategy.

### Checklist & best practices

* Always **assume responsibility** for data and configuration.
* Use **encryption at rest** and **in transit** for sensitive data.
* Implement **least privilege** on IAM policies.
* Keep systems **patched and monitored**.
* Enable logging (CloudTrail, VPC Flow Logs, S3 access logs) and send logs to a central location.

---

## 👤 Topic B: AWS Identity and Access Management (IAM)

IAM is the core service to manage authentication and authorization in AWS.

### Key concepts

* **Principal**: an entity that can take action (IAM user, role, service, or federated user).
* **Identity**: users, groups, roles.
* **Policy**: a JSON document that grants or denies permissions.
* **Resource**: AWS entities like S3 buckets, EC2 instances, Lambda functions.
* **Trust Policy**: defines who can assume a role.

### Identity types

* **IAM User**: Long-lived identity for an individual; has permanent credentials.
* **IAM Group**: Collection of users; attach policies to groups for easier management.
* **IAM Role**: Temporary credentials for services or federated users. Use roles for EC2, Lambda, cross-account access.
* **Federated Identity**: Temporary access via SAML, OIDC (e.g., using corporate AD or Google Workspace).

### Types of policies and where to use them

* **Identity-based policies**: Attached to users, groups, or roles.
* **Resource-based policies**: Attached directly to resources (e.g., S3 bucket policies, SQS policies).
* **Permission boundaries**: Maximum permissions an identity can grant itself.
* **Session policies**: Passed when using `AssumeRole` to restrict permissions for that session.
* **Service Control Policies (SCPs)**: Applied at Organization/OU level to set account-wide guardrails.

### Policy evaluation basics (short)

When an API call is made, AWS evaluates all relevant policies. The algorithm roughly follows:

1. By default, all requests are **denied**.
2. An explicit **Allow** overrides default deny.
3. An explicit **Deny** always overrides Allow.

### Common JSON policy examples

**1) S3 read-only policy (identity-based)**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": ["arn:aws:s3:::example-bucket", "arn:aws:s3:::example-bucket/*"]
    }
  ]
}
```

**2) Trust policy example for EC2 role (allow EC2 to assume role)**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {"Service": "ec2.amazonaws.com"},
      "Action": "sts:AssumeRole"
    }
  ]
}
```

**3) Cross-account role trust policy (allow account 111122223333 to assume role)**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::111122223333:root"},
      "Action": "sts:AssumeRole"
    }
  ]
}
```

### Step-by-step: common IAM tasks (CLI + Console)

**Create an IAM user (CLI)**

```bash
aws iam create-user --user-name dev-user
aws iam create-login-profile --user-name dev-user --password 'P@ssw0rd!' --password-reset-required
aws iam attach-user-policy --user-name dev-user --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

**Create an IAM role for EC2 (CLI)**

1. Create a trust policy file `trust.json` (example above for `ec2.amazonaws.com`).
2. Create the role:

```bash
aws iam create-role --role-name EC2S3AccessRole --assume-role-policy-document file://trust.json
```

3. Attach a policy allowing S3 access:

```bash
aws iam attach-role-policy --role-name EC2S3AccessRole --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

**Assume role (using STS)**

```bash
aws sts assume-role --role-arn arn:aws:iam::123456789012:role/EC2S3AccessRole --role-session-name demoSession
```

> Note: For the console, prefer guided wizards – but always remember to attach least-privilege policies.

### Best practices & common pitfalls

* **Do not use root account** for everyday tasks. Configure MFA on the root account and create admin users.
* **Use roles for services** (EC2, Lambda) instead of storing keys in the instance.
* **Enforce least privilege** and refine policies gradually (start with read-only, add write as needed).
* **Rotate credentials and keys** periodically.
* **Monitor**: enable CloudTrail and send logs to a secure account.
* **Avoid wildcard (`*`) permissions** except for very specific admin roles.

---

## 🏢 Topic C: AWS Organizations

AWS Organizations helps you manage multiple AWS accounts under a single organization for governance, automation, and consolidated billing.

### Core concepts

* **Organization**: The root container for accounts.
* **Organizational Unit (OU)**: Logical grouping of accounts (e.g., `Prod`, `Dev`, `Security`).
* **Root**: The top-level parent of your organization.
* **Service Control Policies (SCPs)**: Policies that set the maximum available permissions for accounts in an OU.
* **Consolidated Billing**: Centralized billing across member accounts.

### Service Control Policies (SCPs) explained

SCPs are applied to the account or OU and limit the maximum permissions for all IAM identities in those accounts. SCPs do **not** grant permissions; they only restrict what can be done.

**Example SCP – Deny EC2 in an OU**

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Effect":"Deny",
      "Action":["ec2:RunInstances","ec2:TerminateInstances","ec2:StartInstances"],
      "Resource":"*"
    }
  ]
}
```

### Account design patterns and use-cases

* **Single account**: Simple but riskier for large orgs (resource blast radius is large).
* **Multiple accounts (recommended for production)**: Isolate workloads and teams (e.g., `Security`, `Networking`, `Prod`, `Dev`).
* **Account per environment**: Each environment (dev/test/prod) gets its own account to isolate faults and manage costs.

### Step-by-step: organize accounts and apply SCPs (high level)

1. Create Organization from the master account.
2. Create OUs for `Security`, `SharedServices`, `Prod`, `Dev`.
3. Create member accounts or invite existing accounts.
4. Create and attach SCPs to OUs (for example, prevent public S3 buckets, or disable certain services).
5. Use tagging and consolidated billing to track costs.

### Best practices

* Enforce strong guardrails in the `Security` OU (enable CloudTrail, Config recorder to central logging account).
* Use SCPs to **deny** dangerous APIs rather than granting selectively.
* Use a separate account for security tooling and logging.

---

## ✅ Topic D: Compliance

Compliance is the practice of meeting regulatory and industry standards for data protection and privacy.

### Compliance programs overview

AWS maintains many compliance certifications (examples):

* **ISO 27001**
* **SOC 1/2/3**
* **PCI DSS** (payment card industry)
* **HIPAA** (healthcare)
* **GDPR** (EU data protection)
* **FedRAMP** (US government cloud security)

> AWS provides the infrastructure and compliance artifacts; customers must implement controls to remain compliant.

### Customer responsibilities for compliance

* Classify data and identify which workloads are subject to regulations.
* Configure services securely (e.g., encryption, access controls).
* Keep audit logs (CloudTrail) and retain for required periods.
* Demonstrate appropriate data handling and consent for GDPR.

### Tools to assist compliance

* **AWS Artifact**: Access compliance reports (audit artifacts).
* **AWS Config**: Continuous assessment of resource configurations against rules.
* **CloudTrail**: Auditing API calls and user activity.
* **AWS Security Hub**: Provides compliance checks against standards (CIS AWS Foundations, PCI DSS, etc.).

### Practical example: HIPAA/PCI/GDPR considerations

* **HIPAA**: Use encryption at rest/in transit; sign Business Associate Agreement (BAA) with AWS if storing PHI; enforce strict access controls and logging.
* **PCI DSS**: Use dedicated accounts, encryption, logging, and strong access control for cardholder data environments.
* **GDPR**: Implement data minimization, encryption, data subject rights processes (deletion, access), and determine data residency if required.

---

## 🛡️ Topic E: Application Security

Application security focuses on protecting your applications from attacks and preventing data leaks.

### Key services and how they work together

* **AWS Shield**: DDoS protection.

    * *Standard*: free and automatic at the AWS edge.
    * *Advanced*: paid service with additional protections and response support.
* **AWS WAF (Web Application Firewall)**: Configure rules to block malicious web requests (SQLi, XSS, bot traffic).
* **Amazon Inspector**: Automated security assessment service for EC2 and container images.
* **AWS Secrets Manager & Systems Manager Parameter Store**: Manage and rotate secrets.
* **AWS Certificate Manager (ACM)**: Provision, manage TLS/SSL certificates.

### Examples: WAF + CloudFront + Shield

* Place CloudFront (CDN) in front of your web application.
* Enable AWS WAF on CloudFront to filter and block malicious web traffic using rules.
* Shield Standard protects from common layer 3/4 DDoS attacks automatically; enable Shield Advanced for sensitive apps that require DDoS cost protection and extra mitigation.

### Amazon Inspector (v2) and vulnerability scanning

* Inspector runs network reachability checks and vulnerability scans for packages.
* Use findings to prioritize patching and remediation.

### Secrets management & secure coding basics

* Never store secrets in source code or environment variables in plaintext.
* Use **Secrets Manager** or **Parameter Store** with KMS encryption.
* Rotate credentials regularly and implement fine-grained IAM to limit who can retrieve secrets.

**Example: retrieve secret via CLI**

```bash
aws secretsmanager get-secret-value --secret-id MyDatabaseSecret
```

### Application security checklist

* Use HTTPS everywhere (ACM for certificates).
* Validate user input and sanitize outputs to prevent injection.
* Use WAF to protect against common web exploits.
* Store secrets securely and rotate them.
* Automate security scanning of code and images (Inspector, SCA tools).

---

## 🧰 Topic F: Additional Security Services

### Amazon GuardDuty

* Continuous threat detection using machine learning, anomaly detection, and threat intel.
* Detects suspicious logins, unusual API calls, and potential data exfiltration.

### AWS CloudTrail

* Records AWS API calls and events.
* Enable multi-region CloudTrail and send logs to a central logging account/S3 bucket.
* Enable log file validation for integrity checks.

### Amazon Macie

* Data classification and discovery service for S3.
* Finds sensitive data (PII, credentials) and provides alerting.

### AWS Config

* Records resource configuration and enables rules to detect noncompliant resources.
* Useful for automated remediation and compliance evidence.

### AWS Security Hub

* Aggregates findings from GuardDuty, Inspector, Macie, and other tools.
* Provides a unified dashboard and standard checks (e.g., CIS AWS Foundations).

### AWS KMS (Key Management Service)

* Create and manage encryption keys (CMKs).
* Integration with many services (S3, EBS, RDS, Lambda, Secrets Manager).
* Support for customer-managed keys, automatic rotation, IAM policies, and grants.

### Common workflows & incident response example

1. **Detection**: GuardDuty finds anomalous API calls from an unusual IP.
2. **Investigation**: CloudTrail shows the API calls and the IAM principal involved; VPC Flow Logs show network activity.
3. **Containment**: Use IAM to revoke suspicious credentials or apply a deny SCP.
4. **Remediation**: Rotate access keys, patch instances found vulnerable by Inspector.
5. **Post-incident**: Use Config and CloudTrail logs to create an incident report and tighten guardrails.

---

## 💻 Hands-on Labs (practical exercises)

These short labs will increase familiarity with AWS security features.

### Lab 1: Secure IAM user and enable MFA

**Goal:** Create an IAM user, enable console access, attach least-privilege policy, and enable MFA.

**Steps (console):**

1. Sign in to AWS as an admin (not root).
2. Go to IAM → Users → Create user. Provide console access and require password reset.
3. Create a new group `DevReadOnly` and attach `ReadOnlyAccess` policy.
4. Add the user to the group.
5. Configure MFA for the user (Virtual MFA device - Authenticator app).

**Verification:** Try logging in as the user and confirm MFA prompt.

### Lab 2: Create role for EC2 to access S3

**Goal:** Launch EC2 and allow it to read from an S3 bucket without keys.

**Steps:**

1. Create an IAM role with a trust policy for `ec2.amazonaws.com`.
2. Attach a policy allowing `s3:GetObject` on `arn:aws:s3:::example-bucket/*`.
3. Launch an EC2 instance; in the "IAM role" step choose the created role.
4. SSH into EC2 and run `aws s3 ls s3://example-bucket` (install AWS CLI if needed).

**Verification:** EC2 can list/download objects without any access keys.

### Lab 3: Enable GuardDuty & respond to a finding

**Goal:** Enable GuardDuty and inspect findings.

**Steps:**

1. Enable GuardDuty in your account (from the console).
2. Wait for initial findings (or simulate via known test patterns if available).
3. Review findings and investigate with CloudTrail.
4. Take action: disable compromised keys, update security groups, or block IPs via WAF/Network ACL.

### Lab 4: Configure WAF basic rules on CloudFront

**Goal:** Block a pattern (e.g., SQL injection) at the CDN level.

**Steps:**

1. Create a CloudFront distribution for your site.
2. Go to AWS WAF → Web ACL → Create web ACL and associate with CloudFront.
3. Add a managed rule group (AWS managed rules) and a custom rule to block suspicious query strings.
4. Test by sending requests that would be blocked and verify 403 responses.

### Lab 5: Store and rotate a secret in Secrets Manager

**Goal:** Save a DB password and enable automatic rotation.

**Steps:**

1. Go to Secrets Manager → Store a new secret (e.g., credentials for RDS).
2. Configure rotation using a Lambda template provided by Secrets Manager.
3. Update application to fetch secret at start or via environment (do not hardcode).
4. Test rotation and confirm application can retrieve new secret.

---

## 🔁 Summary & Key Points for Revision

* **Shared Responsibility**: AWS is responsible for the cloud; you are responsible for what you put in the cloud.
* **IAM**: Use roles for services, enforce least privilege, enable MFA, rotate credentials.
* **Organizations & SCPs**: Use multiple accounts and SCPs to enforce guardrails at scale.
* **Compliance**: Use AWS tools and processes to collect evidence; implement customer-side controls.
* **Application Security**: Use WAF, Shield, Inspector, Secrets Manager, and secure coding practices.
* **Additional Services**: GuardDuty, CloudTrail, Macie, Config, Security Hub, and KMS form the core monitoring and protection stack.

---

