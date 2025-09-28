# EKS Cluster with Terraform

This project provisions an Amazon EKS (Elastic Kubernetes Service) cluster using Terraform, following best practices for networking and IAM.

## Architecture Diagram
![EKS Architecture Diagram](terraforming%20eks%20using%20modules.gif)

- Use AWS official icon set in draw.io for a professional look.
- Group EKS resources inside the VPC shape.
- Use arrows to show dependencies (e.g., EKS Module uses VPC outputs).
- Label each block for clarity.

## Why Use Both EKS and VPC Modules?
During setup, we encountered errors such as:
- `Unsupported argument: create_vpc`
- `Unsupported argument: vpc_cidr`

These errors occurred because the EKS module (v19.x and v20.x) does not manage VPC creation directly. Instead, it expects an existing VPC and subnets. To resolve this and follow AWS/Terraform best practices, we now use:
- The official [terraform-aws-modules/vpc/aws](https://github.com/terraform-aws-modules/terraform-aws-vpc) module to create the VPC and subnets.
- The latest [terraform-aws-modules/eks/aws](https://github.com/terraform-aws-modules/terraform-aws-eks) module to provision the EKS cluster, referencing the VPC outputs.

This approach avoids unsupported argument errors, provides full control over networking, and ensures compatibility with future module updates.

## Structure
- **main.tf**: Provisions the VPC, subnets, and EKS cluster using the official modules.

## Key Features
- Three public and three private subnets in different availability zones
- NAT gateway for private subnet internet access
- EKS cluster with version 1.27
- Managed node group with autoscaling
- IAM roles for cluster and node group, following AWS best practices

## Usage
1. Initialize Terraform:
   ```bash
   terraform init
   ```
2. Review the plan:
   ```bash
   terraform plan
   ```
3. Apply the configuration:
   ```bash
   terraform apply
   ```

## Security
- State files and sensitive files are excluded from version control via `.gitignore`.
- IAM roles are defined with least privilege for EKS operation (managed by the module).

## Customization
- Edit `main.tf` to change cluster/node group settings or module options.
- For advanced customizations, refer to the [terraform-aws-modules/eks/aws documentation](https://github.com/terraform-aws-modules/terraform-aws-eks) and [terraform-aws-modules/vpc/aws documentation](https://github.com/terraform-aws-modules/terraform-aws-vpc).

---

**Note:** Make sure your AWS credentials are configured before running Terraform.
