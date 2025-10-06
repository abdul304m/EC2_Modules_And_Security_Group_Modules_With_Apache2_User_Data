# EC2_Modules_And_Security_Group_Modules_With_Apache2_User_Data 

## Project Documentation: Terraform EC2 Apache Deployment
Project Overview

This project demonstrates how to use Terraform to provision infrastructure on AWS in a modular way.
It automates:

- Creation of a Security Group

- Deployment of an EC2 instance

- Installation and configuration of Apache2 via a UserData script

The project follows best practices by separating resources into reusable modules.

## Deployment Process

1. Security Group Module
Created a security group allowing inbound traffic on ports 22 (SSH) and 80 (HTTP).

2. EC2 Module
Created an EC2 instance using Amazon Linux 2 AMI, associated with the above security group.

3. UserData Script (apache_userdata.sh)
Automatically installed and configured Apache2, created a simple web page, and ensured the service runs on startup.

4. Main Configuration
Linked both modules and applied the configuration using:terraform init
terraform apply

5. Verification
After deployment, accessing the EC2 public IP in a browser displayed:" Hello from Terraform EC2 Apache Server!"

## Observations

The modular approach makes the project reusable and easier to maintain.

Using UserData simplifies post-deployment setup without manual SSH.

Terraform outputs (like EC2 public IP) make it convenient to retrieve connection details.

The infrastructure can be replicated easily across environments (e.g., dev/test/prod) just by changing variables.

## Challenges Faced & Resolutions
Challenge	Description	Resolution
AWS Permissions	Initially encountered IAM permission errors during deployment.	Updated the IAM user policy to include AmazonEC2FullAccess and AmazonVPCFullAccess.
Region-Specific AMI	The provided AMI was not valid in my chosen AWS region.	Used the AWS Console to find a valid Amazon Linux 2 AMI ID for the correct region.
Subnet & VPC IDs	Terraform failed when invalid subnet or VPC IDs were provided.	Verified and replaced with existing VPC/Subnet IDs from AWS Management Console.
UserData Not Executing	Apache wasn’t installed after first run.	Ensured the script used correct OS package manager (yum for Amazon Linux 2) and set the script as executable (chmod +x apache_userdata.sh).
Port 80 Access	Couldn’t access Apache page initially.	Added proper ingress rules in the Security Group module to allow HTTP (port 80) from 0.0.0.0/0.
Resource Cleanup	Forgetting to destroy resources left running EC2s.	Used terraform destroy after testing to avoid AWS charges.

## Conclusion

This project successfully automated the deployment of an Apache2 web server on an AWS EC2 instance using Terraform.
The modular structure, automation via UserData, and clean configuration design demonstrate the effectiveness of Infrastructure as Code (IaC) using Terraform.