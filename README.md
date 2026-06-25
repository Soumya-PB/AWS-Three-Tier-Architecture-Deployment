# AWS Three-Tier Architecture Deployment

## Project Overview

This project demonstrates the manual deployment of a highly available Three-Tier Architecture on AWS. The architecture separates the application into three layers:

- Presentation Layer (Frontend)
- Application Layer (Backend)
- Database Layer

The solution is designed using multiple Availability Zones for high availability, security, scalability, and fault tolerance.

---

## Architecture Components

### Frontend Layer
- Application Load Balancer (Internet Facing)
- Frontend EC2 Instances
- Frontend Auto Scaling Group
- Private Web Subnets

### Backend Layer
- Internal Application Load Balancer
- Backend EC2 Instances
- Backend Auto Scaling Group
- Private App Subnets

### Database Layer
- Amazon RDS MySQL
- Database Subnet Group
- Private DB Subnets

---

## AWS Services Used

- Amazon VPC
- Public Subnets
- Private Web Subnets
- Private App Subnets
- Private DB Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Application Load Balancer
- EC2 Instances
- Launch Templates
- Auto Scaling Groups
- Amazon RDS MySQL

---

## Network Configuration

| Resource | CIDR Block |
|-----------|-----------|
| VPC | 10.0.0.0/16 |
| Public-Web-Subnet-A | 10.0.0.0/20 |
| Public-Web-Subnet-B | 10.0.16.0/20 |
| Public-Web-Subnet-C | 10.0.32.0/20 |
| Private-Web-Subnet-A | 10.0.48.0/20 |
| Private-Web-Subnet-B | 10.0.64.0/20 |
| Private-Web-Subnet-C | 10.0.80.0/20 |
| Private-App-Subnet-A | 10.0.96.0/20 |
| Private-App-Subnet-B | 10.0.112.0/20 |
| Private-App-Subnet-C | 10.0.128.0/20 |
| Private-DB-Subnet-A | 10.0.144.0/20 |
| Private-DB-Subnet-B | 10.0.160.0/20 |
| Private-DB-Subnet-C | 10.0.176.0/20 |

---

## Architecture Flow

Users → Frontend ALB → Frontend EC2 → Internal Backend ALB → Backend EC2 → Amazon RDS MySQL

---

## Deployment Steps

### Step 1: Create VPC
Create a VPC with CIDR block:

10.0.0.0/16

### Step 2: Create Subnets

Create:
- 3 Public Web Subnets
- 3 Private Web Subnets
- 3 Private App Subnets
- 3 Private DB Subnets

Across:
- Availability Zone A
- Availability Zone B
- Availability Zone C

### Step 3: Create Internet Gateway
Attach Internet Gateway to the VPC.

### Step 4: Create NAT Gateway
Deploy NAT Gateway in Public Subnet A and allocate an Elastic IP.

### Step 5: Configure Route Tables

Public Route Table:
0.0.0.0/0 → Internet Gateway

Private Route Table:
0.0.0.0/0 → NAT Gateway

### Step 6: Create Security Groups

Frontend ALB SG
- HTTP (80)
- HTTPS (443)

Frontend EC2 SG
- HTTP (80) from Frontend ALB

Backend ALB SG
- HTTP (80) from Frontend EC2

Backend EC2 SG
- HTTP (80) from Backend ALB

Database SG
- MySQL (3306) from Backend EC2

### Step 7: Create Database Subnet Group

Include:
- Private DB Subnet A
- Private DB Subnet B
- Private DB Subnet C

### Step 8: Launch RDS MySQL

Configuration:
- Engine: MySQL
- Instance Type: db.t3.micro
- Public Access: Disabled
- Multi-AZ: Enabled

### Step 9: Launch Frontend EC2

Install:
- Apache
- Git

Deploy frontend application from GitHub.

### Step 10: Launch Backend EC2

Install:
- Apache
- PHP
- MySQL Client
- Git

Deploy backend application and configure database connection.

### Step 11: Create Frontend Target Group

Register Frontend EC2 instances.

### Step 12: Create Backend Target Group

Register Backend EC2 instances.

### Step 13: Create Frontend ALB

Attach Frontend Target Group.

### Step 14: Create Backend Internal ALB

Attach Backend Target Group.

### Step 15: Create Launch Templates

Create:
- Frontend Launch Template
- Backend Launch Template

### Step 16: Create Auto Scaling Groups

Frontend ASG:
- Min: 2
- Desired: 2
- Max: 4

Backend ASG:
- Min: 2
- Desired: 2
- Max: 4

---

## Validation

### Frontend Validation

Open:

http://<frontend-alb-dns>

Verify website loads successfully.

### Backend Validation

Verify frontend can communicate with backend API.

### Database Validation

mysql -h <rds-endpoint> -u admin -p

Verify successful database connection.

---

## Project Outcome

Successfully deployed a highly available and scalable Three-Tier Architecture using AWS services with secure communication between all layers.
