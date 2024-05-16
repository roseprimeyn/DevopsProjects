# Deploy-Scalable-VPC-Architecture-on-AWS-Cloud

## TABLE OF CONTENTS
1. Use Case

2. Pre-Requisites

3. Pre-Deployment

4. VPC Deployment

5. Validation
   
6. Lesson Learned & Challenge

## Use Case

Company A, a fast-growing e-commerce platform, faced limitations with their existing AWS infrastructure. 
Their initial setup utilized a single VPC with limited resources, struggling to handle the surging traffic during peak seasons. 
This resulted in performance bottlenecks, impacting customer experience and hindering business growth. 
Scaling up the existing infrastructure wasn't cost-effective, and manual resource management proved unsustainable.
Company A required a more scalable and efficient cloud architecture to accommodate traffic fluctuations and support their ambitious growth plans. 
They needed a solution that could automatically adapt to changing resource demands, optimize costs, and ensure high availability of their e-commerce platform

## Pre-Requisites
1. You must be having an AWS account to create infrastructure resources on AWS cloud.
2. Source Code

## Pre-Deployment
Customize the application dependencies mentioned below on AWS EC2 instance and create the Golden AMI.

1. AWS CLI
2. Install Apache Web Server
3. Install Git
4. Cloudwatch Agent
5. Push custom memory metrics to Cloudwatch.
6. AWS SSM Agent

## VPC Deployment

1. Build VPC network ( 192.168.0.0/16 ) for Bastion Host deployment as per the architecture shown above.
2. Build VPC network ( 172.32.0.0/16 ) for deploying Highly Available and Auto Scalable application servers as per the architecture shown above.
3. Create NAT Gateway in Public Subnet and update Private Subnet associated Route Table accordingly to route the default traffic to NAT for outbound internet connection.
4. Create Transit Gateway and associate both VPCs to the Transit Gateway for private communication.
5. Create Internet Gateway for each VPC and Public Subnet associated Route Table accordingly to route the default traffic to IGW for inbound/outbound internet connection.
6. Create Cloudwatch Log Group with two Log Streams to store the VPC Flow Logs of both VPCs.
7. Enable Flow Logs for both VPCs and push the Flow Logs to Cloudwatch Log Groups and store the logs in the respective Log Stream for each VPC.
8. Create Security Group for bastion host allowing port 22 from public.
9. Deploy Bastion Host EC2 instance in the Public Subnet with EIP associated.
10. Create S3 Bucket to store application specific configuration.
11. Create Launch Configuration with below configuration.
    
    i. Golden AMI
    
    ii.Instance Type – t2.micro
    
    iii. Userdata to pull the code from Bitbucket Repository to document root folder of webserver and start the httpd service.
    
    iv.IAM Role granting access to Session Manager and to S3 bucket created in the previous step to pull the configuration. (Do not grant S3 Full Access)
    
     v. Security Group allowing port 22 from Bastion Host and Port 80 from Public.
    
     vi. Key Pair
    
13. Create Auto Scaling Group with Min: 2 Max: 4 with two Private Subnets associated to 1a and 1b zones.
14. Create Target Group and associate it with ASG.
15. Create Network Load balancer in Public Subnet and add Target Group as target.
16. Update route53 hosted zone with CNAME record routing the traffic to NLB.

## Validation
1. As DevOps Engineer login to Private Instances via Bastion Host.
2. Login to AWS Session Manager and access the EC2 shell from console.
3. Browse web application from public internet browser using domain name and verify that page loaded.

## Lesson Learned & Challenge
Lesson: A well-designed VPC with multiple Availability Zones and Auto Scaling allows you to handle traffic fluctuations and growth without limitations.

Challenge:  Scaling up a single VPC might not be cost-effective and manual resource management becomes cumbersome as complexity increases.

