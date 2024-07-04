# Terraform-and-Ansible

# Terraform
## Overview:
Terraform is an open-source IaC tool by HashiCorp that allows you to define and provision infrastructure using a high-level configuration language. It supports multiple cloud providers and on-premises data centers.

# Key Features:
Provider Plugins: Support for a wide range of providers (AWS, Azure, GCP, etc.).
State Management: Keeps track of your infrastructure's state to manage updates.
Execution Plans: Previews changes before applying them.
Resource Graph: Models dependencies between resources.
# Basic Workflow:

Write Configuration:

## Define resources using HCL (HashiCorp Configuration Language)
```
 provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}

```

## Initialize:

Initialize the configuration directory.
Command: 
```
terraform init

```

## Plan

Generate an execution plan.

```

 terraform plan

```

## Apply

Apply the changes to reach the desired state.

```

terraform apply

```

## Destroy

Remove all infrastructure managed by the configuration.

```

terraform destroy

```

# Tools and Best Practices:

# Terraform CLI: Command-line interface to manage Terraform workflows.

Commands: terraform init, terraform plan, terraform apply, terraform destroy.

## Remote State Management:

Store state files remotely for collaboration and state locking.

## Modules:

Reusable configurations to manage infrastructure as code efficiently.

## Version Control:

Store Terraform configurations in version control systems like Git.
Implement CI/CD pipelines to automate Terraform workflows.

# Defining Infrastructure as Code
## Basic Infrastructure Configuration:
### Create a Basic EC2 Instance:

Add the following to your main.tf file:

```

provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "TerraformExample"
  }
}



```


## Networking and Security Groups:

Define a VPC, subnets, and security groups in your main.tf file:
hcl

```

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "main-vpc"
  }
}

resource "aws_subnet" "subnet" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-west-2a"
}

resource "aws_security_group" "allow_ssh" {
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}


```


## Create a More Complex EC2 Instance:

Update the aws_instance resource to use the VPC and security group

```

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.subnet.id
  security_groups = [aws_security_group.allow_ssh.name]

  tags = {
    Name = "TerraformExample"
  }
}


```

## Managing State and Collaboration
## Remote State Management:
Configure Remote State Storage:

Use a backend to store the state file remotely (e.g., S3 for AWS

```

terraform {
  backend "s3" {
    bucket = "my-terraform-state-bucket"
    key    = "path/to/my/key"
    region = "us-west-2"
  }
}



```

# Automation and CI/CD
## Integrate with CI/CD Tools:
### Using Jenkins:

```

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/your-terraform-project.git'
            }
        }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }
}


```

### Using GitHub Actions:

```

name: 'Terraform CI/CD'

on:
  push:
    branches:
      - main

jobs:
  terraform:
    name: 'Terraform'
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v1

      - name: Terraform Init
        run: terraform init

      - name: Terraform Plan
        run: terraform plan

      - name: Terraform Apply
        run: terraform apply -auto-approve


```


# Ansible

## Overview:

Ansible is an open-source automation tool for configuration management, application deployment, and task automation. It uses YAML-based playbooks to describe automation tasks.

### Key Features:
Agentless: Connects to remote nodes over SSH.
Idempotent: Ensures the system reaches the desired state without unintended changes.
Modular: Uses modules to perform tasks (e.g., install packages, manage services).

# Basic Workflow:
## Install Ansible:

Install Ansible on a control node.

```

sudo apt-get install ansible (on Debian-based systems).

```
## Inventory:
Define the hosts to manage.

## Playbook:

Write playbooks to define tasks.

## Run Playbook:
Execute the playbook to apply changes.

```

ansible-playbook site.yml

```
# Tools and Best Practices:

## Ansible Galaxy:
Use and share Ansible roles.

```

ansible-galaxy install <role_name>

```

## Roles:

Organize playbooks into reusable roles.

## Vault:

Encrypt sensitive data in playbooks.

```

ansible-vault encrypt, ansible-vault decrypt

```

## CI/CD Integration:

Integrate Ansible with CI/CD pipelines for automated deployments.

## Conclusion
Terraform and Ansible are powerful tools for managing infrastructure and configuration in a DevOps environment. Terraform excels in provisioning infrastructure as code, while Ansible is ideal for configuration management and application deployment. Together, they enable a comprehensive automation strategy.
