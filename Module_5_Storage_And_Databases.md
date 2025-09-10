# Module 5 — Storage and Database (AWS)

**Audience:** Beginners new to AWS who want clear, step-by-step notes and real-world examples.

**Format:** Simple language, examples, quick-start CLI snippets, comparisons, and best-practices. Save this file as `MODULE-5-STORAGE-AND-DATABASE.md` in your GitHub repo.

---

## Table of Contents

1. Topic A — AWS Storage

    * 1.1 Overview
    * 1.2 Storage types explained (Object / Block / File / Archive / Hybrid)
    * 1.3 Core AWS storage services (S3, EBS, EFS, FSx, Glacier)
    * 1.4 Hybrid & migration tools (Storage Gateway, DataSync, AWS Backup)
    * 1.5 When to use which storage — comparison table
    * 1.6 Examples / Real-world scenarios
    * 1.7 Quickstart: common commands
    * 1.8 Best practices & cost/security tips

2. Topic B — AWS Databases

    * 2.1 Overview
    * 2.2 Database categories (Relational, NoSQL, Data Warehouse, In-memory)
    * 2.3 Core AWS database services (RDS, Aurora, DynamoDB, Redshift)
    * 2.4 Comparisons & decision guide
    * 2.5 Examples / Real-world scenarios
    * 2.6 Quickstart: common commands
    * 2.7 Best practices & backups

3. Topic C — Additional Database Services

    * 3.1 In-memory caches (ElastiCache, MemoryDB)
    * 3.2 Document & Mongo-compatible DB (DocumentDB)
    * 3.3 Graph DB (Neptune)
    * 3.4 Ledger DB (QLDB)
    * 3.5 Time-series DB (Timestream)
    * 3.6 Cassandra-compatible (Keyspaces)
    * 3.7 Search & analytics (OpenSearch Service)
    * 3.8 Use cases and quickstarts

4. Summary & Key Revision Points

5. Appendix: Quick CLI cheatsheet and sample architectures

---

# 1. Topic A — AWS Storage

## 1.1 Overview (Simple)

Storage in AWS is about where and how you keep data. Different applications need different types of storage depending on latency, throughput, access patterns, and cost. AWS offers specialized services for each pattern so you can pick the right tool instead of forcing one tool to do everything.

Key idea: **Choose storage by *how* you access data** (frequently vs rarely, small files vs large files, block device vs file share, streaming vs archive).

## 1.2 Storage types (plain language)

* **Object storage** — stores whole files (called objects) with metadata. Great for backups, logs, images, videos. (Amazon S3)
* **Block storage** — raw disks attached to servers (like a laptop SSD). Great for databases and OS disks. (Amazon EBS)
* **File storage** — shared folders accessible by many servers using NFS/SMB. Great for content repositories, shared home directories. (Amazon EFS, Amazon FSx)
* **Archive (cold) storage** — very cheap, for data you rarely read but must keep for compliance (logs, backups). Retrieval may take time. (S3 Glacier classes)
* **Hybrid / gateway** — local interfaces that forward data to cloud storage for gradual migration or backup. (Storage Gateway)

## 1.3 Core AWS storage services — what they are & quick notes

### Amazon S3 (Simple Storage Service)

* **Type:** Object storage.
* **Good for:** Static website assets, backups, media, data lakes, logs, storing app files (images, documents).
* **Key features:** Buckets/objects, versioning, lifecycle policies, storage classes (Standard, Intelligent-Tiering, One Zone, Glacier classes), strong durability, IAM policies, encryption.
* **Real-world example:** Store user-uploaded profile pictures in an S3 bucket and serve via a CDN.

### Amazon EBS (Elastic Block Store)

* **Type:** Block storage (attaches to EC2 instances).
* **Good for:** Databases (MySQL, PostgreSQL), file systems when mounted to a single EC2, workloads needing low-latency IOPS.
* **Key features:** Different volume types (gp3, io2, st1, sc1), snapshots for backups, encryption, per-AZ volumes.
* **Real-world example:** An EC2 running PostgreSQL uses an EBS gp3 volume as its data disk.

### Amazon EFS (Elastic File System)

