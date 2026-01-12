# Project-3-Tier-Web-Architecture-AWS

<img width="1289" height="573" alt="Screenshot 2026-01-11 233402" src="https://github.com/user-attachments/assets/9b46d856-ca57-4edc-8df9-45c02603ea69" />

<h2>Prerequisite:</h2>

* S3
* IAM
* VPC
* Subnets
* Route Tables
* Internet Gateway
* NAT gateway
* Security Groups

### $\color{blue} \textbf{Create \textbf S3 }$

- Name: project-3-tier-2596
- AWS Region: ap-south-1
- Create

### $\color{blue} \textbf{Create \textbf IAM }$

- Roles
- Trusted entity: AWS Service
- Use case: EC2
- Add permissions: 1. AmazonSSMManagedInstanceCore.
                   2. AmazonS3ReadOnlyAccess
- Create
