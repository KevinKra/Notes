## EFS

### Questions

- Describe the key differences between EBS, EFS, and S3.
- Describe block storage, file storage, and object storage.
- Does EFS automatically scale up and down with your infrastructure?
- Is it possible to share an **EBS instance** with multiple EC2 instances?
- Can you connect EC2s in one region to an EFS file system in another region?
- What type of storage is EBS, what type of file system is EFS?
- How can you back up your EFS?
- Can EFS be used on Windows machines?

#### Notes

- EBS is block storage, with managed encryption, and can only be attached to one EC2 instance. EFS is file storage and shared amongst multiple EC2 instances with automatic scaling. S3 is object storage, it's scalable and can be accessed by multiple EC2 instances and other cloud services.

- Block storage means all data is stored in equal sized blocks, this systems allows for some performance advantages over traditional storage. File storage is the common directory-based storage found on computers, it follows a hierarchical pattern but doesn't provide the increased potential for complex queries. Object storage is similar to file storage, however the files are stored in the same _flat_ plane with comprehensive metadata to make it more accessible. The flatness of object storage plus the extensive metadata (provided by S3), makes it easier to run complex queries against the storage system.

- An Amazon EFS file system provides a standard file system interface allowing you to seamlessly integrate Amazon EFS with your existing applications and tools. EFS can be used with AWS services like EC2, ECS, and others.

- Multiple EC2 instances can access an Amazon EFS file system at the same time, this allows EFS to provide a common data source for workloads and applications running on more than one EC2 instance.

- Amazon Elastic File System (Amazon EFS) automatically grows and shrinks as you add and remove files with no need for management or provisioning.

- With support for inter-region VPC peering, you can connect EC2 instances in one region to EFS file systems in another.

- It is not possible to share a single EBS volume between multiple EC2 instances

- EFS is highly-available storage that can span multiple availability zones.

- You can schedule automatic incremental backups of your EFS file system using the EFS-to-EFS Backup solution.

- Amazon EFS is not supported on Windows instances, **it is only useable for Linux-based systems.**

---

## CloudWatch Events

### Questions

- What are CloudWatch Events, Rules, and Targets?
- Can a single Rule route to multiple targets?
- Does a target have to be in the same region as a rule?
- What is Amazon EventBridge, does it use the same underlying infrastructure as CloudWatch Events?
- What are some key differences between CW-Alarms and CW-Events?

#### Notes

> An application is hosted in an AWS Fargate cluster that runs a batch job whenever an object is loaded on an Amazon S3 bucket. The minimum number of ECS Tasks is initially set to 1 to save on costs, and it will only increase the task count based on the new objects uploaded on the S3 bucket. Once processing is done, the bucket becomes empty and the ECS Task count should be back to 1.

_Set up a CloudWatch Event rule to detect S3 object PUT operations and set the target to the ECS cluster with the increased number of tasks. Create another rule to detect S3 DELETE operations and set the target to the ECS Cluster with 1 as the Task count._

- **Events** – An event indicates a change in your AWS environment.

- **Rules** – A rule matches incoming events and routes them to targets for processing. **A single rule can route to multiple targets**, all of which are processed in parallel.

- **Targets** – A target processes events, targets can include EC2, Lambda, Kinesis, ECS, etc. **A rule's targets must be in the same Region as the rule.**

- CloudWatch Alarms deliver near real-time stream of system events that describe changes in AWS resources. Events respond to these operational changes and take corrective action as necessary, by sending messages to respond to the environment, activating functions, making changes, and capturing state information.

- Amazon EventBridge is a service that builds upon the CloudWatch Events service API. Both Amazon EventBridge and CloudWatch Events use the same underlying infrastructure. You can still continue managing events through CloudWatch Events but the preferred way is to manage events via Amazon EventBridge.

- CW-Alarms **watch a single metric** and respond to changes for that metric. CW-Events response to **actions**, such as a lambda being created or a change in the AWS environment.

---

## ECS Cluster

### Questions

- With ECS, what two locations can you store your sensitive data for injection into your containers?
- Docker Secrets is only usable for what service?
- What is the AWS recommended way to secure sensitive data for containers?
- Should you store sensitive credentials in S3? What is a better alternative?

#### Notes

