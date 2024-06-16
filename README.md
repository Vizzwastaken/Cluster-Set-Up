---

# AWS Infrastructure Deployment with Terraform

This Terraform configuration script sets up a basic AWS infrastructure including a VPC, subnets, route tables, internet gateway, NAT gateway, IAM roles, EKS cluster, and EKS node group.

## Prerequisites

Before you begin, ensure you have the following installed:

- Terraform CLI ([Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli))

## Setup

1. **Clone the Repository**

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Initialize Terraform**

   Initialize Terraform with the AWS provider:

   ```bash
   terraform init
   ```

3. **Review Configuration**

   Review the `main.tf` file to ensure it matches your requirements and AWS account setup.

4. **Deploy Infrastructure**

   Apply the Terraform configuration to deploy the infrastructure:

   ```bash
   terraform apply
   ```

   Enter `yes` when prompted to confirm deployment.

## Structure

### VPC

- Creates a VPC (`aws_vpc.test`) with CIDR block `10.0.0.0/16`.

### Subnets

- **Public Subnet:** (`aws_subnet.Public`)
  - CIDR block: `10.0.1.0/24`
  - Availability Zone: us-east-1

- **Private Subnet 1a:** (`aws_subnet.Private-1a`)
  - CIDR block: `10.0.2.0/24`
  - Availability Zone: us-east-1a

- **Private Subnet 1b:** (`aws_subnet.Private-1b`)
  - CIDR block: `10.0.6.0/27`
  - Availability Zone: us-east-1b

### Route Tables

- **Public Route Table:** (`aws_route_table.Public`)
  - Routes traffic to an internet gateway (`aws_internet_gateway.igw`) for the public subnet.

- **Private Route Table:** (`aws_route_table.Private`)
  - Routes traffic to a NAT gateway (`aws_nat_gateway.nat`) for the private subnets.

### Internet Gateway

- Creates an internet gateway (`aws_internet_gateway.igw`) and attaches it to the VPC.

### NAT Gateway

- Creates an EIP (`aws_eip.nat`) and a NAT gateway (`aws_nat_gateway.nat`) for outbound traffic from private subnets.

### IAM Roles

- **EKS Cluster Role:** (`aws_iam_role.eks_cluster_role`)
  - Role for Amazon EKS cluster with policies attached for EKS service and worker nodes.

- **EKS Node Group Role:** (`aws_iam_role.eks_node_group_role`)
  - Role for Amazon EKS worker nodes with policies attached for networking and container registry access.

### Amazon EKS Cluster

- Creates an Amazon EKS cluster (`aws_eks_cluster.demo`) named "demo" in the specified VPC and subnets.

### Amazon EKS Node Group

- Creates an Amazon EKS node group (`aws_eks_node_group.worker_nodes`) attached to the EKS cluster for workload deployment.

## Cleanup

To avoid incurring charges, destroy the Terraform-managed infrastructure after use:

```bash
terraform destroy
```

Enter `yes` when prompted to confirm destruction.

## Notes

- Ensure you have appropriate AWS credentials configured (`~/.aws/credentials`).
- Review and customize the variables in `main.tf` as per your specific requirements.
- For production use, consider adding more security measures and configuring other AWS resources like security groups, S3 buckets, etc.

---

This README provides an overview of the infrastructure created by your Terraform script, setup instructions, and guidelines for further customization or cleanup. Adjust as necessary based on your specific deployment needs and AWS environment.
