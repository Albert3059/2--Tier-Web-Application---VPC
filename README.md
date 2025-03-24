# AWS Auto Scaling Group with Load Balancer

## Overview
This project demonstrates setting up a scalable and highly available web application on AWS using Auto Scaling Groups, an Application Load Balancer, and a Bastion Host. The setup ensures fault tolerance by distributing traffic across multiple instances in different availability zones.

## AWS Services Deployed

1. **VPC (Virtual Private Cloud)** - Custom VPC to isolate resources.
2. **Internet Gateway** - Enables communication between AWS resources and the internet.
3. **Availability Zones** - Two AZs for high availability.
4. **Public Subnets** - Two public subnets for NAT and Bastion Host.
5. **Private Subnets** - Two private subnets for application servers.
6. **Auto Scaling Group** - Automatically scales instances in private subnets.
7. **Security Groups** - Controls traffic flow for private and public instances.
8. **Application Load Balancer (ALB)** - Routes requests to the most healthy instance.
9. **NAT Gateway** - Allows private instances to access the internet for updates ensuring private resources remain isolated and secure.
10. **Bastion Host** - Provides SSH access to private instances.

## Architecture Diagram
![image](https://github.com/user-attachments/assets/94e1f88e-4191-471e-ba36-77e1620a0f3a)


## Setup Steps

### 1. **Create VPC & Subnets**
- Create a VPC with a CIDR block (e.g., `10.0.0.0/16`).
- Create two public subnets 
- Create two private subnets 

### 2. **Set Up Internet & NAT Gateway**
- Attach an **Internet Gateway** to the VPC.
- Create **NAT Gateways** in public subnets for private instances.

### 3. **Launch Application Instances**
- Use Auto Scaling Group to launch instances in private subnets.
- Connect via **Bastion Host** (SSH into it, then into private instances).
- Deploy a simple HTML page via Python’s HTTP server.
  

### 4. **Configure Load Balancer**
- Create an **Application Load Balancer (ALB)** in public subnets.
- Attach a **Target Group** to forward traffic to private instances.
- Set up **Health Checks** to route traffic to healthy instances.

### 5. **Deploy Application**
- Install a basic HTML page on each private instance:
  
 **Connect via Bastion Host to the privTE instance and install the web application**
   - Used the following to connect to private instances:.  
   - First ssh to the Bastion Host Instance
     ```sh
     ssh -i my-key.pem ec2-user@bastion-public-ip
     ```
   - Within the Baston Host instance then you can ssh to the private instances
      ```sh
          ssh -i my-key.pem ec2-user@private-instance-ip
     ```

- Use Nano : is a simple terminal-based text editor. Try this:

```Nano
nano index.html
```
- Insert the html text below

#### **Instance 1 (AZ 1):**
```html
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<h1>Welcome to Auto Scaling Group with AWS</h1>
<p>AZ 1, Application Instance 1.</p>
</body>
</html>
```

#### **Instance 2 (AZ 2):**
```html
<!DOCTYPE html>
<html>
<head>
<title>Page Title</title>
</head>
<body>
<h1>Welcome to Auto Scaling Group with AWS</h1>
<p>AZ 2, Application Instance 2.</p>
</body>
</html>
```
-To save, press CTRL + X, then Y, and press Enter.

-Run the web server on each private instance
```
python3 -m http.server 8000
```


### 6. **Access the Application**
- Copy the **Load Balancer DNS Name** and access the application via a browser.
- The page should load from different instances based on availability.

## Challenges & Learnings
1. **SSH Key Permissions Issue**
   - Encountered `Permissions too open` error with `.pem` key file.
   - Fixed by running:
     ```sh
     chmod 400 my-key.pem
     ```


     

3. **Health Check & Load Balancer Failover**
   - Ensured the ALB correctly forwarded requests only to healthy instances.

## Conclusion
This setup allows an auto-scaling web application with high availability and secure access. The **Application Load Balancer** ensures smooth traffic distribution, while the **Bastion Host** secures instance access.