> prompt: A media company has an Amazon ECS Cluster, which uses the Fargate launch type, to host its news website. The database credentials should be supplied using environment variables, to comply with strict security compliance. As the Solutions Architect, you have to ensure that the credentials are secure and that they cannot be viewed in plaintext on the cluster itself.

_Use the AWS Systems Manager Parameter Store to keep the database credentials and then encrypt them using AWS KMS. Create an IAM Role for your Amazon ECS task execution role (**taskRoleArn**) and reference it with your task definition, which allows access to both KMS and the Parameter Store. Within your container definition, specify secrets with the name of the environment variable to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container._

- Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in either **AWS Secrets Manager** secrets or **AWS Systems Manager Parameter Store** parameters and then referencing them in your container definition.

- Although you can use Docker Secrets to secure the sensitive database credentials, this feature is only applicable in Docker Swarm. In AWS, the recommended way to secure sensitive data is either through the use of **Secrets Manager or Systems Manager Parameter Store.**

- It is not recommended to store sensitive credentials in S3. This entails a lot of overhead and manual configuration steps which can be simplified by simply using the **Secrets Manager** or **Systems Manager Parameter Store**.

---

## Amazon FSx for Windows

### Questions

- What is FSx for Windows?
- What does EFS run as its file sharing protocol?
- What does FSx run as its file sharing protocol?
- What does NFS stand for?
- What does SMB stand for?
- What Operating systems can FSx run on?
- What Operating systems can EFS run on?

#### Notes

- Amazon FSx for Windows File Server provides fully managed Microsoft Windows file servers. Amazon FSx for Windows File Server has the features, performance, and compatibility to easily lift and shift enterprise applications to the AWS Cloud.

- Amazon EFS is a managed NAS filer for EC2 instances based on **Network File System (NFS)** version 4.

- FSx for Windows, on the other hand, is a managed Windows Server that runs **Windows Server Message Block (SMB)**-based file services.

- NFS is one of the first network file sharing protocols native to Unix and Linux.

- Some Windows applications might not work on EFS or be feature-complete without access to a native Windows SMB file share.

- FSx It is accessible from Windows, Linux, and macOS compute instances and devices that support the SMB protocol.

- Thousands of compute instances and devices can access a file system concurrently.

- **Amazon EFS only supports Linux workloads.**

---

## Lambda + RDS

### Questions

- What type of events can RDS detect?
- Native functions and stored procedures can be used to capture data-modifying events?
- When working with RDSs' what kind of procedure, or event, is capable of triggering a lambda function?

#### Notes

> A car dealership website hosted in Amazon EC2 stores car listings in an Amazon Aurora database managed by Amazon RDS. Once a vehicle has been sold, its data must be removed from the current listings and forwarded to a distributed processing system.

_Create a native function or a stored procedure that invokes a Lambda function. Configure the Lambda function to send event notifications to an Amazon SQS queue for the processing system to consume._

- You can invoke an AWS Lambda function from an Amazon Aurora database with a native function or a stored procedure. This approach can be useful when you want to integrate your database running on Aurora MySQL with other AWS services.

- You can trigger a Lambda function whenever a listing is deleted from the database. You can then write the logic of the function to send the listing data to an SQS queue and have different processes consume it.

- **RDS events only provide operational events** such as DB instance events, DB parameter group events, DB security group events, and DB snapshot events. **What we need in the scenario is to capture data-modifying events (INSERT, DELETE, UPDATE) which can be achieved thru native functions or stored procedures.**

---

## CloudFront + Lambda@Edge

### Questions

- What service is Lambda@Edge like?
- Can Lambda@Edge be used to execute auth processes closer to users?
- What can you use, through CloudFront, to alleviate the 504 errors users are experiencing?

#### Notes

> A popular social media website uses a CloudFront web distribution to serve their static contents to their millions of users around the globe. They are receiving a number of complaints recently that their users take a lot of time to log into their website. There are also occasions when their users are getting HTTP 504 errors. You are instructed by your manager to significantly reduce the user's login time to further optimize the system.

- Lambda@Edge lets you run Lambda functions to customize the content that CloudFront delivers, executing the functions in AWS locations closer to the viewer.

- In the given scenario, you can use Lambda@Edge to allow your Lambda functions to customize the content that CloudFront delivers and to execute the authentication process in AWS locations closer to the users.

