Virtual private clouds (VPC)
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

<img width="183" alt="image" src="https://user-images.githubusercontent.com/118978642/234258590-38928034-6973-42f0-93a6-20df120468d7.png">

steps:

1. create VPC 10.0.0.0/16
2. create internet gateway 2.1 -attach the IG to VPC
3. create a public subnet 10.0.1.0/24 - 3.1 connect subnet to VPC
4. create a route table - associate it with public subnet and attach to IG

step 1:

go to aws
type vpc in search box
click create vpc
click VPC only
give it a name
type in ip range: 10.0.0.0/16
leave rest default
click create vpc

step 2:
click on IG
click create ig
give it a name
create ig

to attach ig to vpc
click on Action then connect to VPC
then find your VPC
then click on attach ig

step 3
click on subnet
click create subnet
select your vpc
chose a name
no pref for AZ

cidr calculator: https://www.ipaddressguide.com/cidr

step 4: create route table 
click on route tavle
give name
select VPC
click create route table

assoicate subenet with route table
click on subnet association
click on edit subnet association
select your subnet

click routes
click edit route
add a route as per below
<img width="696" alt="image" src="https://user-images.githubusercontent.com/118978642/234263126-4bbe28d3-bb55-4a9f-85d5-3ac65a5a2fee.png">

save association
