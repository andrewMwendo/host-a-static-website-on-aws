![Alt text].(/Host_a_Static_Website_on_AWS (1).png).


# Host a Static Website on AWS

## Overview
This project demonstrates how to host a static HTML web application on AWS using various AWS resources to ensure high availability, scalability, and security. The web app is deployed on an EC2 instance within a Virtual Private Cloud (VPC) configured with both public and private subnets across multiple availability zones. This README provides a detailed guide on the resources used and the steps followed to deploy the web app.

## Architecture
The project architecture involves the following AWS resources:
1. **VPC Configuration**: A VPC with both public and private subnets across two different availability zones.
2. **Internet Gateway**: Facilitates connectivity between VPC instances and the wider Internet.
3. **Security Groups**: Acts as a network firewall mechanism.
4. **Availability Zones**: Enhances system reliability and fault tolerance by leveraging two availability zones.
5. **Public Subnets**: Used for infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**: Provides secure connections to assets within both public and private subnets.
7. **Private Subnets**: Positions web servers (EC2 instances) for enhanced security.
8. **NAT Gateway**: Allows instances in both the private Application and Data subnets to access the Internet.
9. **EC2 Instances**: Hosts the website.
10. **Application Load Balancer**: Evenly distributes web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
11. **Auto Scaling Group**: Automatically manages EC2 instances to ensure website availability, scalability, fault tolerance, and elasticity.
12. **GitHub**: Stores web files for version control and collaboration.
13. **Certificate Manager**: Secures application communications.
14. **SNS (Simple Notification Service)**: Alerts about activities within the Auto Scaling Group.
15. **Route 53**: Registers the domain name and sets up a DNS record.

## Deployment Steps
Follow the steps below to deploy the static website on AWS:

1. **Configure VPC**:
   - Create a VPC with public and private subnets across two availability zones.
   - Set up an Internet Gateway and attach it to the VPC.
   - Create Security Groups for the instances and configure appropriate inbound and outbound rules.

2. **Set Up Subnets**:
   - Create public subnets for infrastructure components (NAT Gateway, Application Load Balancer).
   - Create private subnets for the EC2 instances.

3. **Launch EC2 Instances**:
   - Use the following script to install and configure the necessary software on the EC2 instances.

```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su
# Update all installed packages to their latest versions
yum update -y
# Install Apache HTTP Server
yum install -y httpd
# Change the current working directory to the Apache web root
cd /var/www/html
# Install Git
yum install git -y
# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git
# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/
# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws
# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd
# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

4. **Configure Load Balancer**:
   - Set up an Application Load Balancer and target group to distribute traffic across the EC2 instances.
   - Ensure the load balancer is associated with the public subnets.

5. **Set Up Auto Scaling**:
   - Create an Auto Scaling Group to manage the EC2 instances.
   - Configure scaling policies to automatically adjust the number of instances based on traffic and performance metrics.

6. **Domain Registration and DNS Configuration**:
   - Register a domain name using Route 53.
   - Create a DNS record to point to the Application Load Balancer.

7. **Security and Monitoring**:
   - Use AWS Certificate Manager to secure application communications.
   - Configure SNS to send notifications about activities within the Auto Scaling Group.

## Repository Structure
The GitHub repository for this project contains:
- The reference diagram illustrating the architecture.
- Scripts used for deployment.
- The static HTML web application files.
