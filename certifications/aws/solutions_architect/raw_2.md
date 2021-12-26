## EFS

- An Amazon EFS file system provides a standard file system interface and file system access semantics, allowing you to seamlessly integrate Amazon EFS with your existing applications and tools. EFS can be used with AWS services like EC2, ECS, and others.

- Multiple Amazon EC2 instances can access an Amazon EFS file system at the same time, allowing Amazon EFS to provide a common data source for workloads and applications running on more than one Amazon EC2 instance.

- Amazon Elastic File System (Amazon EFS) automatically grows and shrinks as you add and remove files with no need for management or provisioning.

- With support for inter-region VPC peering, you can connect EC2 instances in one region to EFS file systems in another.

- It is not possible to share a single EBS volume between multiple EC2 instances

- EFS is high-available storage that can span multiple availability zones.

- EBS volume is a storage area network (SAN) storage and _not_ a POSIX-compliant shared file system (life EFS).

- You can schedule automatic incremental backups of your EFS file system using the EFS-to-EFS Backup solution.

- Amazon EFS is not supported on Windows instances, it is only useable for Linux-based systems.

### Questions

- What is EFS and how is it different from EBS and S3?
- Does EFS automatically scale up and down with your infrastructure?
- Is it possible to share an **EBS instance** with multiple EC2 instances?
- Can you connect EC2s in one region to an EFS file system in another region?
- What type of storage is EBS, what type of file system is EFS?
- How can you back up your EFS?
- Can EFS be used on Windows machines?

---

## CloudWatch Events

> An application is hosted in an AWS Fargate cluster that runs a batch job whenever an object is loaded on an Amazon S3 bucket. The minimum number of ECS Tasks is initially set to 1 to save on costs, and it will only increase the task count based on the new objects uploaded on the S3 bucket. Once processing is done, the bucket becomes empty and the ECS Task count should be back to 1.

_Set up a CloudWatch Event rule to detect S3 object PUT operations and set the target to the ECS cluster with the increased number of tasks. Create another rule to detect S3 DELETE operations and set the target to the ECS Cluster with 1 as the Task count._

- You can use CloudWatch Events to run Amazon ECS tasks when certain AWS events occur.

- You can create a CloudWatch Events rule for an S3 service that will watch for object-level operations – PUT and DELETE objects. **For object-level operations, it is required to create a CloudTrail trail first.** On the Targets section, select the “ECS task” and input the needed values such as the cluster name, task definition, and the task count. You need two rules – one for the scale-up and another for the scale-down of the ECS task count.

- Creating your own Lambda function for this scenario is not really necessary. It is much simpler to **control ECS task directly as target for the CloudWatch Event rule.** CloudWatch Events can directly target an ECS task on the Targets section when you create a new rule.

- CloudWatch Alarms deliver near real-time stream of system events that describe changes in AWS resources. Events respond to these operational changes and take corrective action as necessary, by sending messages to respond to the environment, activating functions, making changes, and capturing state information.

- Amazon EventBridge is a service that builds upon the CloudWatch Events service API. Both Amazon EventBridge and CloudWatch Events use the same underlying infrastructure. You can still continue managing events through CloudWatch Events but the preferred way is to manage events via Amazon EventBridge.

### Questions

- Can you use CloudWatch Events to trigger ECS tasks when events occur?
- Can you create a CloudWatch events rule for S3 services to watch for _object-level_ operations?
- What are S3 object-level operations?
- For object-level operations in S3, what do you first need to create to track the events?
- Describe CloudWatch Events.
- What is Amazon EventBridge, does it use the same underlying infrastructure as CloudWatch Events?

---

## ECS Cluster

> prompt: A media company has an Amazon ECS Cluster, which uses the Fargate launch type, to host its news website. The database credentials should be supplied using environment variables, to comply with strict security compliance. As the Solutions Architect, you have to ensure that the credentials are secure and that they cannot be viewed in plaintext on the cluster itself.

_Use the AWS Systems Manager Parameter Store to keep the database credentials and then encrypt them using AWS KMS. Create an IAM Role for your Amazon ECS task execution role (**taskRoleArn**) and reference it with your task definition, which allows access to both KMS and the Parameter Store. Within your container definition, specify secrets with the name of the environment variable to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container._

- Amazon ECS enables you to inject sensitive data into your containers by storing your sensitive data in either **AWS Secrets Manager** secrets or **AWS Systems Manager Parameter Store** parameters and then referencing them in your container definition.

