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
<img width="517" alt="image" src="https://user-images.githubusercontent.com/118978642/234579731-cf22fdb5-9997-476e-8878-393d18574808.png">


#### general definitions

scalability mans that an application/system can handle grater loads by adapting - so making the hardware stronger (sclae up) or adding nodes (scale out). There are two types: vertical (increase size of instance) and horizontal (increase the number of instances) scaling

highly available means that you're running you application/systems in at least two AZs. It goes hand in hand with horizontal scaling. so if one AZs is down then you still have the other one. 

elasticity means that once a system is scalable, eleasiticy means there will be some autoscaling so that the system can scale based on the laods.

load balancers are servers that will forward internet traffic to multiple serverse ( instances) downstream. Elastic Load Balancing automatically distributes your incoming application traffic across all the EC2 instances that you are running. Elastic Load Balancing helps to manage incoming requests by optimally routing traffic so that no one instance is overwhelmed.

AWS Auto Scaling monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost.
- scales in or out depending on capacity
- ensures we have amin and max number of instances
- automatically registers new instances to laod balancer
- replaces unhaelthy instances 

#### points to consider when setting up

will use 3 instances across 3 AZs. Need a policy which tells us when to use a different ec2 - e.g. when CPU reaches a certain level. We decide min instanecs and how many max and desired capacity.

Each instance needs ports 80 and 3000. Need a load balancer that allows aacess at ports 80 and 3000. We need a target group which allows access by port 80 and 3000.

min and desired capaicty = 2, so infrastructure is fault tolerant

scale  out - create more instances of the same size, scale up - build a bigger ec2 server (e.g bigger cpu instance)

 To achieve this:
 - need a launch template that can be replicated in multiple AZs with required information
 - ASG policy - target tracking policy (we want to track the target e.g. 50% or above CPU utilisation - if ec2 instance cpu hits 50% then ASG will spin up another instance)
 - target group required ports access
 - load balancer - application load balancer (it works at layer 7 of networking which is for HTTP or HTTPs only traffic) which will need multi AZs information

implementation

1. create launch template

- go to aws console
- type EC2 in search box
- on left click on launch template
- click create and launch template
- give template name: mutiat_tech221_autoscaling_app
- use same details in description
- tick box to provide guidedance tto ASG
<img width="454" alt="image" src="https://user-images.githubusercontent.com/118978642/234584171-6b111764-4fdb-4eb9-a5ee-2dee6073a675.png">
- chose ami to be 18.04 (18.04 LTS, amd64 bionic image build on 2022-09-01)
- instance typ: t2.micro
- select a key pair
- select existing security group: http, ssh, 3000
- in advanced details include the following in the userdata 
```#!/bin/bash
sudo apt update -y 
sudo apt upgrade -y
sudo apt install nginx -y
keep everything default
```
- click create launch template

<img width="578" alt="image" src="https://user-images.githubusercontent.com/118978642/234587571-594964c7-68e3-4ab9-b14f-447a8172a232.png">

2. ASG
- click on ASG on left
- click launch asg
- give name
- select your launch template
- click next
- keep default VPC
- AZ: chose 3 AZs
<img width="357" alt="image" src="https://user-images.githubusercontent.com/118978642/234589075-799b8cc7-d4bb-4846-805c-13d5bd759b57.png">
-click next
- under load balancer, click no attach a new load balancer
- click application load balancer
- under load balancer scheme
<img width="338" alt="image" src="https://user-images.githubusercontent.com/118978642/234592450-bbee280c-ba93-427a-bb35-f50fe9286218.png">
<img width="351" alt="image" src="https://user-images.githubusercontent.com/118978642/234592781-a74ac96c-95fe-4cc4-9a8e-0c83f49fc81e.png">
Health check should be populated like follow
<img width="351" alt="image" src="https://user-images.githubusercontent.com/118978642/234593029-52f0c429-3e05-44b5-bb4b-ed4fcc004dcf.png">
leave the rest as default and click next
<img width="351" alt="image" src="https://user-images.githubusercontent.com/118978642/234593433-750bd335-e0b1-4cbb-8160-9d80c0b367cc.png">
want to monitor CPU
<img width="351" alt="image" src="https://user-images.githubusercontent.com/118978642/234593749-faff4f4a-71b6-4511-9e13-ae65a8d5b91f.png">
rest deault - click on next
click next again
need to add tags - key: mutiat_tech221_asg_alb_app
review it then click create an asg

click instances on left hand side and check that you have 2 instances running which should be in two different AZs
