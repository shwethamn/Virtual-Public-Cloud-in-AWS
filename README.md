VPC (Virtual Private Cloud)
A VPC allows you to create a logically isolated network where you can define IP address ranges, subnets, route tables, and security settings
 

 
IPV4: 32-bits address
8bit – 00000000 to 11111111
                 0          to    1
0.0.0.0	to 255.255.255.255
Ex: 192.168.0.1
11000000.10101000.00000000.00000001
CIDR: Classless Inter Domain Routing
VPC: 192.168.0.0/24                         IPV4: 32 bit address
Network Portion: 24
Host Portion: 32-24=8            2^8=256
192.168.0.0 to 192.168.0.255
Here you will get 256 host IP address
Break VPC into 2 subnets:
Subnet 1 (128)
192.168.0.0/25
Network Portion: 25
Host Portion: 32-25=7    2^7=128
192.168.0.0 to 192.168.0.127
Subnet2 (128)
192.168.0.0/25
Network Portion: 25
Host Portion: 32-25-7   2^7=128
192.168.0.127 to 192.168.0.255
Amazon reserves the first four (4) IP address and the last one(1) IP address of every subnet for IP networking purpose.
128-5=123  So you will get 123 IP address
Note:
 VPCs per Region -5
Subnets per VPC -200
IPv4 CIDR blocks per VPC -5
Prerequisites:
1.	AWS Account: Ensure you have an AWS account. If not, sign up at AWS.
2.	AWS CLI or Management Console: You can perform these steps using the AWS Management Console or AWS CLI.
3.	Creating a VPC (Virtual Private Cloud) in AWS is an essential step for setting up your network architecture in the cloud. A VPC allows you to create a logically isolated network where you can define IP address ranges, subnets, route tables, and security settings. Here's how to create a VPC in AWS step by step:

Step 1: Log in to AWS Management Console
o	Navigate to AWS Management Console.
 

o	Sign in with your AWS credentials.
 

 


Step 2: Navigate to the VPC Dashboard
•	In the AWS Management Console, search for VPC in the search bar or find it under the Networking & Content Delivery section.
 
•	Click on VPC to go to the VPC dashboard.
 
Step 3: Create a VPC
•	In the left sidebar, click Create VPC.
 
•	Choose a VPC wizard (for simplicity, you can use the "VPC with a Single Public Subnet" option to start):
 
•	VPC Settings:
1. Name: Give your VPC a name (e.g., my-vpc).
2. IPv4 CIDR block: Enter the IP range you want for your VPC, e.g., 192.168.0.0/24. This will allow you to have 255 IP addresses.
3. Tenancy: Choose between Default or Dedicated. "Default" means your instances will share hardware, while "Dedicated" means each instance gets isolated hardware.
              4. Click Create VPC.

 

Step 4: Create an Internet Gateway
If you want your VPC to have access to the internet, you need to create an Internet Gateway and attach it to your VPC:
1. In the left sidebar, click Internet Gateways.
2. Click Create internet gateway.
3. Give it a name and click Create.
 
4. After creating it, click on the Actions button, then Attach to VPC.
5. Select your VPC and click Attach.
 


Step 5: Create Subnets
•	After your VPC is created, you need to create subnets.
1.	In the left sidebar, click Subnets.
 
2.	Click Create subnet.
 
3.	Select your VPC and specify:
                           Subnet Name: Give a name to the subnet
                           Availability Zone: Choose an availability zone within your region.
              CIDR Block: Specify the range of IP addresses for your subnet. You might use                           something like 192.168.0.0/25 for your first subnet.
Public subnet:
 
Private subnet:
 
4.	Click Create.
You can create multiple subnets for different purposes (e.g., public, private, or for different availability zones).
 

Step 6: Create Route Tables
A route table controls the routing for traffic within your VPC. You need to associate it with your subnets
1.	In the left sidebar, click Route Tables.
 

2.	Click Create route table and give it a name.

 



3.	Once created associated with subnets. In  Subnet Associations tab, click Edit subnet associations, and select the subnet that you want to route internet traffic through (typically the public subnet).

 
 
Public-rt associate with public subnet
Private-rt associated with private subnet
 
4.	Once created, select the route table, go to the Routes tab, and add a new route to allow internet access:
•	Destination: 0.0.0.0/0 (for all IPs)
•	Target: Select your Internet Gateway.
 
 



Step 7: Launch an EC2 Instance (Optional)
If you want to launch an EC2 instance into your VPC:
1.	Go to EC2 in the AWS Management Console.
2.	Click Launch Instance.
3.	Select an AMI (Amazon Machine Image).
4.	Select an instance type and configure the network and subnet (choose the VPC and subnet you created earlier).
5.	Assign the previously created security group to your instance.
6.	Complete the remaining steps to launch the EC2 instance.
 
7.	Login to public EC2 using Aws console
 
8.	Ping google.com
 
So now public EC2 is working with outside the internet.
9. Login to private EC2 using Aws console but we are unable to login.
 


10.  So first we have to login to public EC2 instance and then add .pem file to public instance. 
11. Add ssh key to ssh agent .
                         eval `ssh-agent -s`
          ssh-add load.pem

 

12. Login to public instance and copy the load.pem file to instance and try to login to private instance.
 
13. I tried to ping google.com but is not working. So if it is working means we have to create a NAT gateway.
 


 
 
14. Once NAT gateway created go to private route table add the routes.



 
15. NAT gateway is attached again login to private EC2 through public EC2 instance.
 
SO now you are able to ping google.com and it is working because of NAT gateway.

