# Module 3 — Global Infrastructure & Reliability

---

## Topic A — AWS Global Infrastructure (what it is & why it matters)

AWS runs many physical data centers around the world and groups them into *Regions*, *Availability Zones*, *Local Zones*, *Wavelength Zones*, and *Edge locations*. This global footprint lets you place compute and storage near users, increase reliability, and reduce latency.

## Key building blocks (plain language)
- **Region** — a geographic area (for example: `us-east-1`, `ap-south-1`). A Region is independent from others so customers control where their data lives.  
  *Why care?* Choose the Region closest to your users or where compliance/data residency requires.

- **Availability Zone (AZ)** — one or more isolated data centers inside a Region (e.g., `us-east-1a`). AZs are connected with very fast, private networks. Use multiple AZs to survive a single datacenter failure.

- **Local Zone** — an extension of a Region placed in a metro area close to users (example: Los Angeles Local Zone for `us-west-2`). Good for very low-latency apps in a city without creating a new Region.

- **Wavelength Zone** — compute placed at a telecom provider’s network edge for ultra-low-latency 5G use cases (mobile gaming, real-time AR/VR). Wavelength Zones let you run AWS compute very close to mobile devices.

- **Edge locations / Points of Presence (PoPs)** — used by services like CloudFront and AWS Global Accelerator to cache content and speed up traffic. There are hundreds of edge locations worldwide to reduce latency for static/dynamic web content.

## Why AWS groups infrastructure this way
- **Reliability** — AZs let you design fault-tolerant architectures (multi-AZ).
- **Performance** — Local Zones/Wavelength and Edge PoPs bring compute/cache closer to users.
- **Compliance/sovereignty** — pick Regions that meet local regulatory/data-residency requirements.

---

# Topic B — Get closer to your customers (latency, caching, and edge services)

## The problem
Users notice delays (latency) if compute or content is far away. For interactive apps, milliseconds matter.

## Ways to *get closer* (and when to use them)
1. **Choose the right Region** — simplest step. If most users are in India, use `ap-south-1` (Mumbai) when possible. Consider service availability (not every Region supports every new service).
2. **Use Availability Zones** — place app servers across AZs to serve users in the same geographic region reliably.
3. **CloudFront (edge caching)** — cache static assets (images, JS, CSS) at edge locations worldwide so users get content from a nearby cache instead of origin servers. Great for websites and CDN needs.  
   *Example:* Put website assets in S3 + CloudFront distribution — users download assets from nearest edge PoP, lowering page load times.
4. **AWS Global Accelerator** — uses the AWS global network to route user traffic to the optimal endpoint (good for TCP/UDP apps requiring consistent low latency).
5. **Local Zones** — deploy latency-sensitive compute (e.g., media rendering) in a city where users are concentrated.
6. **Wavelength** — for 5G-based mobile workloads requiring single-digit millisecond latency (e.g., AR multiplayer).
7. **S3 Transfer Acceleration / Edge optimizations** — for faster uploads from global users to S3 using optimized network paths.

## Example scenario
- App: multiplayer mobile game with players across Mumbai and London.  
  *Solution:* Game servers in `ap-south-1` and `eu-west-1`, use Global Accelerator to direct players to nearest healthy endpoint, and use Wavelength if you target 5G mobile players in cities that support it. Cache static game assets with CloudFront.

---

# Topic C — AWS Outposts (bring AWS to your datacenter)

## 1. What is AWS Outposts?

**AWS Outposts** is a fully managed service that extends AWS infrastructure, services, APIs, and tools to your on-premises data center or co-location space.

- Run AWS compute, storage, and networking services locally.
- Seamlessly connect with the AWS cloud for hybrid workloads.

**Simple Analogy:**
- Think of it as a **mini AWS cloud in your office**, giving you AWS services locally while still connected to the global AWS network.

---

## 2. Why Use AWS Outposts?

1. **Low Latency** – Run applications that need millisecond-level response times.
2. **Data Residency & Compliance** – Keep sensitive data locally for regulatory compliance.
3. **Hybrid Cloud** – Seamlessly integrate on-premises workloads with AWS cloud services.
4. **Fully Managed** – AWS handles installation, monitoring, updates, and maintenance.

---

## 3. How AWS Outposts Works

1. **Install Outpost Rack**
    - AWS ships a fully configured rack with EC2, EBS, and networking.

2. **Connect to AWS Region**
    - Secure connection to AWS region for monitoring and management.

3. **Deploy Services Locally**
    - Run services like:
        - **EC2** → Virtual machines
        - **EBS** → Local block storage
        - **S3 on Outposts** → Object storage
        - **RDS on Outposts** → Databases