**Secrets can be exposed to a container in the following ways:**

1. To inject sensitive data into your containers as environment variables, use the `secrets` container definition parameter.

1. To reference sensitive information in the log configuration of a container, use the `secretOptions` container definition parameter.

- Although you can use Docker Secrets to secure the sensitive database credentials, this feature is only applicable in Docker Swarm. In AWS, the recommended way to secure sensitive data is either through the use of **Secrets Manager or Systems Manager Parameter Store.**

- It is not recommended to store sensitive credentials in S3. This entails a lot of overhead and manual configuration steps which can be simplified by simply using the Secrets Manager or Systems Manager Parameter Store.

### Questions

- With ECS, what two locations can you store your sensitive data for injection into your containers?
- What container definition parameter would you use to inject sensitive data into your containers as **environment variables?**
- What container definition parameter would you use to reference sensitive data in your container's **log configuration**?
- Docker Secrets is only usable for what service?
- What is the AWS recommended way to secure sensitive data for containers?
- Should you store sensitive credentials in S3? What is a better alternative?

---

## Amazon FSx for Windows

- Amazon FSx for Windows File Server provides fully managed Microsoft Windows file servers, backed by a fully native Windows file system. Amazon FSx for Windows File Server has the features, performance, and compatibility to easily lift and shift enterprise applications to the AWS Cloud.

- Amazon EFS is a managed NAS filer for EC2 instances based on **Network File System (NFS)** version 4. FSx for Windows, on the other hand, is a managed Windows Server that runs **Windows Server Message Block (SMB)**-based file services.

- NFS is one of the first network file sharing protocols native to Unix and Linux.

- Some Windows applications might not work on EFS or be feature-complete without access to a native Windows SMB file share.

- FSx It is accessible from Windows, Linux, and macOS compute instances and devices that support the SMB protocol.

- Thousands of compute instances and devices can access a file system concurrently.

- **Amazon EFS only supports Linux workloads.**

### Questions

- What is FSx for Windows?
- What does FSx run as its file sharing protocol?
- What does EFS run as its file sharing protocol?
- What does SMB stand for?
- What does NFS stand for?
- What Operating systems can FSx run on?
- What Operating systems can EFS run on?

---

## Lambda + RDS

> A car dealership website hosted in Amazon EC2 stores car listings in an Amazon Aurora database managed by Amazon RDS. Once a vehicle has been sold, its data must be removed from the current listings and forwarded to a distributed processing system.

_Create a native function or a stored procedure that invokes a Lambda function. Configure the Lambda function to send event notifications to an Amazon SQS queue for the processing system to consume._

- You can invoke an AWS Lambda function from an Amazon Aurora database with a native function or a stored procedure. This approach can be useful when you want to integrate your database running on Aurora MySQL with other AWS services.

- You can trigger a Lambda function whenever a listing is deleted from the database. You can then write the logic of the function to send the listing data to an SQS queue and have different processes consume it.

- **RDS events only provide operational events** such as DB instance events, DB parameter group events, DB security group events, and DB snapshot events. What we need in the scenario is to capture data-modifying events (INSERT, DELETE, UPDATE) which can be achieved thru native functions or stored procedures.

### Questions

- Can RDS events be used to track and trigger other events when a data-modifying event occurs on it?
- What needs to be used to trigger a lambda function?

---

## CloudFront + Lambda@Edge

> A popular social media website uses a CloudFront web distribution to serve their static contents to their millions of users around the globe. They are receiving a number of complaints recently that their users take a lot of time to log into their website. There are also occasions when their users are getting HTTP 504 errors. You are instructed by your manager to significantly reduce the user's login time to further optimize the system.

- Lambda@Edge lets you run Lambda functions to customize the content that CloudFront delivers, executing the functions in AWS locations closer to the viewer.

- In the given scenario, you can use Lambda@Edge to allow your Lambda functions to customize the content that CloudFront delivers and to execute the authentication process in AWS locations closer to the users.

- In addition, you can set up an origin failover by creating an origin group with two origins with one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin fails. This will alleviate the occasional HTTP 504 errors that users are experiencing.

### Questions

- What does Lambda@Edge allow you to do?
- Can Lambda@Edge be used to execute auth processes closer to users?
- What can you use, through CloudFront, to alleviate the 504 errors users are experiencing?

---

## SNS + SQS

