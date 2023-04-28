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

here is the doc: https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/s3.html


## monitoring

The 4 golden rules of monitoring: latency, traffic, errors, and saturation
1.latency: The time it takes to service a request. It’s important to distinguish between the latency of successful requests and the latency of failed requests.
2. traffic: A measure of how much demand is being placed on your system, measured in a high-level system-specific metric. 
3. errors: The rate of requests that fail, either explicitly (e.g., HTTP 500s), implicitly (for example, an HTTP 200 success response, but coupled with the wrong content), or by policy (for example, "If you committed to one-second response times, any request over one second is an error"). 
4. Saturation: How "full" your service is. A measure of your system fraction, emphasizing the resources that are most constrained (e.g., in a memory-constrained system, show memory; in an I/O-constrained system, show I/O)

https://sre.google/sre-book/monitoring-distributed-systems/#:~:text=The%20four%20golden%20signals%20of,system%2C%20focus%20on%20these%20four.&text=The%20time%20it%20takes%20to,the%20latency%20of%20failed%20requests.

<img width="423" alt="image" src="https://user-images.githubusercontent.com/118978642/235122143-ba288f39-5297-4344-b9c3-ab8c166b9f49.png">

A look at some of the services you can monitor on cloudwatch

<img width="310" alt="image" src="https://user-images.githubusercontent.com/118978642/235123984-c1eb1011-df09-4744-bc53-1c65c143d2f9.png">

To set up a notification once an alarm is triggured 

<img width="460" alt="image" src="https://user-images.githubusercontent.com/118978642/235125156-6f13ad3a-54c0-475d-801c-ad299e710b81.png">


1. login to aws management console
2. search ec2 instance 
3. once instance are running, click on the monitoring tab.
4. aws monitors somethings by default - this is not detailed so we can create custom monitoring 
5. click on detailed monitoring and then confirm (note there will be additional charges for detailed monitoring)


Detailed steps to create an alarm which monitors CPU utilisation of ec2 and sends an email (via sns) once cpu >50:
Open the CloudWatch console at https://console.aws.amazon.com/cloudwatch/.

1. In the navigation pane, choose Alarms, All Alarms.

2. Choose Create alarm.

3. Choose Select metric.

4. In the All metrics tab, choose EC2 metrics.

5. Choose a metric category (for example, Per-Instance Metrics).

6. Find the row with the instance that you want listed in the InstanceId column and CPUUtilization in the Metric Name column. Select the check box next to this row, and choose Select metric.

7. Under Specify metric and conditions, for Statistic choose Average, choose one of the predefined percentiles, or specify a custom percentile (for example, p95.45).

8. Choose a period (for example, 5 minutes).

9. Under Conditions, specify the following:

10. For Threshold type, choose Static.

11. For Whenever CPUUtilization is, specify Greater. Under than..., specify the threshold that is to trigger the alarm to go to ALARM state if the CPU utilization exceeds this percentage. For example, 70.

12. Choose Additional configuration. For Datapoints to alarm, specify how many evaluation periods (data points) must be in the ALARM state to trigger the alarm. If the two values here match, you create an alarm that goes to ALARM state if that many consecutive periods are breaching.

13. To create an M out of N alarm, specify a lower number for the first value than you specify for the second value. For more information, see Evaluating an alarm.

14. For Missing data treatment, choose how to have the alarm behave when some data points are missing. For more information, see Configuring how CloudWatch alarms treat missing data.

15. If the alarm uses a percentile as the monitored statistic, a Percentiles with low samples box appears. Use it to choose whether to evaluate or ignore cases with low sample rates. If you choose ignore (maintain alarm state), the current alarm state is always maintained when the sample size is too low. For more information, see Percentile-based CloudWatch alarms and low data samples.

16. Choose Next.

17. Under Notification, choose In alarm and select an SNS topic to notify when the alarm is in ALARM state

18. To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose Add notification.

19. To have the alarm not send notifications, choose Remove.

20. When finished, choose Next.

21. Enter a name and description for the alarm. The name must contain only UTF-8 characters, and can't contain ASCII control characters. Then choose Next.

22. Under Preview and create, confirm that the information and conditions are what you want, then choose Create alarm

23. SSH into your instance, create a script.py file with a inifite while loop, then execute the loop to trigger the alarm.

(https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdEC2.html)

Here is how to create a dashboard on cloudwatch: https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create_dashboard.html