- **You can set up an origin failover by creating an origin group with two origins with one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin fails.** This will alleviate the occasional HTTP 504 errors that users are experiencing.

---

## SNS + SQS

### Questions

- What are SQS and SNS?
- What is Amazon MQ recommended for?
- What 3 destinations can S3 publish events to?
- Can S3 send out notifications if certain events happen in your bucket?
- Describe the _fanout scenario_.
- How many destinations can an S3 event notification be sent out to? How can this be alleviated with the SNS fanout solution?

#### Notes

> A company is using Amazon S3 to store frequently accessed data. When an object is created or deleted, the S3 bucket will send an event notification to the Amazon SQS queue. A solutions architect needs to create a solution that will notify the development and operations team about the created or deleted objects.

_Create an Amazon SNS topic and configure two Amazon SQS queues to subscribe to the topic. Grant Amazon S3 permission to send notifications to Amazon SNS and update the bucket to use the new SNS topic._

- **If you are building brand new applications in the cloud, then it is highly recommended that you consider Amazon SQS and Amazon SNS.** Amazon SQS and SNS are lightweight, fully managed message queue and topic services that scale almost infinitely and provide simple, easy-to-use APIs. You can use Amazon SQS and SNS to decouple and scale microservices, distributed systems, and serverless applications, and improve reliability.

- **If you're using messaging with existing applications and want to move your messaging service to the cloud quickly and easily, it is recommended that you consider Amazon MQ.**

- Amazon SWF is a fully-managed state tracker and task coordinator service and not a messaging service, unlike Amazon MQ, AmazonSQS, and Amazon SNS.

- Create an Amazon SNS topic and configure two Amazon SQS queues to subscribe to the topic. Grant Amazon S3 permission to send notifications to Amazon SNS and update the bucket to use the new SNS topic.

- **The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket.** To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications.

- You store the above configuration in the notification subresource that is associated with a bucket.

**Amazon S3 supports the following destinations where it can publish events:**

- Amazon Simple Notification Service (Amazon SNS) topic
- Amazon Simple Queue Service (Amazon SQS) queue
- AWS Lambda

- In Amazon SNS, the **fanout scenario** is when a message published to an SNS topic is replicated and pushed to multiple endpoints, such as Amazon SQS queues, HTTP(S) endpoints, and Lambda functions. This allows for parallel asynchronous processing.

- By using the message fanout pattern, you can create a topic and use two Amazon SQS queues to subscribe to the topic. If Amazon SNS receives an event notification, it will publish the message to both subscribers.

- Take note that Amazon S3 event notifications are designed to be delivered at least once and to one destination only. **You cannot attach two or more SNS topics or SQS queues for S3 event notification.** Therefore, you must send the event notification to Amazon SNS.

- For example, you can develop an application that publishes a message to an SNS topic whenever an order is placed for a product. Then, SQS queues that are subscribed to the SNS topic receive identical notifications for the new order. An Amazon Elastic Compute Cloud (Amazon EC2) server instance attached to one of the SQS queues can handle the processing or fulfillment of the order. And you can attach another Amazon EC2 server instance to a data warehouse for analysis of all orders received.

- You can only add 1 SQS or SNS at a time for Amazon S3 events notification.

- If you need to send the events to multiple subscribers, you should implement a message fanout pattern with Amazon SNS and Amazon SQS.

- The FIFO capabilities of each of these services (SQS | SNS) work together to act as a fully managed service to integrate distributed applications that require data consistency in near-real-time.

---

### AWS Directory Service

### Questions

- Not really much to ask about this. Just read the bullet points.

#### Notes

> A telecommunications company is planning to give AWS Console access to developers. Company policy mandates the use of identity federation and role-based access control. Currently, the roles are already assigned using groups in the corporate Active Directory.

_Considering that the company is using a corporate Active Directory, it is best to use AWS Directory Service AD Connector for easier integration. In addition, since the roles are already assigned using groups in the corporate Active Directory, it would be better to also use IAM Roles. Take note that you can assign an IAM Role to the users or groups from your Active Directory once it is integrated with your VPC via the AWS Directory Service AD Connector._

- AWS Directory Service provides multiple ways to use Amazon Cloud Directory and Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources.

- AWS Directory Service provides multiple directory choices for customers who want to use existing Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications in the cloud. It also offers those same choices to developers who need a directory to manage users, groups, devices, and access.

