### Virtual private clouds (VPC)

A VPC is a virtual network that closely resembles a traditional network that you'd operate in your own data center. After you create a VPC, you can add subnets.

Benefits:
- Secure and monitor connections, screen traffic, and restrict instance access inside your virtual network.
- Spend less time setting up, managing, and validating your virtual network.
- Customize your virtual network by choosing your own IP address range, creating subnets, and configuring route tables.

Subnets
A subnet is a range of IP addresses in your VPC. A subnet must reside in a single Availability Zone. After you add subnets, you can deploy AWS resources in your VPC.

internet gateway
An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.

An internet gateway enables resources in your public subnets (such as EC2 instances) to connect to the internet if the resource has a public IPv4 address or an IPv6 address.

route table
A route table contains a set of rules, called routes, that determine where network traffic from your subnet or gateway is directed.

CIDR
When you create a VPC, you must specify an IPv4 CIDR block for the VPC. The allowed block size is between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses). After you've created your VPC, you can associate additional IPv4 CIDR blocks with the VPC.

Network access control list
A network access control list (ACL) allows or denies specific inbound or outbound traffic at the subnet level. You can use the default network ACL for your VPC, or you can create a custom network ACL for your VPC with rules that are similar to the rules for your security groups in order to add an additional layer of security to your VPC.

Nat gateway 
for a private subnet to access the internet 

useful resources: 

https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html

https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html

### create VPC and subnet

<img width="183" alt="image" src="https://user-images.githubusercontent.com/118978642/234258590-38928034-6973-42f0-93a6-20df120468d7.png">

steps:

1. create VPC 10.0.0.0/16
2. create internet gateway 2.1 -attach the IG to VPC
3. create a public subnet 10.0.1.0/24 - 3.1 connect subnet to VPC
4. create a route table - associate it with public subnet and attach to IG

1. step 1:

- go to aws
- type vpc in search box
- click create vpc
- click VPC only
- give it a name
- type in ip range: 10.0.0.0/16
- leave rest default
- click create vpc

2. step 2:
- click on IG
- click create ig
- give it a name
- create ig

to attach ig to vpc
- click on Action then connect to VPC
- then find your VPC
- then click on attach ig

3. step 3
- click on subnet
- click create subnet
- select your vpc
- chose a name
- no pref for AZ

cidr calculator: https://www.ipaddressguide.com/cidr

4. step 4: create route table 
- click on route tavle
- give name
- select VPC
- click create route table

assoicate subenet with route table
- click on subnet association
- click on edit subnet association
- select your subnet

- click routes
- click edit route
- add a route as per below
<img width="696" alt="image" src="https://user-images.githubusercontent.com/118978642/234263126-4bbe28d3-bb55-4a9f-85d5-3ac65a5a2fee.png">

- save association


### create app and db infrastructure

1. create AMI for app and db

- click on running instance
- click on action
- click on image
- insert details including name
- click create

do this for both the app and db instances

2. create instance from AMIs

- under AMIs on lefthandside
- click AMIs
- select the AMI for app
- click launch instance
- give it a name
- make sure the AMI used is the one you created
- select keypair
- under network setting, chose your VPC and select the public subnet
- enable auto-assign IP
- create a new SG make sure your security group has the following ports opened 
<img width="332" alt="image" src="https://user-images.githubusercontent.com/118978642/234279412-e4b47835-6319-44b8-8953-3466a6328ed5.png">
- launch instance
- ssh to your app, make sure you use ubuntu user
- cd to app folder - type ```npm start```

3. create private subnet for db instance
- on aws consolse search VPC
- click on subnets on lefthandside
- click create subnet 
- select your vpc
- chose cidr: 10.0.13.0/24 (public is 10.0.12.0/24)

create a route table for private subet
- click on route tables on lefthand side
- create route table
- give it a name
- associate it with VPC
- click create

no need to create any additional routes. need to associate route table with subnet
- click on subnet association tab 
- click on edit subnet association
- tick your private subnet
- click subnet assocation

4. create db instance

- type ec2 in search box on aws console
- click on AMIs (under images on lefthand side)
- click on launch instance from AMIs
- give it a name
- make sure AMI is the db one created
- select key pair
- under network setting click edit then chose VPC and private subnet and create security group with the following ports:
<img width="329" alt="image" src="https://user-images.githubusercontent.com/118978642/234285858-316dad58-10a5-4001-b66a-bd0f864c8316.png">
- enable auto-asign IP
- click on launch instace
- you wont be able to ssh into private instance

5. connect app to db

go to the app instance and cd to home 

```sudo nano .bashrc```

update for the new db ip address (use the private address): ```export DB_HOST=mongodb://<ip_address_db>:27017/posts```
- ```source .bashrc```
- ```printenv DB_HOST```

cd to app folder
- ```npm start```
- ```node app.js```
your app should run on webbrowser: appip:3000/posts

### avaialability, fault tolerant, scalable 

Elastic Load Balancing automatically distributes your incoming application traffic across all the EC2 instances that you are running. Elastic Load Balancing helps to manage incoming requests by optimally routing traffic so that no one instance is overwhelmed.

AWS Auto Scaling monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost


