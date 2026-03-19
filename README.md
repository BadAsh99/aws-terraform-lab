# aws-terraform-lab

> AWS VPC, EC2, and networking fundamentals with Terraform

![Terraform](https://img.shields.io/badge/Terraform-IaC-purple?logo=terraform)
![AWS](https://img.shields.io/badge/AWS-boto3-orange?logo=amazonaws)

---

## Overview

A foundational AWS infrastructure lab built with Terraform. Deploys a complete VPC networking stack with a public subnet, Internet Gateway, and EC2 instance — demonstrating core AWS IaC patterns and provider configuration. Uses dynamic AMI lookup to always resolve the latest Ubuntu 22.04 image at plan time.

---

## Architecture

```
AWS Account
└── VPC (10.0.0.0/16)
        ├── Public Subnet (10.0.1.0/24)
        │       └── EC2 Instance (Ubuntu 22.04, t2.micro)
        │               └── Security Group
        │                       └── Inbound: SSH (22) from 0.0.0.0/0
        │                       └── Outbound: all
        ├── Internet Gateway
        └── Public Route Table
                └── 0.0.0.0/0 → Internet Gateway
```

---

## Features

- VPC with configurable CIDR block
- Public subnet with auto-assigned public IPs
- Internet Gateway and public route table
- Security group with SSH access (lab configuration)
- EC2 instance on t2.micro (free-tier eligible)
- Dynamic AMI data source — always resolves latest Ubuntu 22.04 HVM image
- SSH key-pair authentication
- Output: instance public IP for immediate access

---

## Tech Stack

| Component | Technology |
|-----------|-----------|
| IaC | Terraform, aws ~3.0 |
| Cloud | Amazon Web Services |
| Compute | EC2 (t2.micro, Ubuntu 22.04) |
| Networking | VPC, IGW, Subnet, Route Table, Security Group |

---

## Getting Started

### Prerequisites

- AWS CLI configured (`aws configure`)
- Terraform v1.5+
- An existing EC2 key pair in your target region

### Deploy

```bash
git clone https://github.com/BadAsh99/aws-terraform-lab.git
cd aws-terraform-lab
cp terraform.tfvars.example terraform.tfvars
# Edit terraform.tfvars: set region, key_name
terraform init
terraform plan
terraform apply
```

### Connect

```bash
ssh -i ~/.ssh/your-key.pem ubuntu@$(terraform output -raw instance_public_ip)
```

---

## Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `aws_region` | AWS region | `us-east-1` |
| `vpc_cidr` | VPC CIDR block | `10.0.0.0/16` |
| `key_name` | EC2 key pair name | — |

---

## Outputs

| Output | Description |
|--------|-------------|
| `instance_public_ip` | Public IP of the EC2 instance |
| `vpc_id` | VPC ID |

---

## Cleanup

```bash
terraform destroy
```

---

## Author

**Ash Clements** — Sr. Principal Security Consultant | Infrastructure Automation
[github.com/BadAsh99](https://github.com/BadAsh99)