> A company is using Amazon S3 to store frequently accessed data. When an object is created or deleted, the S3 bucket will send an event notification to the Amazon SQS queue. A solutions architect needs to create a solution that will notify the development and operations team about the created or deleted objects.

_Create an Amazon SNS topic and configure two Amazon SQS queues to subscribe to the topic. Grant Amazon S3 permission to send notifications to Amazon SNS and update the bucket to use the new SNS topic._

- If you are building brand new applications in the cloud, then it is highly recommended that you consider Amazon SQS and Amazon SNS. Amazon SQS and SNS are lightweight, fully managed message queue and topic services that scale almost infinitely and provide simple, easy-to-use APIs. You can use Amazon SQS and SNS to decouple and scale microservices, distributed systems, and serverless applications, and improve reliability.

- **If you're using messaging with existing applications** and want to move your messaging service to the cloud quickly and easily, it is recommended that you consider Amazon MQ.

- Amazon SWF is incorrect because this is a fully-managed state tracker and task coordinator service and not a messaging service, unlike Amazon MQ, AmazonSQS, and Amazon SNS.

- Create an Amazon SNS topic and configure two Amazon SQS queues to subscribe to the topic. Grant Amazon S3 permission to send notifications to Amazon SNS and update the bucket to use the new SNS topic.

- The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket. To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications.

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

### Questions

- What are SQS and SNS?
- What is Amazon MQ recommended for?
- What 3 destinations can S3 publish events to?
- Describe the _fanout scenario_.
- How many destinations can an S3 event notification be sent out to? How can this be alleviated with the SNS fanout solution?

---

### AWS Directory Service

> A telecommunications company is planning to give AWS Console access to developers. Company policy mandates the use of identity federation and role-based access control. Currently, the roles are already assigned using groups in the corporate Active Directory.

_Considering that the company is using a corporate Active Directory, it is best to use AWS Directory Service AD Connector for easier integration. In addition, since the roles are already assigned using groups in the corporate Active Directory, it would be better to also use IAM Roles. Take note that you can assign an IAM Role to the users or groups from your Active Directory once it is integrated with your VPC via the AWS Directory Service AD Connector._

- AWS Directory Service provides multiple ways to use Amazon Cloud Directory and Microsoft Active Directory (AD) with other AWS services. Directories store information about users, groups, and devices, and administrators use them to manage access to information and resources.

- AWS Directory Service provides multiple directory choices for customers who want to use existing Microsoft AD or Lightweight Directory Access Protocol (LDAP)–aware applications in the cloud. It also offers those same choices to developers who need a directory to manage users, groups, devices, and access.

### Questions

- Not really much to ask about this. Just read the bullet points.

---

## DynamoDB

> A popular social network is hosted in AWS and is using a DynamoDB table as its database. There is a requirement to implement a 'follow' feature where users can subscribe to certain updates made by a particular user and be notified via email. Which of the following is the most suitable solution that you should implement to meet the requirement?

_Enable DynamoDB Stream and create an AWS Lambda trigger, as well as the IAM role which contains all of the permissions that the Lambda function will need at runtime. The data from the stream record will be processed by the Lambda function which will then publish a message to SNS Topic that will notify the subscribers via email._

- A DynamoDB stream is an ordered flow of information about changes to items in an Amazon DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.

- Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attribute(s) of the items that were modified.

- A stream record contains information about a data modification to a single item in a DynamoDB table. You can configure the stream so that the stream records capture additional information, such as the "before" and "after" images of modified items.

- Amazon DynamoDB is integrated with AWS Lambda so that you can create _triggers_ —pieces of code that automatically respond to events in DynamoDB Streams. With triggers, you can build applications that react to data modifications in DynamoDB tables.

- If you enable DynamoDB Streams on a table, you can associate the stream ARN with a Lambda function that you write.

- Immediately after an item in the table is modified, a new record appears in the table's stream. AWS Lambda polls the stream and invokes your Lambda function synchronously when it detects new stream records. The Lambda function can perform any actions you specify, such as sending a notification or initiating a workflow.

- Remember that the DynamoDB Stream feature is not enabled by default.

### Questions

- What is a DynamoDB stream?
- When are events on a table captured, and what does the stream record capture?
- Can you configure the stream record to capture more information than just the primary key attribute(s) of the modified record?
- Describe the concept of "triggers".
- Is the DynamoDB Stream feature enabled by default?

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