---

## DynamoDB

### Questions

- What is a DynamoDB stream?
- When are events on a table captured, and what does the stream record capture?
- Can you configure the stream record to capture more information than just the primary key attribute(s) of the modified record?
- Describe the concept of "triggers".
- Is the DynamoDB Stream feature enabled by default?

#### Notes

> A popular social network is hosted in AWS and is using a DynamoDB table as its database. There is a requirement to implement a 'follow' feature where users can subscribe to certain updates made by a particular user and be notified via email. Which of the following is the most suitable solution that you should implement to meet the requirement?

_Enable DynamoDB Stream and create an AWS Lambda trigger, as well as the IAM role which contains all of the permissions that the Lambda function will need at runtime. The data from the stream record will be processed by the Lambda function which will then publish a message to SNS Topic that will notify the subscribers via email._

- A DynamoDB stream is an ordered flow of information about changes to items in an Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.

- Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attribute(s) of the items that were modified.

- A stream record contains information about a data modification to a single item in a DynamoDB table. You can configure the stream so that the stream records capture additional information, such as the "before" and "after" images of modified items.

- Amazon DynamoDB is integrated with AWS Lambda so that you can create _triggers_ —pieces of code that automatically respond to events in DynamoDB Streams. With triggers, you can build applications that react to data modifications in DynamoDB tables.

- If you enable DynamoDB Streams on a table, you can associate the stream ARN with a Lambda function that you write.

- Immediately after an item in the table is modified, a new record appears in the table's stream. AWS Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records. The Lambda function can perform any actions you specify, such as sending a notification or initiating a workflow.

- **the DynamoDB Stream feature is not enabled by default.**

---

## Kinesis

- Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information.

- With Amazon Kinesis, you can ingest real-time data such as video, audio, application logs, website clickstreams, and IoT telemetry data for machine learning, analytics, and other applications.

- Amazon Kinesis enables you to process and analyze data as it arrives and respond instantly instead of having to wait until all your data is collected before the processing can begin.

- Amazon Kinesis enables you to ingest, buffer, and process streaming data in real-time, so you can derive insights in seconds or minutes instead of hours or days.

- Amazon Kinesis is fully managed and runs your streaming applications without requiring you to manage any infrastructure.

- Amazon Kinesis can handle any amount of streaming data and process data from hundreds of thousands of sources with very low latencies.

### Questions

- What is Kinesis?
- What types of data can you inject in real-time with Kinesis?
- Does Kinesis need to wait for all data to be collected before responding?
- Is Kinesis fully managed?

---

## AWS Organizations + AWS Resource Access Manager (RAM)

> A global IT company with offices around the world has multiple AWS accounts. To improve efficiency and drive costs down, the Chief Information Officer (CIO) wants to set up a solution that centrally manages their AWS resources. This will allow them to procure AWS resources centrally and share resources such as AWS Transit Gateways, AWS License Manager configurations, or Amazon Route 53 Resolver rules across their various accounts.

AWS Resource Access Manager (RAM) is a service that enables you to easily and securely share AWS resources with any AWS account or within your AWS Organization. You can share AWS Transit Gateways, Subnets, AWS License Manager configurations, and Amazon Route 53 Resolver rules resources with RAM.

Many organizations use multiple accounts to create administrative or billing isolation, and limit the impact of errors. RAM eliminates the need to create duplicate resources in multiple accounts, reducing the operational overhead of managing those resources in every single account you own. You can create resources centrally in a multi-account environment, and use RAM to share those resources across accounts in three simple steps: create a Resource Share, specify resources, and specify accounts. RAM is available to you at no additional charge.

You can procure AWS resources centrally, and use RAM to share resources such as subnets or License Manager configurations with other accounts. This eliminates the need to provision duplicate resources in every account in a multi-account environment, reducing the operational overhead of managing those resources in every account.

AWS Organizations is an account management service that lets you consolidate multiple AWS accounts into an organization that you create and centrally manage. With Organizations, you can create member accounts and invite existing accounts to join your organization. You can organize those accounts into groups and attach policy-based controls.

Hence, the correct combination of options in this scenario is:

- Consolidate all of the company's accounts using AWS Organizations.

- Use the AWS Resource Access Manager (RAM) service to easily and securely share your resources with your AWS accounts.

