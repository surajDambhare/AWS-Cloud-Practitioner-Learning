# Module 9 – Migration and Innovation

## Table of Contents

* [Topic A: AWS Cloud Adoption Framework (AWS CAF)](#topic-a-aws-cloud-adoption-framework-aws-caf)
* [Topic B: Migration Strategies](#topic-b-migration-strategies)
* [Topic C: AWS Snowball Edge](#topic-c-aws-snowball-edge)
* [Topic D: Innovation with AWS](#topic-d-innovation-with-aws)
* [Topic E: AWS Well-Architected Framework](#topic-e-aws-well-architected-framework)
* [Summary and Key Points](#summary-and-key-points)

---

## Topic A: AWS Cloud Adoption Framework (AWS CAF)

### What is it?

The **AWS Cloud Adoption Framework (CAF)** helps organizations move to the cloud in a structured way.
Think of it as a **roadmap** that guides businesses on **how to prepare, migrate, and operate in the cloud**.

It breaks down cloud adoption into **6 Perspectives**:

1. **Business Perspective**

    * Aligns IT with business outcomes.
    * Example: A retail company wants to reduce costs and improve customer experience.

2. **People Perspective**

    * Focus on training employees, change management, and new skills.
    * Example: Upskilling developers to use AWS Lambda.

3. **Governance Perspective**

    * Manages risks, compliance, and financial controls.
    * Example: Ensuring GDPR compliance while storing customer data in AWS.

4. **Platform Perspective**

    * Focus on designing the cloud infrastructure (network, compute, storage).
    * Example: Moving on-prem servers to Amazon EC2 and S3.

5. **Security Perspective**

    * Ensures data protection, identity, and compliance.
    * Example: Using IAM roles, AWS KMS for encryption.

6. **Operations Perspective**

    * Day-to-day cloud operations and monitoring.
    * Example: Using CloudWatch to monitor application health.

👉 In short: AWS CAF = **Strategy + People + Technology + Security** for smooth migration.

---

## Topic B: Migration Strategies

When moving workloads to AWS, businesses use the **7 R’s migration strategies**:

1. **Rehost (Lift and Shift)**

    * Move applications **as-is** from on-prem to AWS.
    * Example: Moving a web app from local servers to EC2 without changes.

2. **Replatform (Lift, Tinker, and Shift)**

    * Make slight optimizations without changing the core architecture.
    * Example: Moving a database to Amazon RDS instead of running it on EC2.

3. **Repurchase**

    * Replace old system with a SaaS solution.
    * Example: Moving from self-hosted CRM to Salesforce (SaaS).

4. **Refactor / Re-architect**

    * Rebuild the application for the cloud.
    * Example: Breaking a monolith app into microservices using AWS Lambda and API Gateway.

5. **Retire**

    * Shut down applications that are no longer needed.
    * Example: An old HR portal replaced by a new system.

6. **Retain (Revisit)**

    * Keep some workloads on-premises temporarily.
    * Example: Keeping sensitive legacy systems until ready for migration.

7. **Relocate** (newer strategy)

    * Move entire data centers or workloads into AWS **without redesign**.
    * Example: Using VMware Cloud on AWS to move VMs.

👉 Think of it as choosing **how much effort and transformation** you want during migration.

---

## Topic C: AWS Snowball Edge

### What is it?

AWS Snowball Edge is a **physical device** provided by AWS for **large-scale data transfer**.

* When moving **huge datasets (TBs or PBs)** to AWS, the internet is too slow.
* AWS ships a **Snowball Edge device** to your office → you load your data → ship it back → AWS uploads it to S3.

### Types of Snowball Edge:

1. **Snowball Edge Storage Optimized**

    * Large storage (80 TB usable).
    * Best for: Data migration, backup.

2. **Snowball Edge Compute Optimized**

    * Storage + Extra compute power.
    * Best for: Running local workloads, machine learning at remote sites (like oil rigs, military bases).

### Real-World Example:

* A film studio wants to move **100 TB of raw video footage** to AWS. Uploading would take weeks.
* Instead, they use **Snowball Edge** → copy data locally → ship it back → AWS uploads directly to S3.

👉 Key Point: Snowball Edge = **fast, secure, offline data migration device**.

---

## Topic D: Innovation with AWS

AWS enables businesses to **innovate faster** by providing services without heavy upfront investment.

### How AWS supports innovation:

1. **Pay-as-you-go model**

    * No huge investment, experiment at low cost.

2. **Serverless Computing**

    * Example: AWS Lambda lets you run code without servers.

3. **AI and Machine Learning**

    * Example: Amazon Rekognition for image/video analysis.
    * Example: Amazon SageMaker for building ML models.

4. **IoT and Edge Computing**

    * Example: AWS IoT Core for smart devices.

5. **Rapid Prototyping**

    * Startups can quickly test an app idea with services like Amplify and DynamoDB.

### Example:

* Netflix uses AWS to **recommend movies** (Machine Learning), **stream content globally** (CloudFront, S3), and **innovate quickly** without managing servers.

👉 AWS Innovation = **Experiment → Fail Fast → Scale Quickly**.

---

## Topic E: AWS Well-Architected Framework

The **AWS Well-Architected Framework** provides **best practices** to design secure, efficient, and reliable cloud systems.

It has **6 pillars**:

1. **Operational Excellence**

    * Continuous improvement, monitoring, automation.
    * Example: Automating deployments with AWS CodePipeline.

2. **Security**

    * Protect data and systems.
    * Example: Encrypting data with AWS KMS.

3. **Reliability**

    * Build systems that recover from failures.
    * Example: Using Multi-AZ RDS for failover.

4. **Performance Efficiency**

    * Use resources efficiently.
    * Example: Auto Scaling groups to adjust compute power.

5. **Cost Optimization**

    * Avoid unnecessary costs.
    * Example: Using Spot Instances for non-critical workloads.

6. **Sustainability** (new pillar)

    * Reduce environmental impact.
    * Example: Using serverless (Lambda) instead of always-on EC2.

👉 It acts like a **checklist** to make sure your cloud apps are **secure, reliable, and cost-effective**.

---

## Summary and Key Points

* **AWS CAF** → A roadmap for cloud adoption (Business, People, Governance, Platform, Security, Operations).
* **Migration Strategies (7 Rs)** → Rehost, Replatform, Repurchase, Refactor, Retire, Retain, Relocate.
* **AWS Snowball Edge** → Physical device for secure large-scale offline data transfer.
* **Innovation with AWS** → Enables quick experimentation with AI, IoT, serverless, and global infrastructure.
* **Well-Architected Framework** → 6 pillars: Operational Excellence, Security, Reliability, Performance, Cost, Sustainability.

📌 **Revision Tip:**

* CAF = **How to adopt cloud**
* 7 R’s = **How to migrate**
* Snowball Edge = **Move huge data**
* Innovation = **Build fast, fail fast, scale fast**
* Well-Architected = **Best practices checklist**
