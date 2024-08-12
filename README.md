# PROJECT OVERVIEW

This is project demonstrates how to use Terraform to create and manage various AWS cloud services, including
- VPC
- EC2 Instances
- Load Balancers
- Autoscaling Groups
- Security Groups
- Launch Templates
- IAM Roles

## PREREQUISITES

The following softwares are required to accomplish this project:
1. Visual Studio Code
2. Terraform 
3. AWS CLI Configure
4. GIT: Version control system 
An AWS account is require with the appropriate permissions and access tokens to access the account in AWS CLI.

## INSTRUCTIONS

1. **Setting up the Provider**

- Create a provider.tf, Specify AWS Provider and set Terraform Version
- Set the AWS Region in the general_variables.tf file,  referenced by the region variable
For this we have used terraform version: terraform-provider-aws_5.62.0 and Region: eu-north-1

![Provider.](/Screenshots/Provider.png)
![Region](/Screenshots/Region%20variable.png)

2. **Setting up VPC**

- Create a vpc_variable.tf and vpc.tf file then specify to allocate the CIDR block range. For this project we have allocated 10.0.0/16. 
![VPC](/Screenshots/VPC%20variable.png)
![VPC](/Screenshots/vpc%20image%201.png)

**Create Subnets:**

 - Create a public subnet(public-subnetA) with a CIDR block range of 10.0.0.0/24 and assign it a public IP. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet a(private-subnetA), without a public IP and a CIDR block range of 10.0.1.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet b(web-private-B), without a public IP and a CIDR block range of 10.0.2.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet c(app-private-subnetA), without a public IP and a CIDR block range of 10.0.3.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

  - Create private subnet d(app-private-subnetB), without a public IP and a CIDR block range of 10.0.4.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

   - Create private subnet e(bastion-public-subnetA), without a public IP and a CIDR block range of 10.0.7.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.
   ![Subnet1](/Screenshots/Subnet%20pic%201.png)
   ![Subnet2](/Screenshots/Subnet%20pic%202.png)

   3. **Setting Up NAT Gateway and Internet Gateway**
   - Create a gateways.tf file to configure:
     -  The  Elastic IP (EIP) that will be associated with the NAT Gateway. Include a Tag for labelling the resource.
     - A NAT Gateway that depends on the previously created Elastic IP and allocates ID from the Elastic IP, and is associated with subnet from the subnets defined earlier. Include a Tag for labelling the resource.
   - Create an Internet Gateway, referencing to the earlier created VPC as it attached to the VPC for the resources to communicate to the internet, and a Tag for labelling the resource.

 ![Gatewayspic](/Screenshots/Gateways%20pic.png)

4. **Setting up NACLs**

- Create a nacl.tf file to configure: 
 - A public NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A public subnet to associate with the NACL and and a Tag for labelling the resource.
   ![Nacl1](/Screenshots/NACL%201.png)

 - A Web Private NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules similar to the Public NACL: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Nacl2](/Screenshots/NACL%202.png)

   - A App Private NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules similar to the Public NACL: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Nacl3](/Screenshots/NACL%203.png)

   - A Bastion Public NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules similar to the other NACLs: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Nacl4](/Screenshots/NACL%204.png)


5. **Setting Up Route Tables and Associations**

Create a route_tables.tf file to configure the following:
 - A Public Subnet Route Table, attach this route table to the  earlier created VPC by referencing the VPC ID.  Allow traffic to the internet by setting the cidr_block to 0.0.0.0/0 and linking it to the internet gateway created earlier. Add a Tag for labelling the resource.
  - Create Route Table Association, and Link this route table to the public subnet.

    ![RouteTable](/Screenshots/RT%201.png)

 - A Web Private Subnet Route Table, attach this route table to the earlier created VPC by referencing the VPC ID.  A Route outbound traffic through the NAT gateway by setting the cidr_block to 0.0.0.0/0 and linking it to the NAT gateway created earlier. Add a Tag for labelling the resource.
  - Create Route Table Association, and Link this route table to the private subnets for the web servers.
   ![RouteTable2](/Screenshots/RT%202.png)


 - An App Private Subnet Route Table, attach this route table to the  earlier created VPC by referencing the VPC ID.  Route outbound traffic through the NAT gateway by setting the cidr_block to 0.0.0.0/0 and linking it to the NAT gateway created earlier. Add a Tag for labelling the resource.
  - Create Route Table Association, and Link this route table to the private subnets for the application servers.
   ![RouteTable3](/Screenshots/RT%203.png)

 - A Bastion Public Subnet Route Table:, attach this route table to the  earlier created VPC by referencing the VPC ID.  Allow traffic to the internet by setting the cidr_block to 0.0.0.0/0 and linking it to the internet gateway created earlier. Add a Tag for labelling the resource.
  - Create Route Table Association, and Link this route table to the bastion public subnet.
   ![RouteTable4](/Screenshots/RT%204.png)