The option that says: Use the AWS Identity and Access Management service to set up cross-account access that will easily and securely share your resources with your AWS accounts is incorrect because although you can delegate access to resources that are in different AWS accounts using IAM, this process is extremely tedious and entails a lot of operational overhead since you have to manually set up cross-account access to each and every AWS account of the company. A better solution is to use AWS Resources Access Manager instead.

The option that says: Use AWS Control Tower to easily and securely share your resources with your AWS accounts is incorrect because AWS Control Tower simply offers the easiest way to set up and govern a new, secure, multi-account AWS environment. This is not the most suitable service to use to securely share your resources across AWS accounts or within your Organization. You have to use AWS Resources Access Manager (RAM) instead.

The option that says: Consolidate all of the company's accounts using AWS ParallelCluster is incorrect because AWS ParallelCluster is simply an AWS-supported open-source cluster management tool that makes it easy for you to deploy and manage High-Performance Computing (HPC) clusters on AWS. In this particular scenario, it is more appropriate to use AWS Organizations to consolidate all of your AWS accounts.

---

## Amazon Macie

> A government entity is conducting a population and housing census in the city. Each household information uploaded on their online portal is stored in encrypted files in Amazon S3. The government assigned its Solutions Architect to set compliance policies that verify sensitive data in a manner that meets their compliance standards. They should also be alerted if there are compromised files detected containing personally identifiable information (PII), protected health information (PHI) or intellectual properties (IP).

Amazon Macie is an ML-powered security service that helps you prevent data loss by automatically discovering, classifying, and protecting sensitive data stored in Amazon S3. Amazon Macie uses machine learning to recognize sensitive data such as personally identifiable information (PII) or intellectual property, assigns a business value, and provides visibility into where this data is stored and how it is being used in your organization.

Amazon Macie continuously monitors data access activity for anomalies, and delivers alerts when it detects risk of unauthorized access or inadvertent data leaks. Amazon Macie has ability to detect global access permissions inadvertently being set on sensitive data, detect uploading of API keys inside source code, and verify sensitive customer data is being stored and accessed in a manner that meets their compliance standards.

Hence, the correct answer is: Set up and configure Amazon Macie to monitor and detect usage patterns on their Amazon S3 data.

The option that says: Set up and configure Amazon Rekognition to monitor and recognize patterns on their Amazon S3 data is incorrect because Rekognition is simply a service that can identify the objects, people, text, scenes, and activities, as well as detect any inappropriate content on your images or videos.

The option that says: Set up and configure Amazon GuardDuty to monitor malicious activity on their Amazon S3 data is incorrect because GuardDuty is just a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads.

The option that says: Set up and configure Amazon Inspector to send out alert notifications whenever a security violation is detected on their Amazon S3 data is incorrect because Inspector is basically an automated security assessment service that helps improve the security and compliance of applications deployed on AWS.

---

AWS DataSync, Redshift, SWF, AWS Glue, EMR, GuardDuty, MQ, Kinesis, AppStream, SnowMobile, Snowball Edge, Route 53, A-N-C Gateways, STS

## AWS DataSync

### Questions

- What two ways can DataSync move data traffic between services?
- Is DataSync suitable for hybrid on-prem/cloud architectures? What's a better alternative?
- What is used to transfer data from your on-prem environment to AWS?

#### Notes

- AWS DataSync is an **online data transfer service**. It is primarily used to move data from on-prem solutions to the cloud.

- **If you want hybrid on-prem / AWS storage capabilities use AWS Storage gateway instead of DataSync.**

- AWS DataSync simplifies, automates, and accelerates the process of copying large amounts of data to and from on-prem storage systems and/or AWS storage services over the Internet or over AWS Direct Connect.

- Allows you to **copy large amounts of data over the internet, or via direct connect,** to and from AWS storage services.

- Can copy data between shared file servers, self-managed object storage, AWS SnowCone, S3, EFS, and FSx for Windows.

- Transfers your data from your on-prem data center to AWS through the use of a **DataSync Agent.** An Agent is a virtual machine used to read data from or write data to an on-premises location.

---

## AWS SES

### Questions

- What is SES what does it provide?
- How is SES different than SNS?
- Is SES regional or global?

#### Notes

