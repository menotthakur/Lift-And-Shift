# AWS Lift and Shift - vProfile Application

## Overview
This project involves migrating the **vProfile** multi-tier web application from a local data center to **AWS** using the **Lift and Shift** strategy. The goal is to run application workloads on AWS with scalability, automation, and cost efficiency.

## Project Objectives
- Migrate the application without modifying its architecture.
- Implement **Infrastructure as Code (IaC)** for automation.
- Utilize AWS **pay-as-you-go** model for cost efficiency.
- Ensure security and high availability with AWS services.

## AWS Services Used
- **Compute:** EC2 instances for Tomcat, RabbitMQ, Memcache, and MySQL.
- **Load Balancing:** Elastic Load Balancer (ELB) for traffic distribution.
- **Scaling:** Auto Scaling Group (ASG) for dynamic resource allocation.
- **Storage:** Amazon S3 for artifact storage, EBS for block storage.
- **DNS:** Route 53 for private DNS resolution.
- **Security:** IAM for access control, ACM for HTTPS encryption.

## Architectural Design
1. **User Access:**
   - Users access the application via a domain configured in **GoDaddy DNS**.
   - Traffic is directed to an **Elastic Load Balancer (ELB)**.

2. **Load Balancer:**
   - Uses HTTPS with an **ACM** certificate.
   - Routes requests to **Auto Scaling Group (ASG)** managing **Tomcat instances**.

3. **Application Servers:**
   - Runs on EC2 instances within a **private security group**.
   - Communicates with backend services via **Route 53 private DNS**.

4. **Backend Services:**
   - MySQL, Memcache, and RabbitMQ run on separate EC2 instances.
   - Each service is within its own security group.

## Execution Workflow
### **1. AWS Setup**
- Create an **AWS account** and configure billing.
- Set up **IAM roles** and security policies.

### **2. Security Groups Configuration**
- Load Balancer: Allow **HTTPS (443)**
- Tomcat Instances: Allow traffic from **ELB on port 8080**
- Backend Services: Allow **MySQL (3306), Memcache (11211), RabbitMQ (5672)**

### **3. Deploy EC2 Instances**
- Launch instances with **user data scripts** for auto-provisioning.
- Assign the correct **key pair** for SSH access.

### **4. Route 53 Private DNS Setup**
- Create a **private hosted zone**.
- Map backend servers using **private IPs**.

### **5. Application Build and Deployment**
- Build the **artifact locally** using Maven.
- Upload the artifact to **S3 bucket**.
- Deploy from **S3 to Tomcat EC2 instances**.

### **6. Load Balancer and DNS Configuration**
- Configure ELB to distribute requests to **Tomcat instances**.
- Point GoDaddy DNS to ELB endpoint.

### **7. Verification & Auto Scaling**
- Verify connectivity between components.
- Set up **Auto Scaling Group (ASG)** for high availability.

## Project Benefits
✅ **High availability** via ELB and Auto Scaling.  
✅ **Automation** through user data scripts and IaC.  
✅ **Security** with IAM, security groups, and ACM certificates.  
✅ **Cost optimization** using pay-as-you-go AWS services.  

## Future Enhancements
- Implement **CI/CD pipelines** for automated deployments.
- Use **Terraform or CloudFormation** for full IaC automation.
- Migrate to **AWS Managed Services** (e.g., RDS, ElastiCache).