6. **Setting up Web and App Servers**

- Create a web_app.tf file to configure the following:
  - Resource - aws-instance- 'web-server'
  - AMI(Amazon Machine Image) - ami-07c8c1b18ca66bb07
  - Instance type - set to t3.micro
  - A key pair - my key pair is JosephKey
  - Subnet Association: Deploy this instance in the private subnet created earlier for web servers
  - Assign the security group for web servers
  - Add a Tag for labelling the resource.
  - EBS Volume: Set the EBS volume configurations.
  ![Webserver](/Screenshots/Web%20server%201.png)
  
- Resource - aws-instance- 'app-server'
  - AMI(Amazon Machine Image) - ami-07c8c1b18ca66bb07
  - Instance type - set to t3.micro
  - A key pair - my key pair is JosephKey
  - Subnet Association: Deploy this instance in the private subnet created earlier for app servers
  - Assign the security group for app servers
  - Add a Tag for labelling the resource.
  - EBS Volume: Set the EBS volume configurations.
  ![Appserver](/Screenshots/App%20server%201.png)

7. **Setting up IAM Role and Instance Profile**
- Create a aws_role.tf file to configure: 
  - Resource - AWS IAM role - bootcamp_server_role
  - Attach Role Policy: Allow EC2 instances to assume this role by specifying with an "Allow" effect.
  - Add a tag for labelling the resource.
   ![AWSRole](/Screenshots/AWS%20role%201.png)
- Create Instance profile: 
   - Instance Profile Name: server_profile.
   - Specify policy to attach: the bootcamp_server_role to this instance profile.
- Role Policy Attachment:
  - Attach the 'AmazonEC2RoleforSSM' managed policy to the bootcamp_server_role.
   ![AWSRole](/Screenshots/AWS%20role%202.png)

8. **Setting up Security Groups**
- Create securitygroups.tf file to configure:
  - Resource - AWS Security Group - web-elb-security-group
  - Set the name, description and VPC ID, referencing to the earlier created VPC.
  - Set Allow inbound traffic on ports 80 (HTTP) and 443 (HTTPS) from any IP address for Ingress rules.
  - Set Allow all outbound traffic for Egress rules.
  - Set a Tag for labelling the resource
 ![SG1](/Screenshots/Security%20Group%201.png)

- Resource - AWS Security Group - web-app-security-group
  - Set the name, description and VPC ID, referencing to the earlier created VPC.
  - Set Allow inbound traffic on ports on port 22 (SSH), traffic on ports 80 (HTTP) and 443 (HTTPS), fo the Ingress rules.
  - Set Allow all outbound traffic for the Egress rules.
  - Set a Tag for labelling the resource
 ![SG1](/Screenshots/Security%20Group%202.png)
  ![SG1](/Screenshots/Security%20Group%203.png)

- Resource - AWS Security Group - bastion-security-group:
  - Set the name, description and VPC ID, referencing to the earlier created VPC.
  - Set allow inbound traffic on port 22 (SSH) for the Ingress rules.
  - Set allow all outbound traffic for the Egress rules.
  - Set a Tag for labelling the resource
 ![SG1](/Screenshots/Security%20Group%204.png)

9. **Setting Load Balancer**
- Create web_app_lb.tf file to configure:
  - Resource - application load balancer
  - Ensure the load balancer is public (internal = false) and of type "application". 
  - Associate the ALB with the security group created for the web load balancer.
  - Deploy the ALB in the public subnets created earlier.

- Configuring the Listener for the ALB:
  - Create an HTTP listener on port 80 that redirects traffic to HTTPS on port 443.
  - The listener is associated with the ALB created earlier.
   ![LB1](/Screenshots/Load%20Balancer%201.png)

-  Setting Up the Target Group:
   - Configure a target group named ${var.alb_name}-app-tg for the application servers.
   - Set the protocol to HTTP and port to 80.
   - The target type is set to "instance" to register EC2 instances as targets.
   - Define a health check to monitor the targets
      ![LB2](/Screenshots/Load%20Balancer%202.png)

