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

### $\color{blue} \textbf{Create \textbf VPC }$

- Name: 3tier
- IPv4CIDR: 10.0.0.0/16
- create

### $\color{blue} \textbf{Create \textbf Subnet }$

- VPC ID: vpc-07af23475dacb4dbd | 3tier
- Subnet name: Public-Web-Subnet-AZ-1
- Availability Zone: aps1-az1 (ap-south-1a)
- IPv4 CIDR: 10.0.0.0/24
- Create subnet

We will need six subnets across two availability zones. That means that three subnets will be in one availability zone, and three subnets will be in another zone. Each subnet in one availability zone will correspond to one layer of our three tier architecture. Create each of the 6 subnets by specifying the VPC we created and then choose a name, availability zone, and appropriate CIDR range for each of the subnets.

<img width="1920" height="912" alt="Screenshot (140)" src="https://github.com/user-attachments/assets/7f64e6ac-8bfe-46d2-bb84-42b601f594b2" />

### $\color{blue} \textbf{Create \textbf Internet Gateway }$

- Name: three-tier-igw
- create
- Attach to VPC
- Slecte VPC
- Attach Internet Gateway

### $\color{blue} \textbf{Create \textbf NAT Gateway }$

- Name: NAT-GW-AZ1
- Availability mode: Zonal
- Subnet: Public-Web-Subnet-AZ-1
- Allocate Elastic IP
- Create

 ### $\color{blue} \textbf{Create \textbf Route Tables }$

 - Name: PublicRouteTable
 - VPC: vpc-07af23475dacb4dbd 3tier
 - Create

<h2>Edit routes</h2>

- Add route: 0.0.0.0/0
- Target: Internet Gateway
- Save

<h2>Edit subnet associations</h2>

Attach 2 subnet in Public Route Table
- Available subnets: 1) Public-Web-Subnet-AZ-1. 2) Public-Web-Subnet-AZ-2

<h2>Private Route Table</h2>

- Name: Private-RT-AZ1
- VPC: vpc-07af23475dacb4dbd 3tier
- Create

<h2>Edit routes</h2>

- Add route: 0.0.0.0/0
- Target: NAT Gateway
- Save

<h2>Edit subnet associations</h2>

Attach subnet in Private Route Table
- Available subnets: Private-App-Subnet-AZ-2
