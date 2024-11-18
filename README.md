# Telegram Bot with YOLOv5 and AWS Infrastructure Managed by Terraform
This project integrates a Telegram bot (Polybot) with the YOLOv5 AI model to provide object detection for user-uploaded images. Using Terraform, the infrastructure is automated, ensuring scalability, security, and efficient management across AWS services.

Project Overview
Telegram Bot (Polybot):
Allows users to upload images through Telegram.
Responds with object detection results using the YOLOv5 model.
YOLOv5 AI Model:
Processes images and detects objects.
Results are stored in DynamoDB and returned to the user.
Containerized Architecture:
Both Polybot and YOLOv5 are Dockerized and hosted on DockerHub, simplifying deployment.
Terraform-Managed AWS Infrastructure:
Automates provisioning, deployment, and scaling across AWS services.
Ensures secure and reliable communication between components.

Testing Phase
To ensure reliability and precision, the infrastructure was initially deployed manually. This allowed verification of each component's functionality and ensured seamless communication between the Telegram bot, YOLOv5 model, and AWS services.
Once validated, the environment was transitioned to Terraform, which simplifies the deployment process to a few minutes and optimizes cloud workflows.
This repository contains all the Terraform files used to provision the infrastructure, including a well-defined architecture and modular design. Five distinct Terraform modules were created, each specializing in a unique cloud task.
Below is a detailed explanation of these modules, starting with the first: the Internet environment.

AWS Services Used
VPC: Provides a secure network environment with public subnets for application hosting.
Application Load Balancer (ALB): Routes Telegram traffic to Polybot instances.
EC2 Instances: Host Polybot and YOLOv5 applications.
Auto Scaling: Dynamically scales YOLOv5 instances to meet workload demands.
IAM: Grants necessary permissions to securely access AWS resources.
Amazon Machine Image (AMI): Ensures consistent YOLOv5 instance creation.
DynamoDB: Stores object detection results for efficient retrieval.
S3: Stores uploaded images securely.
SQS: Facilitates communication between Polybot and YOLOv5.
Secrets Manager: Manages sensitive data, such as Telegram bot tokens.
CloudWatch: Monitors the health and performance of resources.
Route 53: Manages DNS and domain configurations for the ALB.

Terraform Infrastructure Modules
1. VPC Module
Purpose: Establishes the foundation for the network environment.
Resources Created:
Public subnets across multiple availability zones for redundancy.
SQS for communication between Polybot and YOLOv5.
S3 for storing uploaded images.
DynamoDB for storing processed results.
2. Polybot Module
Purpose: Deploys and configures the Telegram bot.
Resources Created:
EC2 instances running the Polybot application.
Instance Setup:
Configures EC2 with:
Docker containers for Polybot.
IAM Roles to access AWS services (S3, SQS, DynamoDB, and Secrets Manager).
Open ports for:
HTTP (80)
HTTPS (8443 for Telegram communication)
SSH (22 for administrative access).
User Data:
Automates the installation of Docker and deployment of the Polybot container.
3. YOLOv5 Module
Purpose: Manages object detection and scaling.
Resources Created:
Initial EC2 Setup:
Deploys an EC2 instance with YOLOv5 and required dependencies.
Configures IAM Roles to access S3, DynamoDB, and SQS.
AMI Creation:
Converts the YOLOv5 instance into an AMI for consistent auto-scaling.
Auto Scaling:
Dynamically scales YOLOv5 instances based on workload.
CloudWatch Monitoring:
Tracks instance health and performance.
4. ALB Module
Purpose: Handles load balancing for the Telegram bot.
Resources Created:
Configures an Application Load Balancer to route Telegram traffic to Polybot instances.
Key Features:
HTTPS listener on port 8443 for secure Telegram integration.
Route 53 domain management for a custom endpoint.
5. Additional Modules
IAM: Provides permissions for both Polybot and YOLOv5 to interact with AWS services.
Secrets Management:
Ensures sensitive information, such as bot tokens, remains secure.

CI/CD Automation
After validating that the entire infrastructure works seamlessly, a CI/CD process was implemented using GitHub Actions. This automation further streamlined infrastructure deployment and updates.
CI Process
Defining the GitHub Actions Environment:
Configured a GitHub Actions workflow to:
Specify the machine type used for deployment.
Set the Terraform version required for the automation.
Define AWS credentials for secure access to the AWS environment.
Variables and Planning:
Used external variables, such as:
AWS Region
Private keys
Other dynamic inputs
Ran terraform plan to validate configurations and ensure no errors.
Applying Infrastructure:
Executed terraform apply to provision the infrastructure from scratch.

CD Process
Dynamic Configuration with Ansible:
Ansible was installed on Polybot instances to manage variables and configurations automatically.
Created an inventory.ini file containing the IP addresses of Polybot machines for seamless setup.
Automated Deployment of Polybot Container:
Extracted necessary variables dynamically, such as:
AWS Region
DynamoDB table name
ALB URL
S3 bucket name
SQS queue name
Used an Ansible playbook to deploy the Polybot container across machines uniformly and efficiently.
Outcome
This CI/CD pipeline enables:
Automated infrastructure deployment and updates with Terraform (CI).
Quick and consistent configuration and container deployment with Ansible (CD).
Scalable and error-free processes, reducing setup time significantly.

