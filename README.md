

# Terraform AWS VPC Module

This Terraform module provisions an AWS Virtual Private Cloud (VPC) with public and private subnets, internet gateway, NAT gateway, and route tables. It is designed to be reusable and easily configurable for different environments.


## Overview

This module creates the following AWS resources:
- VPC with customizable CIDR block.
- Public and private subnets across multiple availability zones.
- Internet Gateway (IGW) for public subnet internet access.
- NAT Gateway (NAT) for private subnet internet access.
- Route tables and route table associations.


## Usage

### Terraform Example

```hcl
module "vpc" {
  source = "./vpc"
  
  env                = "dev"
  vpc_cidr_block     = "10.0.0.0/16"
  private_subnets    = {
    "10.0.0.0/19" = "us-east-1a"
    "10.0.32.0/19" = "us-east-1b"
  }
  public_subnets     = {
    "10.0.64.0/19" = "us-east-1a"
    "10.0.96.0/19" = "us-east-1b"
  }
  private_subnet_tags = {
    "Owner" = "dev-team"
  }
  public_subnet_tags = {
    "Owner" = "dev-team"
  }
}
```

Run the following commands to deploy the infrastructure:

```bash
terraform init
terraform apply
```

## Inputs

| Name                 | Description                                             | Type         | Default         | Required |
|----------------------|---------------------------------------------------------|--------------|-----------------|----------|
| `env`                | Environment name (e.g., dev, prod)                      | `string`     | `"dev"`         | Yes      |
| `vpc_cidr_block`      | The CIDR block for the VPC                              | `string`     | `"10.0.0.0/16"` | Yes      |
| `private_subnets`     | Map of private subnet CIDR blocks to availability zones | `map(string)`| See example     | Yes      |
| `public_subnets`      | Map of public subnet CIDR blocks to availability zones  | `map(string)`| See example     | Yes      |
| `private_subnet_tags` | Additional tags for private subnets                     | `map(any)`   | `{}`            | No       |
| `public_subnet_tags`  | Additional tags for public subnets                      | `map(any)`   | `{}`            | No       |

## Outputs

| Name                   | Description                              |
|------------------------|------------------------------------------|
| `vpc_id`               | The ID of the VPC                        |
| `private_subnet_ids`    | The IDs of the private subnets           |
| `public_subnet_ids`     | The IDs of the public subnets            |

## Resources

This module provisions the following resources:

| Resource Type             | Resource Name              |
|---------------------------|----------------------------|
| `aws_vpc`                 | VPC                        |
| `aws_internet_gateway`     | Internet Gateway           |
| `aws_nat_gateway`          | NAT Gateway                |
| `aws_eip`                 | Elastic IP for NAT Gateway |
| `aws_subnet`              | Public and Private Subnets |
| `aws_route_table`         | Route Tables               |
| `aws_route_table_association` | Route Table Associations |