4. **Integrate with Cloud**
    - Connect seamlessly with AWS cloud services for additional functionality.

---

## 4. Real-World Example

**Scenario:** A bank must keep customer data on-premises due to regulations but wants AWS capabilities.

- **Step 1:** Install AWS Outposts in the bank’s data center.
- **Step 2:** Run EC2 servers locally for processing transactions.
- **Step 3:** Store sensitive data in EBS / S3 on Outposts.
- **Step 4:** Connect to AWS region for backups and analytics.

**Result:**
- Low latency for critical apps.
- Regulatory compliance achieved.
- Leverages AWS ecosystem for management and scalability.

---

## 5. Key Features of AWS Outposts

| Feature                       | Description |
| ------------------------------ | ----------- |
| Hybrid Cloud                   | Connect on-premises apps to AWS cloud seamlessly |
| Fully Managed                  | AWS handles hardware, updates, and monitoring |
| Consistent Services            | Same APIs, tools, and services as AWS cloud |
| Low Latency                    | Applications run close to local resources |
| Compliance-Friendly            | Data can remain on-premises for regulations |

---

## 6. Benefits in Simple Words

- **Local AWS Services:** Run EC2, RDS, EBS, and S3 locally.
- **Hybrid Integration:** Connect on-premises apps to AWS cloud easily.
- **Regulatory Compliance:** Keep sensitive data on-premises.
- **Fully Managed:** AWS handles installation, updates, and monitoring.
- **Low Latency:** Ideal for applications needing very fast responses.

---

## 7. Key Takeaways

- **AWS Outposts = AWS cloud brought to your data center.**
- Ideal for hybrid workloads requiring **low latency or local data residency**.
- Fully managed by AWS with seamless integration to AWS region services.
- Use cases: financial services, healthcare, media processing, industrial IoT.
---

# Topic D — Interact with AWS services (console, CLI, SDKs, IaC)

## Ways to interact (overview)
1. **AWS Management Console** — browser UI. Easy for exploration, monitoring, and one-off tasks.
2. **AWS CLI** — command-line tool for automation and scripting. (Install the AWS CLI v2 and configure `aws configure` with credentials or a profile.)
3. **SDKs** — e.g., **boto3** for Python, **AWS SDK for JavaScript**, etc. Use SDKs for app-level interactions.
4. **REST APIs** — every AWS service exposes APIs. SDKs wrap these APIs.
5. **Infrastructure as Code (IaC)** — CloudFormation, CDK, or Terraform to declare and automate infrastructure.
6. **Management & Observability** — CloudWatch, X-Ray, AWS Config, and AWS CloudTrail for logs, tracing, and audit trails.

## Quick CLI & SDK examples (hands-on)

### A — List Regions (CLI)
```bash
# show all Regions (even disabled)
aws ec2 describe-regions --all-regions --query "Regions[].{Name:RegionName,OptIn:OptInStatus}" --output table
```
*Use this to see Regions your account can access.*

### B — List Availability Zones, Local Zones, Wavelength Zones (CLI)
```bash
# List AZs (normal + local + wavelength info depending on account)
aws ec2 describe-availability-zones --region us-east-1 --output table

# List Local Zones (filter by zone-type)
aws ec2 describe-availability-zones --region us-west-2 \
  --filters Name=zone-type,Values=local-zone --all-availability-zones --output table

# Find Wavelength Zones enabled for your account
aws ec2 describe-availability-zones --region us-east-1 \
  --filters Name=zone-type,Values=wavelength-zone --all-availability-zones --output table
```

### C — Python (boto3) example: list regions
```python
import boto3
ec2 = boto3.client('ec2')
regions = ec2.describe_regions()['Regions']
print([r['RegionName'] for r in regions])
```

### D — Quick example: create an S3 bucket + CloudFront (conceptual steps)
1. Create an S3 bucket in a Region and upload your website files.
2. Create a CloudFront distribution that uses the S3 bucket as origin.
3. Configure cache behavior, TTLs, and (optionally) a custom domain + ACM certificate.  
   CloudFront will serve cached copies from edge locations worldwide.

---

# Demonstration: Explore the AWS Global Infrastructure (step-by-step lab)

Follow these steps locally — you can record outputs and add them to your GitHub notes.

## Prerequisites
- AWS account + admin or developer IAM user with `ec2:Describe*` permissions.
- AWS CLI v2 installed and configured (`aws configure`) or Console access.
- Optional: Python + boto3.

## Demo steps