- **SES provides a bulk and transactional email-sending service.** Amazon SES eliminates the complexity and expense of building an in-house email solution or licensing, installing, and operating a third-party email service. The service integrates with other AWS services, making it easy to send emails from applications being hosted on services such as Amazon EC2.

- SNS however is a fully managed push messaging service. **Amazon Simple Notification Service makes it simple and cost-effective to push to mobile devices such as iPhone, iPad, Android, Kindle Fire, and internet connected smart devices, as well as pushing to other distributed services.** Besides pushing cloud notifications directly to mobile devices, **SNS can also deliver notifications by SMS text message or email, to Simple Queue Service (SQS) queues, or to any HTTP endpoint.**

- SES is a regional service.

- Common use cases: transactional emails and marketing emails.

---

## AWS SWF

### Questions

- What is SWF?

- Does SWF organize and process sequential events, what is a more popular alternative to SWF?

#### Notes

- SWF helps developers build, run, and scale background jobs that have parallel or sequential steps. You can think of Amazon SWF as a fully-managed state tracker and task coordinator in the Cloud. If your app’s steps take more than 500 milliseconds to complete, you need to track the state of processing, and you need to recover or retry if a task fails, Amazon SWF can help you.

- SWF is used to help organize and process a sequential order of events. It can be used to track and manage the workflow of your processes.

- SWF is awkward to work with, step-functions are the more modern approach to setting workflow steps in AWS.

---

## Redshift

### Questions

- What is AWS Redshift?
- What querying language does Redshift use?
- How many concurrent users can interface with Redshift?
- Does Redshift automatically back up your data?

#### Notes

- A fully managed, **petabyte-scale data warehouse service.** Redshift extends data warehouse queries to your data lake. You can run analytic queries against petabytes of data stored locally in Redshift, and directly against exabytes of data stored in S3.

- Allows you to **analyze all your data using standard SQL** or through your existing business intelligence tools.

- Offers concurrency scaling feature that supports unlimited concurrent users and concurrent queries.

- Redshift automatically and continuously backs up your data to S3. It can asynchronously replicate your snapshots to S3 in another region for disaster recovery.

---

## AWS Glue

### Questions

- What does AWS Glue do?

#### Notes

- AWS Glue is a fully managed ETL (extract, transform, and load) service that makes it simple and cost-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores and data streams. AWS Glue is a serverless data integration service that makes it easy to discover, prepare, and combine data for analytics, machine learning, and application development. AWS Glue provides all the capabilities needed for data integration, so you can start analyzing your data and putting it to use in minutes instead of months.

- You can use AWS Glue to organize, cleanse, validate, and format data for storage in a data warehouse or data lake.

---

## AWS EMR

### Questions

- What is EMR?

#### Notes

- **A managed cluster platform that simplifies running big data frameworks**, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data.

- You can process data for analytics purposes and business intelligence workloads using EMR together with Apache Hive and Apache Pig.

- You can use EMR to transform and move large amounts of data into and out of other AWS data stores and databases.

---

## AWS GuardDuty

### Questions

- What does AWS GuardDuty provide?

- Describe the three severity events that GuardDuty provides.

#### Notes

- Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your Amazon Web Services accounts, workloads, and data stored in Amazon S3.

- Amazon GuardDuty offers CloudWatch Events, CLI tools, and HTTPS APIs to assist you in creating your own custom automated functions to handle all alerted threats.

- To help you to determine the action you want to take for each alert, GuardDuty provides three levels of severity which we will take a deeper look at in just a moment:

1. **Low severity:** indicates threats that have already been removed or blocked before compromising any resource.
1. **Medium severity:** indicates suspicious activity, such as an increase in traffic specifically directed to, for example, bitcoin related domains, indicating cryptocurrency mining.
1. **High severity:** indicates a resource that is fully compromised and is constantly being used for unintended purposes.

---

## AWS MQ

### Questions

#### Notes

- AWS MQ is a managed Apache ActiveMQ(or RabbitMQ) broker service.

- This provides you a fully managed Apache ActiveMQ system in the cloud, with support for a variety of industry-standard queue and broadcast protocols like AMQP, JMS etc. It is useful when you have complicated delivery rules - or when you're migrating an existing system from outside AWS into AWS, and your systems happen to talk to one another with a standard queueing protocol.

---

---

## AWS Glue

### Questions

#### Notes

---
