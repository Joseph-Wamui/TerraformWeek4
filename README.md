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

1. Setting up the Provider
- Create a provider.tf, Specify AWS Provider and set Terraform Version
- Set the AWS Region in the general_variables.tf file,  referenced by the region variable
For this we have used terraform version: terraform-provider-aws_5.62.0_SHA256SUMS and Region: eu-north-1

![Provider.](/Provider.png)
![Region](/Region%20variable.png)

2. Setting up VPC
- Allocate the CIDR block range, for this project we have allocated 10.0.0/16, the 10.0 ssection has been set in variable of vpc_variable.tf file
- Create Subnets:
 - Public Subnet a, with a pulic IP and CIDR block range of 10.0.0/24, with the following varible refrencinng the earlier crrated file: VPC and avaulability zone