### 1) Open the interactive global map (quick look)
- Visit **AWS Global Infrastructure** page in browser to see Regions, AZs, Local Zones and Edge locations visually. (Great for classroom demo.)

### 2) List Regions with CLI
```bash
aws ec2 describe-regions --all-regions --query "Regions[].{Name:RegionName,OptIn:OptInStatus}" --output table
```
Take a screenshot and note which Regions are `opt-in` if any.

### 3) Inspect AZs in a Region
```bash
aws ec2 describe-availability-zones --region ap-south-1 --output table
```
Note zone names like `ap-south-1a`, and that each AZ is distinct.

### 4) Find Local Zones near you
```bash
aws ec2 describe-availability-zones --region us-west-2 \
  --filters Name=zone-type,Values=local-zone --all-availability-zones --output table
```
If none appear, that Local Zone might require account opt-in; the docs show how to opt-in.

### 5) Find Wavelength Zones (if you want to demo 5G edge)
```bash
aws ec2 describe-availability-zones --region <parent-region> \
  --filters Name=zone-type,Values=wavelength-zone --all-availability-zones --output table
```
(Again, some Wavelength zones require opt-in.)

### 6) Explore Edge locations (CloudFront)
- In CloudFront console, create a test distribution with an S3 origin and request the `/.well-known/` object from different global locations (or use an online latency testing tool). This demonstrates content served from the nearest edge PoP. Read CloudFront features to understand PoP counts and architecture.

### 7) Try Outposts (concept demo)
- Outposts require procurement, so for demo, read the “How Outposts works” guide and review networking/anchor AZ concepts. You can simulate the architecture diagram and explain how the Outpost connects to the Region.

---

# Reliability & best practices (design patterns)

## Design for availability
- **Multi-AZ** for high availability (e.g., RDS Multi-AZ, EC2 across AZs). AZ failure should not bring app down.
- **Multi-Region** for disaster recovery (if an entire Region fails). Use cross-region replication (S3 CRR) and failover DNS (Route 53).
- **Health checks & routing** — use Route 53 health checks + failover policies or Global Accelerator for weighted/latency routing.

## Design for performance
- **Cache at edge** (CloudFront).
- **Move compute closer** with Local Zones or Wavelength for latency-sensitive workloads.

## Security & operations
- Use **IAM** least privilege, **CloudTrail** for audit logs, **AWS Config** for drift detection, and **GuardDuty** for threat detection.
- Monitor with **CloudWatch** (metrics, alarms) and **X-Ray** (distributed tracing).

---

# Quick reference — CLI cheatsheet (put these in your GitHub `commands.md`)
```bash
# List regions
aws ec2 describe-regions --all-regions --output table

# List AZs in a region
aws ec2 describe-availability-zones --region us-east-1 --output table

# List Local Zones in a parent region (opt-in may be needed)
aws ec2 describe-availability-zones --region us-west-2 \
  --filters Name=zone-type,Values=local-zone --all-availability-zones --output table

# Check CloudFront distributions
aws cloudfront list-distributions

# Simple boto3 to list regions
python -c "import boto3; ec2=boto3.client('ec2'); print([r['RegionName'] for r in ec2.describe_regions()['Regions']])"
```

---

# Suggested lab exercises to add to your GitHub (practical & demonstrable)
1. **Repo: `aws-global-infra-demo`**
    - `lab-01-list-regions-azs/` — scripts (bash + boto3) that list Regions and AZs and save to CSV.
    - `lab-02-cloudfront-s3/` — create an S3 static site; create CloudFront distribution; test latency from two locations.
    - `lab-03-local-zone-check/` — script to detect available Local Zones and opt-in instructions (documented).
    - `lab-04-outposts-architecture/` — markdown + diagram explaining Outposts network design and HA considerations (no need to order hardware).

2. **README content to include in repo** — summary, commands, screenshots, and a `resources.md` linking to the official docs (below).

---

# Useful official docs & resources (for your repo links)
- EC2 Regions & Zones (concepts & CLI): Amazon EC2 User Guide.
- AWS Global Infrastructure overview (interactive map): AWS About Global Infrastructure.
- AWS Outposts — what it is + how it works: Outposts User Guide.
- AWS Local Zones overview: Local Zones docs.
- AWS Wavelength overview: Wavelength docs.
- CloudFront features & PoPs: CloudFront features page.

---

# Final tips (short)
- **Start simple:** pick one Region and learn AZs → then add Local Zones / CloudFront.
- **Record every demo:** save CLI outputs and screenshots to your repo — great for interviews & portfolio.
- **Keep an eye on region availability:** services roll out region-by-region; always check the official pages before production design.
