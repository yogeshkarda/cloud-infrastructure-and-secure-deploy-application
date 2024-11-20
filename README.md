# cloud-infrastructure-and-secure-deploy-application
AWS project for deploying application for production environment
Step 1: Create a VPC
Go to VPC Console > Create VPC.
Choose VPC only.
Set:
CIDR block: e.g., 10.0.0.0/16
Tenancy: Default (for cost-effectiveness)
Step 2: Create Subnets
Create two private subnets:
Subnet 1 (e.g., 10.0.1.0/24) in Availability Zone A.
Subnet 2 (e.g., 10.0.2.0/24) in Availability Zone B.
Create two public subnets:
Subnet 3 (e.g., 10.0.3.0/24) in Availability Zone A.
Subnet 4 (e.g., 10.0.4.0/24) in Availability Zone B.
Step 3: Create and Attach an Internet Gateway
Go to Internet Gateways > Create Internet Gateway.
Attach the Internet Gateway to your VPC.
Step 4: Create NAT Gateways
Create a NAT Gateway in Subnet 3 (Public Subnet in AZ A).
Create a NAT Gateway in Subnet 4 (Public Subnet in AZ B).
Assign Elastic IPs to both NAT Gateways.
Step 5: Configure Route Tables
Public Route Table:
Associate with the public subnets.
Add a route to the Internet Gateway (0.0.0.0/0 → IGW).
Private Route Tables:
Create two route tables for the private subnets (one per AZ).
Add routes to the respective NAT Gateway in the same AZ (0.0.0.0/0 → NAT Gateway).
Step 6: Deploy an Auto Scaling Group and EC2 Instances
Create a Launch Template or Launch Configuration:
Configure EC2 instance type, AMI, key pair, and security group.
Create an Auto Scaling Group:
Define the desired, minimum, and maximum instance count.
Specify private subnets for placement.
Configure health checks (ELB-based recommended).
Step 7: Deploy an Application Load Balancer
Go to EC2 Console > Load Balancers > Create Load Balancer.
Choose Application Load Balancer.
Configure:
Scheme: Internet-facing
Subnets: Select public subnets.
Add target group:
Type: Instance
Target: EC2 instances in private subnets.
Configure security groups to allow HTTP/HTTPS traffic.
Step 8: Test and Validate
Confirm that the ALB's DNS name routes traffic to your servers.
Ensure NAT Gateways allow outgoing internet traffic from private subnets.
Check Auto Scaling policies and ensure instances are deployed across AZ
