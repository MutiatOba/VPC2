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

<img width="375" alt="image" src="https://user-images.githubusercontent.com/118978642/234863704-26e8c517-2f65-42e1-af01-73cd7db7c4cc.png">

want to:
create bucket
upload/download data
delete

to create a bucket: 
- aws s3 mb s3://mutiat-tech221 (note naming convention prevents using symbols like _)
- aws s3 ls (this allows you to view a list of buckets)
- sudo nano test.txt (creates a file to be uploaded to s3 bucket)
- aws s3 cp test.txt s3://mutiat-tech221 (copy file to your s3 bucket)
- sudo rm test.txt (removes the file on server, but file should be avaiable in s3 bucket)
- aws s3 cp s3://mutiat-tech221/test.txt /home/ubuntu (to download a file from s3 to ec2)
- aws s3 rm s3://mutiat-tech221/text.txt (to remove the objects in the bucket)
- aws s3 rb s3://mutiat-tech221 (to delete the bucket - need to delete the content before using this)

s3 permissions [NEED TO RESEARCH]
s3 storage class: https://aws.amazon.com/s3/storage-classes/

#### using the cli to create s3 bucket, upload & download files and delete files & buckets

Assume you already have the following:
1. aws cli installed
2. python3 installed
3. signed into aws via cli

Steps:
1. install boto3: pip3 install boto3

2. create the script for creating bucket: sudo nano script.py
```
import boto3

s3_client = boto3.client('s3')

bucket_name = 'mutiat-tech221'

response = s3_client.create_bucket(
    ACL='private',
    Bucket=bucket_name,
    CreateBucketConfiguration={
        'LocationConstraint': 'eu-west-1'
    }
)

print(bucket_name)
print(response)
```

make the script executable: sudo chmod +x script.py
excute the script: python script.py

3. create script for uploading file to s3 bucket
```
import boto3

s3 = boto3.client('s3')
bucket_name ='mutiat-tech221'
response2= s3.upload_file('/home/ubuntu/tests3.txt', bucket_name, 'tests3.txt')

print(response2)
```
make the script executable: sudo chmod +x script.py
excute the script: python script.py