- Attaching Instances to the Target Group:
   - Attach the web server instance to the target group on port 80.
  - Attach the application server instance to the target group on port 80.
  ![LB3](/Screenshots/Load%20Balancer%203.png)

10. **Auto Scaling Group Configuration**

- File: auto_scaling_groups.tf
- Purpose: Define auto-scaling groups for web and app servers.
- Web Server Auto Scaling Group:
   - Name Prefix: "web-server-group-"
   - Scaling Configuration: Minimum size of 1, maximum size of 2, and desired capacity set to 1.
   - Health Check Type: Set to "EC2" to monitor instance health.
   - Subnet Association: Deploy instances within the private subnet for web servers.
   - Launch Template: Reference the web_server_template created in _launch_temp.tf using the latest version.

- App Server Auto Scaling Group:
  - Name Prefix: "app-server-group-"
  - Scaling Configuration: Minimum size of 1, maximum size of 2, and desired capacity set to 1.
  -  Health Check Type: Set to "EC2" to monitor instance health.
  - Subnet Association: Deploy instances within the private subnet for app servers.
  ![ASG](/Screenshots/Autoscaling%20group%20.png)

- Launch Template Configuration:
  - Create file: _launch_temp.tf
  - Purpose: Define launch templates for the auto-scaling groups of web and app servers.
  - **Web Server:**
  - Set EC2 Configurations: Reuse earlier settings for AMI, instance type, key pair, and IAM instance profile.
  - Security Group: Associate with the previously defined security group for web and app servers.
  - Tagging: Apply resource tags for identification.
  -  Configure EBS volumes with the same parameters set earlier.
  ![LT1](/Screenshots/Launch%20template%201.png)

- **App Server**
  - Set EC2 Configurations: Reuse earlier settings for AMI, instance type, key pair, and IAM instance profile.
  - Security Group: Associate with the previously defined security group for web and app servers.
  - Tagging: Apply resource tags for identification.
  ![LT2](/Screenshots/Launch%20template%202.png)

  ## DEPLOYING SERVICES 
  1. **Terraform init**
 Run terraform init to set up the working directory and download necessary plugins.
 ![TR](/Screenshots/Terraform%201.png)

 2. **Terraform plan**
 Use terraform plan to preview the changes that will be made to your infrastructure.
  ![TR](/Screenshots/Terraform%202.png)
3. **Terraform apply**
Execute terraform apply to create the infrastructure as per the configuration.
  ![TR](/Screenshots/Terraform%203.png)


**The AWS console showing the created resources**
1. **VPC**
  ![vpc](/Screenshots/VPC%20IMAGE.png)

2. **SUBNETS**
  ![TR](/Screenshots/SUBNET%20PIC%20AWS.png)

3. **ROUTE TABLES**
  ![TR](/Screenshots/ROUTE%20TABLES%20AWS.png)

4. **INTERNET GATEWAY**
  ![TR](/Screenshots/INTERNET%20GATEWAY%20AWS.png)

5. **NACLS**
  ![TR](/Screenshots/NACL%20AWS.png)

6. **NAT GATEWAY**
  ![TR](/Screenshots/NAT%20GATEWAY%20AWS.png)

7. **SECURITY GROUP**
  ![TR](/Screenshots/SECURITY%20GROUP%20AWS.png)

8. **EC2 INSTANCES**
 ![TR](/Screenshots/EC2%20INSTANCES%20AWS.png)

9. **LAUNCH TEMPLATES**
 ![TR](/Screenshots/LAUNCH%20TEMPLATE%20AWS.png)

10. **LOAD BALANCERS**
 ![TR](/Screenshots/LOAD%20BALANCERS%20AWS.png)

11. **AUTOSCALING GROUP**
 ![TR](/Screenshots/AUTO%20SCALING%20GROUPS%20AWS.png)

12. **TARGET GROUPS**
 ![TR](/Screenshots/TARGET%20GROUP%20AWS.png)

13. **IAM ROLES**
 ![TR](/Screenshots/IAM%20ROLE%20AWS.png)

14. **ELASTIC IP**
 ![TR](/Screenshots/ELASTIC%20IP%20AWS.png)



## Terraform destroy
 Run terraform destroy to remove all resources managed by Terraform.
  ![TR](/Screenshots/Terraform%204.png)








  







  






