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
* RDS
* Subnet groups
* Databases
* EC2

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

Two nat gateway create 

1)
- Name: NAT-GW-AZ1
- Availability mode: Zonal
- Subnet: Public-Web-Subnet-AZ-1
- Allocate Elastic IP
- Create

2)
- Name: NAT-GW-AZ2
- Availability mode: Zonal
- Subnet: Public-Web-Subnet-AZ-2
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

<h2>Create tow Private Route Table</h2>

1.

- Name: Private-RT-AZ1
- VPC: vpc-07af23475dacb4dbd 3tier
- Create

<h2>Edit routes</h2>

- Add route: 0.0.0.0/0
- Target: NAT Gateway- NAT-GW-AZ1
- Save

<h2>Edit subnet associations</h2>

Attach subnet in Private Route Table
- Available subnets: Private-App-Subnet-AZ-1

2.

- Name: Private-RT-AZ2
- VPC: vpc-07af23475dacb4dbd 3tier
- Create

<h2>Edit routes</h2>

- Add route: 0.0.0.0/0
- Target: NAT Gateway- NAT-GW-AZ2
- Save

<h2>Edit subnet associations</h2>

Attach subnet in Private Route Table
- Available subnets: Private-App-Subnet-AZ-2

### $\color{blue} \textbf{Create \textbf Security Groups }$

five Security group creating for this project

1.The first security group you’ll create is for the public, internet facing load balancer. After typing a name and description, add an inbound rule to allow HTTP type traffic for your IP.

<img width="1920" height="913" alt="Screenshot (146)" src="https://github.com/user-attachments/assets/a4090729-196a-4c6e-b3f3-84744c1d8c0c" />



2. The second security group you’ll create is for the public instances in the web tier. After typing a name and description, add an inbound rule that allows HTTP type traffic from your internet facing load balancer security group you created in the previous step. This will allow traffic from your public facing load balancer to hit your instances. Then, add an additional rule that will allow HTTP type traffic for your IP. This will allow you to access your instance when we test.

<img width="1920" height="870" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/10359934-a551-42e9-8a61-a1b9baca4785" />



3. The third security group will be for our internal load balancer. Create this new security group and add an inbound rule that allows HTTP type traffic from your public instance security group. This will allow traffic from your web tier instances to hit your internal load balancer.


<img width="1920" height="905" alt="Screenshot (148)" src="https://github.com/user-attachments/assets/f4587a4e-5d73-4817-8b7c-13bf6da1f9b9" />


4. The fourth security group we’ll configure is for our private instances. After typing a name and description, add an inbound rule that will allow TCP type traffic on port 4000 from the internal load balancer security group you created in the previous step. This is the port our app tier application is running on and allows our internal load balancer to forward traffic on this port to our private instances. You should also add another route for port 4000 that allows your IP for testing.


<img width="1920" height="908" alt="Screenshot (149)" src="https://github.com/user-attachments/assets/cf5f4eeb-bb87-4ecd-a411-a07d6e1ddb92" />


5. The fifth security group we’ll configure protects our private database instances. For this security group, add an inbound rule that will allow traffic from the private instance security group to the MYSQL/Aurora port (3306).


<img width="1920" height="904" alt="Screenshot (150)" src="https://github.com/user-attachments/assets/a2814228-4318-4da6-8689-7a01d324e2d5" />


- last final view for the security group

<img width="1920" height="909" alt="Screenshot (151)" src="https://github.com/user-attachments/assets/9ced0aeb-e2ae-4c90-82bb-cd4dcb697af1" />

### $\color{blue} \textbf{Subnet \textbf groups }$

<h3>Create DB subnet group</h3>

- Give your subnet group a name, description, and choose the VPC we created.

<img width="1920" height="675" alt="Screenshot (152)" src="https://github.com/user-attachments/assets/71b994f1-acb3-4433-843d-aa338c9f5a07" />

- When adding subnets, make sure to add the subnets we created in each availability zone specificaly for our database layer. You may have to navigate back to the VPC dashboard and check to make sure you're selecting the correct subnet IDs.

<img width="1920" height="912" alt="Screenshot (153)" src="https://github.com/user-attachments/assets/98347ba5-5116-4637-9bd6-04ac4bf82498" />


 ### $\color{blue} \textbf{Create \textbf Databases }$

  - Choose a database creation method: Full configuration
  - Engine type: Aurora (MySQL Compatible)
  - Templates: Dev/Test
  - DB cluster identifier: database-1
  - Master username: admin
  - Credentials management: Self managed
  - set password: ***********
  - Availability & durability: Create an Aurora Replica or Reader node in a different AZ (recommended for scaled availability)
    
  <h4>Connectivity </h4>
  
  - Compute resource: Don’t connect to an EC2 compute resource
  - Virtual private cloud (VPC): 3 tier (custom vpc)
  - DB subnet group: three-tier-db-subnet-group
  - Public access: No
  - VPC security group (firewall)
  - Choose existing
  - Existing VPC security groups: DBSG
  - create database
  
  <img width="1920" height="909" alt="Screenshot (158)" src="https://github.com/user-attachments/assets/116c0d0f-50c2-436e-a3bd-73fcd0cf083f" />

  <h3>Instance</h3>

  - Name: App layer
  - O.S: Amazon linux
  - Amazon Machine Image (AMI): Amazon linux 2023 kernel-6.1 AMI
  - Instance type: t2.micro
  - Key pair: proceed without a key pair (not recommended)
  - VPC: vpc-07af23475dacb4dbd 3tier
  - Subnet: Private-App-Subnet-AZ-1
  - Auto-assign public IP: Disable
  - security groups: Select existing security group -> privateinstanceSG

    <h5>Advanced details </h5>

    - IAM instance profile: demo-ec2role
    - Launch Instance

  

