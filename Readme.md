# TerraForm-Jenkins-AWS-Pipeline

[![Infrastructure](https://img.shields.io/badge/Infrastructure-Terraform-623CE4?style=flat&logo=terraform)](https://www.terraform.io/)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-Jenkins-D33833?style=flat&logo=jenkins)](https://www.jenkins.io/)
[![Cloud](https://img.shields.io/badge/Cloud-AWS-FF9900?style=flat&logo=amazon-aws)](https://aws.amazon.com/)
[![Container](https://img.shields.io/badge/Container-Docker-2496ED?style=flat&logo=docker)](https://www.docker.com/)

A complete Infrastructure as Code (IaC) solution that automates AWS infrastructure provisioning using Terraform with Jenkins CI/CD pipeline integration. This project demonstrates enterprise-level DevOps practices including modular infrastructure design, automated deployments, and secure state management.

## 🏗️ Architecture Overview

This project provisions a complete AWS infrastructure stack including:

- **VPC with Public/Private Subnets** - Network isolation and segmentation
- **EC2 Instances** - Auto-configured with Docker and application deployment
- **Security Groups** - Granular network access control
- **IAM Roles & Policies** - Secure resource access management
- **KMS Encryption** - Data encryption at rest and in transit
- **S3 Backend** - Centralized Terraform state management
- **Key Pair Management** - Secure SSH access provisioning

## 🚀 Key Features

### Infrastructure as Code
- **Modular Design**: Reusable Terraform modules for network, security, compute, and storage
- **Multi-Environment Support**: Separate workspaces for dev, staging, and production
- **State Management**: Remote S3 backend with DynamoDB locking
- **Encryption**: KMS-based encryption for sensitive resources

### CI/CD Pipeline
- **Parameterized Builds**: Flexible deployment configurations
- **Approval Gates**: Manual approval for critical operations
- **Workspace Management**: Automatic environment isolation
- **Artifact Management**: Secure key pair archiving
- **Error Handling**: Comprehensive error handling and rollback capabilities

### Security & Best Practices
- **Least Privilege Access**: IAM roles with minimal required permissions
- **Network Security**: Properly configured security groups and NACLs
- **Encryption**: End-to-end encryption using AWS KMS
- **IP Whitelisting**: Configurable source IP restrictions
- **Automated Key Rotation**: Configurable KMS key rotation policies

## 📋 Prerequisites

- **Jenkins Server** with the following plugins:
  - Pipeline
  - Git
  - AWS Pipeline Steps
- **AWS CLI** configured with appropriate credentials
- **Terraform** >= 1.0
- **Docker** (for containerized applications)
- **Git** access to your repository

## 🛠️ Installation & Setup

### 1. Jenkins Configuration

```bash
# Install Jenkins dependencies (run on Jenkins server)
chmod +x jenkins-setup.sh
./jenkins-setup.sh
```

### 2. AWS Configuration

Ensure your Jenkins server has AWS credentials configured:

```bash
aws configure
# OR use IAM roles for EC2 instances
```

### 3. S3 Backend Setup

Create an S3 bucket for Terraform state:

```bash
aws s3 mb s3://your-terraform-state-bucket
```

## 🚦 Usage

### Jenkins Pipeline Parameters

| Parameter | Description | Default | Example |
|-----------|-------------|---------|---------|
| `ENVIRONMENT` | Target environment | `dev` | dev, staging, prod |
| `AWS_REGION` | AWS deployment region | `eu-west-2` | us-west-2, eu-central-1 |
| `PROJECT_NAME` | Project identifier | `docker` | my-webapp |
| `VPC_CIDR` | Network CIDR block | `172.16.0.0/16` | 10.0.0.0/16 |
| `EC2_INSTANCE_TYPE` | Instance size | `t2.micro` | t3.medium |
| `MY_IP` | Admin IP for access | Required | 203.0.113.0/32 |
| `ACTION` | Terraform operation | `apply` | apply, destroy |

### Deployment Steps

1. **Access Jenkins Dashboard**
   - Navigate to your Jenkins instance
   - Select "New Item" → "Pipeline"

2. **Configure Pipeline**
   - Point to your repository
   - Branch: `tf_module/cicd-jenkins`

3. **Execute Deployment**
   - Click "Build with Parameters"
   - Configure your environment settings
   - Review and approve the plan
   - Monitor deployment progress

4. **Access Your Infrastructure**
   - SSH key will be archived as build artifact
   - Use the output IP address to access your instance

### Infrastructure Destruction

To safely destroy infrastructure:

1. Set `ACTION` parameter to `destroy`
2. Review the destruction plan
3. Approve the destruction when prompted

## 📁 Project Structure

```
├── modules/
│   ├── network/          # VPC, subnets, routing
│   ├── security/         # Security groups, NACLs
│   ├── ec2/             # Compute instances
│   ├── iam/             # Identity and access management
│   ├── kms/             # Key management service
│   ├── s3/              # Storage buckets
│   └── keypair/         # SSH key pair management
├── jenkins-setup.sh     # Jenkins server setup script
├── Jenkinsfile         # CI/CD pipeline definition
├── main.tf             # Main Terraform configuration
├── variables.tf        # Variable definitions
├── terraform.tf       # Provider and backend configuration
├── outputs.tf          # Output definitions
└── README.md           # This file
```

## 🔧 Customization

### Adding New Modules

1. Create module directory: `modules/new-module/`
2. Add module call in `main.tf`
3. Define required variables in `variables.tf`
4. Update pipeline if additional parameters needed

### Environment-Specific Configurations

Each environment uses separate Terraform workspaces:

```hcl
# Automatically managed by pipeline
terraform workspace select ${ENVIRONMENT}
```

### Security Customization

Update security group rules in `modules/security/`:

```hcl
ingress {
  from_port   = 80
  to_port     = 80
  protocol    = "tcp"
  cidr_blocks = [var.my_ip]
}
```

## 📊 Monitoring & Troubleshooting

### Common Issues

1. **Backend Configuration Errors**
   - Verify S3 bucket exists and is accessible
   - Check IAM permissions for backend operations

2. **Resource Conflicts**
   - Ensure unique resource naming across environments
   - Check for existing resources in target region

3. **Network Connectivity**
   - Verify security group configurations
   - Check route table associations

### Logs & Debugging

- Jenkins build logs contain detailed Terraform output
- AWS CloudTrail for API call auditing
- VPC Flow Logs for network troubleshooting

## 🔐 Security Considerations

- **Secrets Management**: Use Jenkins credentials store for sensitive data
- **Network Isolation**: Resources deployed in private subnets where possible
- **Encryption**: All data encrypted using KMS keys
- **Access Control**: Role-based access with principle of least privilege
- **Audit Trail**: Complete logging of all infrastructure changes

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Olawale Fagbule** - DevOps Engineer

- GitHub: [@slimboi](https://github.com/slimboi)
- LinkedIn: [Olawale Fagbule](https://www.linkedin.com/in/olawale-fagbule/)
- Email: ola.fagbule@gmail.com

## 🙏 Acknowledgments

- HashiCorp for Terraform
- AWS for cloud infrastructure
- Jenkins community for CI/CD automation
- Docker for containerization technology

---

**⭐ If this project helped you, please consider giving it a star!**