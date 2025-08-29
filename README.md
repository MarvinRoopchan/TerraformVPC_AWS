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