* **Type:** Managed NFS file system (shared file storage for Linux workloads).
* **Good for:** Shared content, home directories, CMS file storage, microservices that need shared files.
* **Key features:** Scales automatically, multi-AZ access, POSIX semantics, mountable by many EC2s.
* **Real-world example:** Multiple web servers read/write the same content directory stored on EFS.

### Amazon FSx

* **Type:** Managed high-performance file systems (Windows File Server, Lustre, NetApp ONTAP, OpenZFS).
* **Good for:** Lift-and-shift Windows applications, high-performance computing, NetApp workloads.
* **Real-world example:** Running an enterprise Windows file share without managing Windows servers.

### S3 Glacier (archive)

* **Type:** Archive storage classes inside S3 for cold data.
* **Good for:** Long-term backups, compliance archives.
* **Note:** Several retrieval options from milliseconds (Glacier Instant Retrieval) to hours (Deep Archive).

## 1.4 Hybrid & migration tools

* **AWS Storage Gateway** — run a local appliance that presents file/volume/tape interfaces and stores data in S3/Gateway volumes. Good for gradual cloud migration and backup.
* **AWS DataSync** — fast, managed data transfer tool for moving large amounts of data into AWS (EFS, S3, FSx).
* **AWS Backup** — centralized policy-driven backup across AWS services (EBS, RDS, EFS, DynamoDB, etc.). Great for compliance and scheduled backups.

## 1.5 When to use which storage — short comparison

| Need                                      |                        Use | Why                                        |
| ----------------------------------------- | -------------------------: | ------------------------------------------ |
| Store images, logs, backups, static files |                         S3 | Scalable, durable, low cost for objects    |
| OS disk or DB data                        |                        EBS | Block-level device, low latency            |
| Shared POSIX file system                  |                        EFS | Multi-instance mount, scales automatically |
| Windows native file sharing               |              FSx (Windows) | SMB + Windows features                     |
| Long-term archive                         |                 S3 Glacier | Cheapest for rarely accessed data          |
| On-prem to cloud sync                     | DataSync / Storage Gateway | Handles transfer + metadata                |

## 1.6 Examples / Real-world scenarios

* **Website assets (images, CSS, JS):** Upload to S3 → serve using Amazon CloudFront (CDN) for fast delivery worldwide.
* **Database on EC2:** EBS volume attached as `/dev/xvdf` for DB storage with daily snapshots to S3.
* **Shared media store for workflows:** Use EFS to let many build servers access the same media files.
* **Tape replacement:** Use Storage Gateway Tape Gateway to migrate backup jobs to S3/Glacier.

## 1.7 Quickstart: common commands (CLI)

**Create S3 bucket**

```bash
aws s3api create-bucket --bucket my-bucket-12345 --region us-east-1
```

**Upload a file to S3**

```bash
aws s3 cp ./photo.jpg s3://my-bucket-12345/photo.jpg
```

**Create EBS volume and attach to EC2** (example)

```bash
# create volume
aws ec2 create-volume --size 20 --availability-zone us-east-1a --volume-type gp3
# attach to instance
aws ec2 attach-volume --volume-id vol-0123456789abcdef0 --instance-id i-0123456789abcdef0 --device /dev/xvdf
```

**Create an EFS file system (quick)**

```bash
aws efs create-file-system --creation-token my-efs-token
```

> Note: These are minimal examples — always specify correct region, encryption, tags, and security groups for production.

## 1.8 Best practices & cost/security tips

* **Lifecycle policies:** Move older objects to cheaper storage classes (e.g., to Glacier) automatically.
* **Encrypt at rest & transit:** Use S3 encryption (SSE-S3/SSE-KMS) and enable TLS for in-transit.
* **Versioning & MFA delete:** Enable versioning on buckets for accidental deletion protection.
* **Right-size volumes & use appropriate EBS type:** Don’t use high-performance io2 for small dev workloads — pick cheaper gp3.
* **Backups & snapshots:** Automate snapshots (Amazon Data Lifecycle Manager or AWS Backup) and test restores.

---

# 2. Topic B — AWS Databases

## 2.1 Overview (Simple)

Databases store structured data for your applications. AWS offers managed database services so you can focus on schema and queries while AWS handles hardware, replication, backups, and scaling.

Key idea: **Pick the database that matches your access patterns and data model**: relational for transactions and complex queries, NoSQL for scale and simple access patterns, data warehouses for analytics, in-memory for low-latency data.

