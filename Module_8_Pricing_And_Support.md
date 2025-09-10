# Module 8: Pricing and Support

In this module, we will learn how AWS pricing works, explore billing tools, understand consolidated billing, use pricing calculators, learn about AWS support plans, and explore AWS Marketplace. This is a very important part of AWS because cost optimization and support are critical for both beginners and enterprises.

---

## 📌 Topic A: AWS Pricing

### 1. Basics of AWS Pricing

AWS follows a **pay-as-you-go** model. This means:

* You pay only for what you use.
* No upfront costs.
* No termination fees.
* Stop using a service → Stop paying.

Think of it like your **electricity bill**:

* If you switch on lights/fans → You pay for the units consumed.
* If you don’t use them → You don’t pay anything.

### 2. AWS Pricing Models

There are **three main pricing models**:

1. **On-Demand Pricing**

    * Pay only for the compute/storage resources you use.
    * No long-term commitment.
    * Example: EC2 On-Demand Instance.
    * Use Case: Best for short-term, unpredictable workloads.

2. **Reserved Instances (RIs)**

    * You commit to use AWS resources for **1 year or 3 years**.
    * In return, you get **up to 75% discount** compared to On-Demand.
    * Example: A company knows it will need a server for the next 3 years.
    * Use Case: Stable, predictable workloads.

3. **Spot Instances**

    * Use spare AWS capacity at **up to 90% discount**.
    * BUT: Instance can be terminated anytime if AWS needs the capacity back.
    * Example: Batch jobs, data analysis, background processing.
    * Use Case: Flexible workloads that can handle interruptions.

### 3. Other Pricing Factors

* **Storage Pricing** → Pay for how much data you store (e.g., S3 charges per GB).
* **Data Transfer Pricing** → Inbound is mostly free, outbound (data going out from AWS) costs money.
* **Request Pricing** → Some services charge per request (e.g., DynamoDB, S3 PUT/GET requests).

---

## 💻 Demonstration: Explore AWS Billing Tools

AWS provides billing tools in the **AWS Management Console**:

1. **AWS Billing Dashboard**

    * View current and past bills.
    * See usage breakdown by service.
    * Example: You can see how much EC2, S3, or RDS is costing you.

2. **Cost Explorer**

    * Visualize and analyze your costs.
    * Create custom reports.
    * Example: Track which service had the highest cost last month.

3. **AWS Budgets**

    * Set custom budgets for cost or usage.
    * Get alerts when usage exceeds the budget.
    * Example: Set a budget of \$50/month for S3. If usage crosses, you’ll get an alert.

---

## 📌 Topic B: Consolidated Billing

Consolidated Billing is part of **AWS Organizations**.

### How it works:

* You can link multiple AWS accounts under one **management (payer) account**.
* All linked accounts share the same billing method.
* Benefits:

    1. **One Bill** – All accounts get one single bill.
    2. **Volume Discounts** – Usage is aggregated across accounts, leading to cost savings.

### Example:

* Company ABC has 3 departments (HR, IT, Finance). Each has its own AWS account.
* Instead of 3 bills, they get **1 consolidated bill**.
* If total EC2 usage reaches a discount tier, **all accounts benefit from the discount**.

---

## 📌 Topic C: AWS Pricing Tools

AWS provides several tools to estimate and manage costs:

1. **AWS Pricing Calculator**

    * Estimate costs before using a service.
    * Example: Estimate monthly cost for running 2 EC2 t2.micro instances and 50 GB S3 storage.
    * Use Case: Planning and budgeting.

2. **AWS Cost Explorer**

    * Visualize costs and usage over time.
    * Example: See daily/monthly trends in EC2 spending.

3. **AWS Budgets**

    * Set spending/usage limits and get alerts.
    * Example: Notify when costs exceed \$100.

4. **Trusted Advisor (Cost Optimization Checks)**

    * Gives recommendations to reduce costs.
    * Example: Suggests terminating idle EC2 instances.

---

## 📌 Topic D: AWS Support Plans

AWS offers **4 types of Support Plans**:

1. **Basic Support (Free)**

    * Included with all accounts.
    * 24/7 access to **customer service** and documentation.
    * Support forums.
    * No technical support.

2. **Developer Support**

    * Starting at **\$29/month**.
    * Business hours support via email.
    * Best for experimenting or testing in AWS.

3. **Business Support**

    * Starts at **\$100/month**.
    * 24/7 phone, chat, and email support.
    * Trusted Advisor checks.
    * Best for production workloads.

4. **Enterprise Support**

    * Starts at **\$15,000/month**.
    * 24/7 support with **Technical Account Manager (TAM)**.
    * Concierge support.
    * Best for large enterprises.

### Comparison Table

| Plan       | Cost         | Best For          | Key Features               |
| ---------- | ------------ | ----------------- | -------------------------- |
| Basic      | Free         | Beginners         | Docs, forums               |
| Developer  | \$29/month   | Testing, Dev      | Email support              |
| Business   | \$100/month  | Production        | 24/7 phone/chat, TA checks |
| Enterprise | \$15k+/month | Large Enterprises | TAM, Concierge support     |

---

## 📌 Topic E: AWS Marketplace

AWS Marketplace is like an **online store for software** that runs on AWS.

### Key Features:

* Buy or subscribe to software (security tools, databases, monitoring apps, etc.).
* Pay through your AWS bill.
* Options include **free trials, pay-as-you-go, or annual subscriptions**.

### Example:

* A company wants to use **Splunk** for monitoring.
* Instead of manual installation, they can quickly subscribe from AWS Marketplace.
* Cost will appear in the AWS bill.

### Benefits:

* Easy to find and deploy third-party software.
* Integration with AWS billing.
* Secure and pre-configured.

---

## 📌 Summary & Key Points for Revision

* **AWS Pricing Models**: On-Demand (flexible), Reserved (long-term, cheaper), Spot (cheap but interruptible).
* **Billing Tools**: Billing Dashboard, Cost Explorer, Budgets.
* **Consolidated Billing**: One bill for multiple accounts + volume discounts.
* **AWS Pricing Tools**: Pricing Calculator, Cost Explorer, Budgets, Trusted Advisor.
* **Support Plans**: Basic (free), Developer (\$29), Business (\$100), Enterprise (\$15k).
* **AWS Marketplace**: Online store for third-party software on AWS.

- Always monitor your costs using **Cost Explorer** and **Budgets**.
- For enterprises, use **Consolidated Billing** to save money.
- Choose the right **Support Plan** depending on your needs.
-  Use **Marketplace** for quick, secure third-party software deployment.

---

