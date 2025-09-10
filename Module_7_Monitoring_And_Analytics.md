# 📘 Module 7: Monitoring and Analytics in AWS

Monitoring and analytics are essential in AWS because they help you **track your resources, analyze logs, identify issues, and optimize costs**. In this module, we’ll cover three key services:

* **Amazon CloudWatch** → Real-time monitoring
* **AWS CloudTrail** → Auditing and governance
* **AWS Trusted Advisor** → Best-practice recommendations

---

## 📑 Table of Contents

* [🔹 Topic A: Amazon CloudWatch](#-topic-a-amazon-cloudwatch)
* [🔹 Topic B: AWS CloudTrail](#-topic-b-aws-cloudtrail)
* [🔹 Topic C: AWS Trusted Advisor](#-topic-c-aws-trusted-advisor)
* [📊 Comparison Table](#-comparison-table)
* [📝 Summary & Key Points for Quick Revision](#-summary--key-points-for-quick-revision)

---

## 🔹 Topic A: Amazon CloudWatch

### 1. What is CloudWatch?

Amazon CloudWatch is a **monitoring and observability service** for AWS resources and applications. It collects:

* **Metrics** (e.g., CPU utilization, memory usage)
* **Logs** (application/system logs)
* **Events** (changes in environment)
* **Alarms** (alerts when something goes wrong)

Think of CloudWatch as **AWS’s monitoring dashboard** where you can **see what’s happening in real-time**.

---

### 2. Key Features of CloudWatch

1. **Metrics** → Collects and stores resource performance data.

    * Example: CPU utilization of an EC2 instance.
2. **Logs** → Collects and monitors log files from applications and AWS services.

    * Example: Error logs from Lambda or Apache server logs.
3. **Alarms** → Trigger actions when thresholds are crossed.

    * Example: Send an alert if CPU usage > 80%.
4. **Dashboards** → Visualize metrics in graphs and charts.

    * Example: A custom dashboard showing EC2 + RDS + S3 usage.
5. **Events (Amazon EventBridge)** → React to changes in AWS environment.

    * Example: Automatically trigger a Lambda function if an EC2 instance stops.

---

### 3. Example Use Cases of CloudWatch

* **EC2 Instance Monitoring**: Get alerts if CPU usage is too high.
* **Application Logs Monitoring**: Analyze logs to debug errors.
* **Auto Scaling**: Scale up/down instances automatically based on demand.
* **Billing Alerts**: Get notified when AWS monthly cost exceeds a limit.

---

### 4. Real-World Example

Imagine you run an **E-commerce Website** hosted on EC2 + RDS.

* CloudWatch tracks CPU usage, memory, and database queries.
* If traffic spikes during a sale → CPU hits 90%.
* CloudWatch triggers an **Alarm** → Auto Scaling adds 2 new EC2 instances.
* Customers continue shopping smoothly 🚀.

---

## 🔹 Topic B: AWS CloudTrail

### 1. What is CloudTrail?

AWS CloudTrail is a **logging and auditing service**.
It records **all API calls and actions** taken in your AWS account.

Think of it as a **security camera** for your AWS environment.

---

### 2. Key Features of CloudTrail

1. **Event History** → See who did what, when, and from where.

    * Example: “User John stopped an EC2 instance at 10:30 AM.”
2. **Multi-Region Trails** → Track API calls across all regions.
3. **Integration with CloudWatch** → Monitor suspicious activities.
4. **Data Events** → Track actions at the object level (e.g., S3 object uploads/deletes).

---

### 3. Example Use Cases of CloudTrail

* **Security Auditing** → Check if unauthorized users accessed resources.
* **Compliance** → Maintain logs for regulations (HIPAA, PCI-DSS, GDPR).
* **Troubleshooting** → Find who deleted an S3 bucket by mistake.
* **Monitoring Data Access** → Track file uploads/downloads in S3.

---

### 4. Real-World Example

You discover an **S3 bucket is deleted**.

* Without CloudTrail → You don’t know who deleted it.
* With CloudTrail → You check logs and see:

  ```
  User: IAM-User-Dev  
  Action: DeleteBucket  
  Time: 2025-09-10 10:32:45 UTC  
  ```

✅ Now you know **who did it, when, and how**.

---

## 🔹 Topic C: AWS Trusted Advisor

### 1. What is Trusted Advisor?

AWS Trusted Advisor is like a **cloud consultant**.
It analyzes your AWS account and gives **best-practice recommendations** to improve:

* **Cost Optimization**
* **Performance**
* **Security**
* **Fault Tolerance**
* **Service Limits**

---

### 2. Key Features of Trusted Advisor

1. **Cost Optimization** → Find unused EC2 instances or underutilized resources.
2. **Performance** → Suggests better configurations for high performance.
3. **Security** → Alerts on weak security practices (e.g., open ports, IAM policies).
4. **Fault Tolerance** → Ensures backup and redundancy.
5. **Service Limits** → Warns when you are nearing AWS limits (e.g., max EC2 per region).

---

### 3. Example Use Cases of Trusted Advisor

* **Reduce Costs** → Identifies unused Elastic IPs or idle EC2.
* **Improve Security** → Alerts if your S3 bucket is public.
* **Ensure Reliability** → Checks if Auto Scaling is configured properly.

---

### 4. Real-World Example

Your company receives a **Trusted Advisor alert**:

* "You have 5 EC2 instances running at <5% CPU for 30 days."
* Action: Stop or downsize them → Save **\$300/month**.
  💡 This makes cloud usage **cost-effective and efficient**.

---

## 📊 Comparison Table

| Feature            | CloudWatch (Monitoring)   | CloudTrail (Auditing)    | Trusted Advisor (Optimization)   |
| ------------------ | ------------------------- | ------------------------ | -------------------------------- |
| **Purpose**        | Monitor metrics & logs    | Record API activity      | Give best-practice advice        |
| **Focus**          | Performance & health      | Security & compliance    | Cost, security, performance      |
| **Data Collected** | Metrics, logs, events     | API calls & user actions | Account-level recommendations    |
| **Example**        | CPU > 80% → Trigger alarm | Who deleted S3 bucket?   | Reduce EC2 costs, fix open ports |
| **Audience**       | DevOps, SysAdmins         | Security Teams           | Managers, Architects             |

---

## 📝 Summary & Key Points for Quick Revision

* **Amazon CloudWatch** → Real-time monitoring of AWS resources. Use for metrics, logs, alarms, and dashboards.
* **AWS CloudTrail** → Records every API call. Use for auditing, compliance, and troubleshooting.
* **AWS Trusted Advisor** → AWS consultant that suggests improvements for cost, security, performance, and fault tolerance.

👉 **Quick Mnemonic**:

* **CloudWatch** = *What’s happening now?*
* **CloudTrail** = *Who did what in the past?*
* **Trusted Advisor** = *How can I improve for the future?*