## 2.2 Database categories (plain)

* **Relational (SQL):** Structured tables with ACID transactions (e.g., MySQL, PostgreSQL). Use RDS/Aurora.
* **NoSQL Key-Value / Document:** Flexible schemas, massive scale and single-digit millisecond reads — DynamoDB, DocumentDB.
* **Data Warehouse:** Columnar storage for analytics (Redshift).
* **In-memory DB / Cache:** Ultra-fast data access (ElastiCache, MemoryDB).
* **Graph DB, Ledger, Time-series:** Specialized databases for relationships (Neptune), immutable audit trails (QLDB), time-series (Timestream).

## 2.3 Core AWS database services — practical notes

### Amazon RDS (Relational Database Service)

* **Type:** Managed relational DB service (hosts MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, and Amazon Aurora engine via RDS).
* **Good for:** OLTP applications requiring ACID transactions, complex joins, foreign keys.
* **Features:** Automated backups, Multi-AZ replicas for high availability, read replicas, easy scaling.
* **Real-world example:** E-commerce order processing using RDS PostgreSQL.

### Amazon Aurora

* **Type:** Cloud-native relational DB compatible with MySQL and PostgreSQL.
* **Good for:** High performance and high availability relational workloads.
* **Key benefits:** Faster than standard MySQL/PostgreSQL on many workloads, storage automatically distributed across multiple AZs.
* **Use case:** High-traffic SaaS app needing read scaling and fast failover.

### Amazon DynamoDB (NoSQL)

* **Type:** Serverless key-value and document database.
* **Good for:** Applications needing predictable, single-digit millisecond latency at any scale (mobile backends, gaming, IoT counters).
* **Key features:** Auto-scaling, global tables (multi-region), on-demand mode, transactions, DAX (in-memory cache for DynamoDB).
* **Real-world example:** Session store for a globally distributed app.

### Amazon Redshift (Data Warehouse)

* **Type:** Columnar, MPP (massively parallel processing) data warehouse.
* **Good for:** Large-scale analytics, BI reporting, complex SQL over petabytes of data.
* **Features:** Columnar storage, compression, massive parallel queries, Redshift Spectrum for querying S3 data.

## 2.4 Comparisons & decision guide (short)

* **Need low-latency reads/writes at huge scale →** DynamoDB.
* **Need relational features, joins, transactions →** RDS / Aurora.
* **Need analytics on large datasets →** Redshift.
* **Need cache for database reads →** ElastiCache or DAX.

## 2.5 Examples / Real-world scenarios

* **Banking transaction system:** RDS/Aurora (strong consistency & ACID).
* **Social media likes and counters:** DynamoDB (high write throughput, simple keys).
* **Business analytics & dashboards:** Redshift (aggregate terabytes of events nightly).

## 2.6 Quickstart: common commands (CLI)

**Create a small RDS MySQL instance (example)**

```bash
aws rds create-db-instance \
  --db-instance-identifier mydb1 \
  --db-instance-class db.t3.micro \
  --engine mysql \
  --allocated-storage 20 \
  --master-username admin \
  --master-user-password your-password \
  --backup-retention-period 7
```

**Create a DynamoDB table**

```bash
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions AttributeName=UserId,AttributeType=S \
  --key-schema AttributeName=UserId,KeyType=HASH \
  --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```

> Note: For production, use IAM roles, VPCs for private access to RDS, secure passwords and parameter groups.

## 2.7 Best practices & backups

* **Use Multi-AZ for RDS** for automatic failover.
* **Use automated backups and test restores** regularly.
* **Model DynamoDB carefully** (design keys and access patterns up-front) — access patterns drive table design.
* **Use read replicas** for read-heavy relational workloads and offload reporting queries.
* **Encrypt data at rest** (KMS) and in transit.

---

# 3. Topic C — Additional Database Services

These are specialized or complementary database services for specific needs.

## 3.1 In-memory caches: ElastiCache & MemoryDB

* **ElastiCache (Redis/Memcached):** Managed in-memory cache. Use for session stores, leaderboards, counters, caching DB query results.
* **MemoryDB for Redis:** Durable, Redis-compatible in-memory primary database (durability + Redis API).
* **Example:** Cache product details in ElastiCache to avoid repeated RDS reads.

## 3.2 Document DB (Amazon DocumentDB)

