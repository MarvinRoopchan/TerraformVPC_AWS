# Terraform AWS VPC Infrastructure

This project provisions a **highly available VPC network** in AWS using Terraform.  
It follows the standard **3-tier VPC design** with public and private subnets spread across multiple Availability Zones (AZs).  

---

## 📐 Architecture Overview

The Terraform configuration builds the following resources:

- **VPC** (`10.0.0.0/16`) with DNS hostnames and DNS support enabled.  
- **3 Public Subnets** (one in each AZ: `us-east-1a`, `us-east-1b`, `us-east-1c`)  
  - Auto-assign public IP enabled  
  - Used for Internet-facing resources (e.g., Load Balancer, Bastion, NAT Gateway)  
- **3 Private Subnets** (one in each AZ: `us-east-1a`, `us-east-1b`, `us-east-1c`)  
  - No public IPs  
  - Used for databases and application servers  
- **Internet Gateway (IGW)** attached to the VPC (for public internet access).  
- **NAT Gateway** deployed in a public subnet, with an Elastic IP (for outbound access from private subnets).  
- **Public Route Table**  
  - `0.0.0.0/0 → IGW`  
  - Associated with all public subnets  
- **Private Route Table**  
  - `0.0.0.0/0 → NAT Gateway`  
  - Associated with all private subnets  

---

## 📊 Network Diagram

```mermaid
graph TD
  A[VPC 10.0.0.0/16] --> B1[Public Subnet 1a<br>10.0.1.0/24]
  A --> B2[Public Subnet 1b<br>10.0.2.0/24]
  A --> B3[Public Subnet 1c<br>10.0.3.0/24]

  A --> C1[Private Subnet 1a<br>10.0.4.0/24]
  A --> C2[Private Subnet 1b<br>10.0.5.0/24]
  A --> C3[Private Subnet 1c<br>10.0.6.0/24]

  B1 --> IGW[Internet Gateway]
  B1 --> NAT[NAT Gateway (EIP)]

  C1 -->|0.0.0.0/0| NAT
  C2 -->|0.0.0.0/0| NAT
  C3 -->|0.0.0.0/0| NAT

