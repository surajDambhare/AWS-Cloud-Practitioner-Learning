# Module 4: Networking

This module covers the fundamental and advanced concepts of AWS Networking, including Amazon VPC, security boundaries, and interacting with the AWS global network.  
It is designed for beginners and progresses step by step with examples for better understanding.

---

## 📑 Table of Contents
- [Topic A: Amazon Virtual Private Cloud (Amazon VPC)](#topic-a-amazon-virtual-private-cloud-amazon-vpc)
    - [What is Amazon VPC?](#what-is-amazon-vpc)
    - [Default VPC vs Custom VPC](#default-vpc-vs-custom-vpc)
    - [Key Components of VPC](#key-components-of-vpc)
    - [Example Scenario](#example-scenario)
- [Topic B: Network Access Control Lists (NACLs) and Security Groups](#topic-b-network-access-control-lists-nacls-and-security-groups)
    - [What are Security Groups?](#what-are-security-groups)
    - [What are Network ACLs?](#what-are-network-acls)
    - [Comparison: Security Groups vs NACLs](#comparison-security-groups-vs-nacls)
    - [Example Use Case](#example-use-case)
- [Topic C: Interacting with the AWS Global Network](#topic-c-interacting-with-the-aws-global-network)
    - [What is the AWS Global Network?](#what-is-the-aws-global-network)
    - [Key Services for Global Networking](#key-services-for-global-networking)
    - [Benefits of AWS Global Network](#benefits-of-aws-global-network)
    - [Example Scenario](#example-scenario-1)

---

## Topic A: Amazon Virtual Private Cloud (Amazon VPC)

### What is Amazon VPC?
- **Amazon Virtual Private Cloud (Amazon VPC)** is a logically isolated virtual network in AWS where you can launch and manage resources like EC2 instances, databases, and load balancers.
- Think of it as your own private data center within AWS, where you have full control over:
    - IP address ranges
    - Subnets
    - Route tables
    - Internet connectivity
    - Security

---

### Default VPC vs Custom VPC
- **Default VPC**:
    - Automatically created in each AWS region.
    - Includes one subnet per Availability Zone (AZ).
    - Easy for beginners to quickly launch resources without custom setup.
- **Custom VPC**:
    - You create and configure from scratch.
    - More flexibility and control (IP ranges, security, routing).
    - Best practice for production workloads.

---

### Key Components of VPC
1. **Subnets**
    - Divisions of the VPC’s IP address range.
    - Types:
        - **Public Subnet**: Connected to the internet (via Internet Gateway).
        - **Private Subnet**: Isolated from the internet, used for databases or internal services.

2. **Route Tables**
    - Define rules for where network traffic should go.
    - Example: Route public traffic (0.0.0.0/0) to the Internet Gateway.

3. **Internet Gateway (IGW)**
    - A gateway attached to your VPC that enables resources in a public subnet to access the internet.

4. **NAT Gateway**
    - Allows private subnet resources to access the internet (for updates, downloads) without exposing them to incoming traffic.

5. **VPC Peering**
    - Connects two VPCs privately across the same or different AWS accounts/regions.

---

### Example Scenario
Imagine you’re hosting a web application:
- Web servers in a **public subnet** (so users can access it over the internet).
- Databases in a **private subnet** (protected from direct internet access).
- A **NAT Gateway** allows the database servers to download security patches without being publicly accessible.

---

## Topic B: Network Access Control Lists (NACLs) and Security Groups

### What are Security Groups?
- Virtual firewalls at the **instance level**.
- **Stateful**: If you allow inbound traffic, the response outbound is automatically allowed.
- Rules are **whitelist-based**: only allow the traffic you specify.

Example:
- Allow inbound traffic on port 80 (HTTP) and port 443 (HTTPS).
- Allow SSH access (port 22) only from your office IP.

---

### What are Network ACLs?
- Virtual firewalls at the **subnet level**.
- **Stateless**: Inbound and outbound rules must be defined separately.
- Rules can **allow or deny** traffic.

Example:
- Allow inbound HTTP (80), HTTPS (443).
- Deny traffic from a suspicious IP range.

---

### Comparison: Security Groups vs NACLs

| Feature              | Security Groups                  | Network ACLs                  |
|-----------------------|----------------------------------|--------------------------------|
| Level of Control      | Instance level                  | Subnet level                  |
| Stateful/Stateless    | Stateful                        | Stateless                     |
| Allow/Deny Rules      | Only Allow                      | Allow and Deny                |
| Default Behavior      | Deny all until allowed          | Allow all until changed       |

---

### Example Use Case
- Use a **Security Group** to tightly control access to your EC2 instances (e.g., allow only HTTPS).
- Use a **NACL** to block an entire range of suspicious IP addresses at the subnet level.

---

## Topic C: Interacting with the AWS Global Network

### What is the AWS Global Network?
- AWS operates a **high-speed, private global fiber network** that connects data centers, edge locations, and regions worldwide.
- Ensures **low latency, high availability, and secure communication** between resources.

---

### Key Services for Global Networking
1. **Amazon CloudFront**
    - Content Delivery Network (CDN) that caches content closer to users via **edge locations**.

2. **AWS Global Accelerator**
    - Improves availability and performance by routing traffic through AWS’s global backbone.

3. **VPC Peering & Transit Gateway**
    - Connect VPCs across regions/accounts.
    - Transit Gateway simplifies hub-and-spoke networking.

4. **Direct Connect**
    - Private, dedicated connection between your data center and AWS.
    - Provides lower latency and higher bandwidth than the internet.

---

### Benefits of AWS Global Network
- **Low Latency**: Traffic uses AWS’s private backbone instead of the public internet.
- **High Availability**: Multiple redundant connections.
- **Security**: Built-in encryption and isolation.
- **Global Reach**: Access to resources and users worldwide.

---

### Example Scenario
A company with customers in the USA, Europe, and Asia:
- Uses **CloudFront** to deliver website content with low latency.
- Implements **Global Accelerator** to ensure users connect to the nearest healthy application endpoint.
- Uses **Direct Connect** for secure, high-performance connectivity between its on-premise data center and AWS.

---