* **Type:** MongoDB-compatible document database (managed).
* **Good for:** JSON-style documents, flexible schemas, apps built on MongoDB drivers.
* **Example:** Store product catalogs with varying attributes per product.

## 3.3 Graph DB (Amazon Neptune)

* **Type:** Managed graph database supporting Gremlin, openCypher, SPARQL.
* **Good for:** Social graphs, fraud detection, recommendation engines where relationships matter.
* **Example:** Social network friend-of-friend queries demonstrated quickly with Gremlin.

## 3.4 Ledger DB (Amazon QLDB)

* **Type:** Immutable, verifiable ledger. Centralized ledger with cryptographic verification of changes.
* **Good for:** Applications needing an auditable history of changes (finance, supply chain).
* **Example:** Track vehicle ownership transfers with tamper-evident history.

## 3.5 Time-series DB (Amazon Timestream)

* **Type:** Purpose-built time-series database.
* **Good for:** IoT telemetry, metrics, operational monitoring — data with time as a primary dimension.
* **Example:** Store and query temperature sensor readings over time.

## 3.6 Cassandra-compatible (Amazon Keyspaces)

* **Type:** Managed Apache Cassandra-compatible service.
* **Good for:** Wide-column workloads that already use Cassandra or need Cassandra APIs without operational overhead.
* **Example:** Large write-heavy datasets with time-to-live (TTL) per row.

## 3.7 Search & analytics (Amazon OpenSearch Service)

* **Type:** Managed Elasticsearch/OpenSearch.
* **Good for:** Full-text search, log analytics, dashboards (Kibana/OpenSearch Dashboards).
* **Example:** Search product descriptions on an e-commerce site or analyze application logs.

## 3.8 Use cases & quickstarts

**ElastiCache (Redis) quick create (CLI)**

```bash
aws elasticache create-cache-cluster --cache-cluster-id my-redis --engine redis --cache-node-type cache.t3.micro --num-cache-nodes 1
```

**Create a Neptune cluster (simplified)**

```bash
aws neptune create-db-cluster --db-cluster-identifier my-neptune-cluster --engine neptune
```

> Many of these services require VPC, subnet groups, security groups, and proper IAM roles. Use the AWS Console or AWS CDK/Terraform for production deployments.

---

# 4. Summary & Key Points (Quick Revision)

* **Storage**: pick object (S3) for static files and data lakes; block (EBS) for OS/DB disks; file (EFS/FSx) for multi-instance shared file access; Glacier for archives.
* **Databases**: pick RDS/Aurora for relational ACID needs; DynamoDB for serverless NoSQL at scale; Redshift for analytics.
* **Specialized**: use ElastiCache or MemoryDB for caching, Neptune for graph, QLDB for ledger needs, Timestream for time-series.
* **Security & durability**: enable encryption (KMS), backups (AWS Backup), and proper IAM + VPC isolation.
* **Cost**: architect lifecycle policies, right-size instances/volumes, and leverage serverless options (DynamoDB on-demand, S3 Intelligent-Tiering) for unpredictable workloads.

---

# 5. Appendix: CLI Cheatsheet & Sample Architectures

**Common CLI commands quicklist**

* S3: `aws s3 cp`, `aws s3 ls`, `aws s3api create-bucket`
* EBS: `aws ec2 create-volume`, `aws ec2 attach-volume`, `aws ec2 create-snapshot`
* EFS: `aws efs create-file-system`
* RDS: `aws rds create-db-instance`
* DynamoDB: `aws dynamodb create-table`
* Redshift: `aws redshift create-cluster`

**Sample architecture patterns**

1. **Web app (static + dynamic):** S3 for static front-end files + CloudFront CDN, RDS/Aurora for app DB, ElastiCache for caching, S3 for backups.
2. **Big data pipeline:** Ingest events → store raw data in S3 (data lake) → ETL with AWS Glue → load aggregated data into Redshift for BI.
3. **IoT telemetry:** Edge devices → ingest into Kinesis or IoT Core → store time-series in Timestream → visualize.

---

If you want, I can:

* Convert this to a ready-to-commit `MODULE-5-NOTES.md` file with a clean header and Git commit message template.
* Create shorter flashcards or an Anki deck for revision.

*End of Module 5 — Storage & Database*
