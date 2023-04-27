### Disaster recovery

Disaster recovery is the process by which an organization anticipates and addresses technology-related disasters. IT systems in any company can go down unexpectedly due to unforeseen circumstances, such as power outages, natural events, or security issues. Disaster recovery includes a company's procedures and policies to recover quickly from such events.

#### The benefits of having a disaster recovery plan

- Ensures business continuity
- Enhances system security
- Improves customer retention
- Reduces recovery costs

Resources: https://aws.amazon.com/what-is/disaster-recovery/

#### what is S3

Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can use Amazon S3 to store and protect any amount of data for a range of use cases, such as data lakes, websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics.

Find out more here: https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html 

#### AWS CLI and SDK

The AWS CLI is an open source tool built using the AWS SDK for Python (Boto3) that provides commands to interact with AWS services. With minimal configuration, you can use all the features provided by the AWS management console from your favorite terminal.

AWS SDKs provide an API for different programming languages (Python, Java, JavaScript, C++, .NET, GO, PHP, Ruby,…) to programmatically build and use AWS services.

Read more here: https://scalastic.io/en/aws-cli-sdk/#:~:text=An%20AWS%20SDK%20(Software%20Development,SDK%20(also%20called%20Boto3).

<img width="389" alt="image" src="https://user-images.githubusercontent.com/118978642/234826564-9e2341c7-b538-4b33-b3c3-10af98bec1ed.png">

#### aws cli

To set up the aws cli, you need to take the following steps:

1.  Create a new instance with port 22 open and AMI: Canonical, Ubuntu, 18.04 LTS, amd64 bionic image build on 2022-06-10

2.  Open your terminal/bash window.

3.  Change directory into .ssh folder. `cd .ssh`

4.  Paste ssh key from AWS into terminal to ssh into instance.

5.  Run the following command to update your instance. `sudo apt update -y`

6.  Run the following command to upgrade your instance. `sudo apt upgrade -y`

7.  Check python version `python3 --version`. I needed 3.6 or above.

8.  Install python on your instance. `sudo apt install python`

9.  Install pip on your instance. `sudo apt install python3-pip`

10. Tell ubuntu which version of python to use `alias python=3`[ when u do python --version after, should show the python3 version instead of the other one]

11. Install AWS CLI onto your instance. `sudo python3 -m pip install awscli`

12. Run the command `aws configure` to enter specific details.

13. Copy and paste your Access Key ID which can be found in your file in your .ssh folder.

14. Copy and paste your Secret Access Key which can be found in your file in your .ssh folder.

15. Enter your default region. I used `eu-west-1`

16. Enter your output format. I used `json`

17. Validate your connection to your S3 bucket by testing to see if you can see the objects inside. `aws s3 ls`. If prompted with error, follow from step 12 to configure again.



