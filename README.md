# <p style="text-align: center;">Provisioning AWS EKS resources using Terraform</p>

## Overview
This project demonstrates the use of Terraform to provision AWS Elastic Kubernetes Service (EKS) and its associated infrastructure. It includes VPC creation, subnets, and an EKS cluster with managed node groups. Terraform modules and best practices are used to achieve modular, reusable, and manageable infrastructure.

## Architecture
The infrastructure includes the following components:

1. **VPC**:
   - CIDR Block: `10.0.0.0/16`
   - 3 Public Subnets and 3 Private Subnets
   - NAT Gateway for private subnets

2. **EKS Cluster**:
   - EKS version: `1.27`
   - Public API endpoint access
   - Managed node group with 3 nodes of instance type `t2.small` (scalable between 1 and 3 nodes)

3. **Networking Tags**:
   - Subnets are tagged for Kubernetes load balancers (public and internal ELBs).

## File Structure
- **`providers.tf`**: Defines required providers and their versions.
- **`vpc.tf`**: Provisions the VPC and subnets using the `terraform-aws-modules/vpc/aws` module.
- **`eks-cluster.tf`**: Provisions the EKS cluster using the `terraform-aws-modules/eks/aws` module.
- **`terraform.tfvars`**: Defines variable values for the VPC CIDR block and subnets.

## Prerequisites
Before you begin, ensure you have the following:

1. Terraform installed on your local machine.
2. AWS CLI installed and configured with a profile (`iamadmin-general` used here).
3. IAM permissions to create the resources mentioned above.

## Usage

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. **Initialize Terraform:**
   ```bash
   terraform init
   ```

3. **Plan the infrastructure:**
   ```bash
   terraform plan -var-file="terraform.tfvars"
   ```

4. **Apply the configuration:**
   ```bash
   terraform apply -var-file="terraform.tfvars"
   ```

5. **Verify resources:**
   - Use the AWS Console to check the VPC, subnets, and EKS cluster.
   - Use `kubectl` to interact with the cluster after configuring the kubeconfig.

6. **Destroy the infrastructure (if needed):**
   ```bash
   terraform destroy -var-file="terraform.tfvars"
   ```

## High-Level Design (HLD)

Below is the high-level design of the architecture:

- **VPC**
  - CIDR Block: 10.0.0.0/16
  - Public Subnets (for Load Balancers):
    - Subnet1: 10.0.4.0/24
    - Subnet2: 10.0.5.0/24
    - Subnet3: 10.0.6.0/24
  - Private Subnets (for Worker Nodes):
    - Subnet1: 10.0.1.0/24
    - Subnet2: 10.0.2.0/24
    - Subnet3: 10.0.3.0/24

- **EKS Cluster**
  - Name: myapp-eks-cluster
  - Version: 1.27
  - Node Group: dev
    - Instance Type: t2.small
    - Desired Nodes: 3
    - Maximum Nodes: 3

- **NAT Gateway**
  - Single NAT Gateway for private subnets

- **Tags**
  - Kubernetes-specific subnet tags for ELB and internal ELB functionality

## Additional Notes
- **Modules Used:**
  - `terraform-aws-modules/vpc/aws` for VPC and subnets
  - `terraform-aws-modules/eks/aws` for EKS provisioning
- **Terraform Version:** Ensure Terraform is updated to the latest version to support the specified module versions.

## HLD Diagram
The following diagram provides a high-level overview of the architecture:
![HLD](/aws-eks-terraform/HLD.png)