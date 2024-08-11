# PROJECT OVERVIEW
This is project demonstrates how to use Terraform to create and manage various AWS cloud services.

## PREREQUISITIES
The following softwares are required to accomplish this project:
1. Visual Studio Code
2. Terraform extension in Visual Studio Code
3. AWS CLI Configure extension installed in Visual Studio Code
4. GIT: Version control system 
An AWS account is require with the appropriate permissions and access tokens to acess the account in AWS CLI.

## INSTRUCTIONS

1. **Setting up the Provider**
- Create a provider.tf, Specify AWS Provider and set Terraform Version
- Set the AWS Region in the general_variables.tf file,  referenced by the region variable
For this we have used terraform version: terraform-provider-aws_5.62.0_SHA256SUMS and Region: eu-north-1

![Provider.](/Provider.png)
![Region](/Region%20variable.png)

2. **Setting up VPC**
- Create a vpc_variable.tf file then specify to allocate the CIDR block range. For this project we have allocated 10.0.0/16. 
![Region](/VPC%20variable.png)


- Create Subnets:
 - Create a public subnet with a CIDR block range of 10.0.0.0/24 and assign it a public IP. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet a(public-a), without a public IP and a CIDR block range of 10.0.1.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet b(web-private-a), without a public IP and a CIDR block range of 10.0.2.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

 - Create private subnet c(web-private-b), without a public IP and a CIDR block range of 10.0.3.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

  - Create private subnet d(app-private-a), without a public IP and a CIDR block range of 10.0.4.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.

   - Create private subnet e(app-private-b), without a public IP and a CIDR block range of 10.0.7.0/24. Use variables to reference the VPC and AWS region defined earlier, and a Tag for labelling the resource.
   ![Subnet1](/Subnet%20pic%201.png)
   ![Subnet2](/Subnet%20pic%202.png)

   3. Setting Up NAT Gateway and Internet Gateway
   - Create a gateways.tf file to congigure:
     -  The  Elastic IP (EIP) that will be associated with the NAT Gateway. Include a Tag for labelling the resource.
     - A NaT Gateway that depends on the previously created Elastic IP and allocates ID from the Elastic IP, and is associated with subnet from the subnets defined earlier. Include a Tag for labelling the resource.
   - Create an internet gateway, referencing to the earlier created VPC as it attched to the VPC for the resources to communicate to the internet, and a Tag for labelling the resource.

 ![Gatewayspic](/Gateways%20pic.png)

4. **Step 4: Setting Up Network ACLs (NACLs)**

- Create a nacl.tf file to configure: 
 - A public NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A public subnet to associate with the NACL and and a Tag for labelling the resource.
   ![Gatewayspic](/NACL%201.png)

 - A Web Private NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules simialr to the Public NACL: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Gatewayspic](/NACL%202.png)

   - A Web Private NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules simialr to the Public NACL: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Gatewayspic](/NACL%203.png)

   - A Bastion Public NACL, attach this NACL to the earlier created VPC by referencing the VPC ID. Set the Ingress and Egress Rules simialr to the other NACLs: by setting it to allow any incoming traffic and outgoing traffic from any IP address.
  - A Private subnet to associate with the NACL and and a Tag for labelling the resource.
  ![Gatewayspic](/NACL%204.png)

5. **Setting Up Route Tables and Associations**
- Create a route_tables.tf file to configure the following:
 - A Public Subnet Route Table, attach this route table to the  earlier created VPC by referencing the VPC ID.  Allow traffic to the internet by setting the cidr_block to 0.0.0.0/0 and linking it to the internet gateway created earlier. Add a Tag for labelling the resource.
  - Create Route Table Association, and : Link this route table to the public subnet, and a Tag for labelling the resource.
   ![Gatewayspic](/RT%201.png)

  






