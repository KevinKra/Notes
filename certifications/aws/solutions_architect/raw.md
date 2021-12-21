## API Gateway

- API Gateway provides throttling at multiple levels. For example, API owners can set a rate limit of 1,000 requests per second for a specific method in their REST APIs, and also configure Amazon API Gateway to handle a burst of 2,000 requests per second for a few seconds.

- Amazon API Gateway tracks the number of requests per second. Any requests over the limit will receive a 429 HTTP response

### Q

1. API Gateway can set rate limits for your endpoints,
1. What status code do requests get when they're over the Gateway throttling limit?

### A

---

## Amazon RDS

- Amazon RDS provides metrics in real time for the operating system (OS) that your DB instance runs on.
- You can view the metrics for your DB instance using the console, or consume the Enhanced Monitoring JSON output from CloudWatch Logs in a monitoring system of your choice.
- By default, Enhanced Monitoring metrics are stored in the CloudWatch Logs for 30 days.

- Take note that there are certain differences between CloudWatch and Enhanced Monitoring Metrics. CloudWatch gathers metrics about CPU utilization from the hypervisor for a DB instance, and Enhanced Monitoring gathers its metrics from an agent on the instance. As a result, you might find differences between the measurements, because the hypervisor layer performs a small amount of work.

- CPU% and MEM% metrics are not readily available in the Amazon RDS console

- The data provided by CloudWatch is not as detailed as compared with the Enhanced Monitoring feature in RDS. Take note as well that you do not have direct access to the instances/servers of your RDS database instance, unlike with your EC2 instances where you can install a CloudWatch agent or a custom script to get CPU and memory utilization of your instance.

- Using Amazon CloudWatch to monitor the CPU Utilization of your database is incorrect because although you can use this to monitor the CPU Utilization of your database instance, it does not provide the percentage of the CPU bandwidth and total memory consumed by each database process in your RDS instance.

- Take note that CloudWatch gathers metrics about CPU utilization from the hypervisor for a DB instance while RDS Enhanced Monitoring gathers its metrics from an agent on the instance.

- When you create or modify your DB instance to run as a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a synchronous **standby** replica in a different Availability Zone.

- **Read Replicas provides an asynchronous replication** instead of synchronous.

---

## EC2

- An **Amazon EBS** volume is a durable, block-level storage device that you can attach to your instances.

- The AWS Nitro System is the underlying platform for the latest generation of EC2 instances that enables AWS to innovate faster, further reduce the cost of the customers, and deliver added benefits like increased security and new instance types.

- **Amazon EBS** is a persistent block storage volume. It can persist independently from the life of an instance.

- Since the scenario requires you to have an EBS volume with up to 64,000 IOPS, you have to launch a Nitro-based EC2 instance, since the maximum IOPS and throughput (64,000 IOPS) is **only** guaranteed on instances built with the Nitro system provisioned with more than 32k IOPS. Other instances guarantee up to 32,000 IOPS only.

- Although an **Instance Store** is a block storage volume, it is not persistent and the data will be gone if the instance is restarted from the stopped state. (note that this is different from the OS-level reboot. In OS-level reboot, data still persists in the instance store).

- An **instance store** only provides temporary block-level storage for your instance. It means that the data in the instance store can be lost if the underlying disk drive fails, if the instance stops, and if the instance terminates.

- Although **Amazon EFS** can provide over 64,000 IOPS, this solution uses a file system and not a block storage volume

---

## Microsoft Active Directory

- Microsoft Active Directory implements Security Assertion Markup Language (SAML), you can set up a SAML-Based Federation for API Access to your AWS cloud. In this way, you can easily connect to AWS using the login credentials of your on-premises network.

- AWS supports identity federation with SAML 2.0, an open standard that many identity providers (IdPs) use. This feature enables federated single sign-on (SSO), so users can log into the AWS Management Console or call the AWS APIs without you having to create an IAM user for everyone in your organization.

- By using SAML, you can simplify the process of configuring federation with AWS, because you can use the IdP's service instead of writing custom identity proxy code.

- SAML 2.0-Based Federation by using a **Microsoft Active Directory Federation Service (AD FS)**. Before you can use SAML 2.0-based federation, you must configure your organization's IdP and your AWS account to trust each other. The general process for configuring this trust is described in the following steps. Inside your organization, you must have an IdP that supports SAML 2.0, like Microsoft Active Directory Federation Service (AD FS, part of Windows Server), Shibboleth, or another compatible SAML 2.0 provider.

- Setting up SAML 2.0-Based Federation by using a Web Identity Federation is primarily used to let users sign in via a well-known external identity provider (IdP), such as Login with Amazon, Facebook, Google. It does not utilize Active Directory.

---

## CloudFront

- CloudFront **signed URLs** and **signed cookies** provide the same basic functionality: they allow you to control who can access your content.

_**Use signed URLs for the following cases:**_

1. You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.

1. You want to restrict access to individual files, for example, an installation download for your application.

1. Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

_**Use signed cookies for the following cases:**_

1. You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.

1. You don't want to change your current URLs.

- Use Signed Cookies to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required _Set-Cookie_ headers to the viewer which will unlock the content only to them.

- CloudFront **Match Viewer** is an Origin Protocol Policy which configures CloudFront to communicate with your origin using HTTP or HTTPS, depending on the protocol of the viewer request. CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols.

- CloudFront **Signed URLs** are primarily used for providing access to individual files. In addition, the scenario explicitly says that they don't want to change their current URLs which is why implementing Signed Cookies is more suitable than Signed URL.

- CloudFront **Field-Level Encryption** only allows you to securely upload user-submitted sensitive information to your web servers. It does not provide access to download multiple private files.

---

## Lambda

- AWS Lambda lets you run code without provisioning or managing servers. You pay only for the compute time you consume. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration.

- Just upload your code, and Lambda takes care of everything required to run and scale your code with high availability.

- You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.

- The first time you invoke your function, AWS Lambda creates an instance of the function and runs its handler method to process the event. When the function returns a response, it stays active and waits to process additional events. If you invoke the function again while the first event is being processed, Lambda initializes another instance, and the function processes the two events concurrently.

- As more events come in, Lambda routes them to available instances and creates new instances as needed. When the number of requests decreases, Lambda stops unused instances to free up the scaling capacity for other functions.

- Your functions' concurrency is the number of instances that serve requests at a given time.

- For an initial burst of traffic, your functions' cumulative concurrency in a Region can reach an initial level of between 500 and 3000, which varies per Region.

- The first requirement (to handle the bursts of traffic to the example website) is to create a solution that will allow the users to access the data using an API. To implement this solution, you can use Amazon API Gateway. The second requirement is to handle the burst of traffic within seconds. You should use AWS Lambda in this scenario because Lambda functions can absorb reasonable bursts of traffic for approximately 15-30 minutes.

- Lambda can scale faster than the regular Auto Scaling feature of Amazon EC2, Amazon Elastic Beanstalk, or Amazon ECS. This is because AWS Lambda is more lightweight than other computing services.

- Under the hood, Lambda can run your code to thousands of available AWS-managed EC2 instances (that could already be running) within seconds to accommodate traffic. This is faster than the Auto Scaling process of launching new EC2 instances that could take a few minutes or so.

- Whether using API Gateway with ECS, Elastic Beanstalk and Auto Scaling, or Auto Scaling EC2 instances, all of the options take minutes to spin up resources to handle demand. Lambdas take seconds.

---

## Amazon FSx for Windows File Server

- Amazon FSx for Windows File Server provides fully managed, highly reliable, and scalable file storage that is accessible over the industry-standard Service Message Block (SMB) protocol. It is built on Windows Server, delivering a wide range of administrative features such as user quotas, end-user file restore, and Microsoft Active Directory (AD) integration

- Amazon FSx is accessible from Windows, Linux, and MacOS compute instances and devices. Thousands of compute instances and devices can access a file system concurrently.

- Amazon FSx works with Microsoft Active Directory to integrate with your existing Microsoft Windows environments.

- You have two options to provide user authentication and access control for your file system: AWS Managed Microsoft Active Directory and Self-managed Microsoft Active Directory.

- Take note that after you create an Active Directory configuration for a file system, you can't change that configuration. However, you can create a new file system from a backup and change the Active Directory integration configuration for that file system.

- These configurations allow the users in your domain to use their existing identity to access the Amazon FSx file system and to control access to individual files and folders.

- **Amazon EFS does not support Windows systems, only Linux OS.** You should use Amazon FSx for Windows File Server instead to satisfy the requirement in the scenario (scenario being a company plans to migrate its on-premises workload to AWS. The current architecture is composed of a Microsoft SharePoint server that uses a Windows shared file storage.)

- Creating a **Network File System (NFS)** file share using AWS Storage Gateway is incorrect because **NFS file share is mainly used for Linux systems.** Remember that the requirement in the scenario is to use a Windows shared file storage. Therefore, you must use an SMB file share instead, which supports Windows OS and Active Directory configuration.

---

## EBS volumes

- SSD-backed volumes, such as General Purpose SSD (gp2) and Provisioned IOPS SSD (io1), deliver consistent performance whether an I/O operation is random or sequential.

- HDD-backed volumes like Throughput Optimized HDD (st1) and Cold HDD (sc1) deliver optimal performance only when I/O operations are large and sequential.

- SSD can be used as a bootable volume, HDD cannot.

- Provisioned IOPS SSD volumes are much more suitable, than general purpose SSDs (gp2), in meeting the needs of I/O-intensive database workloads such as MongoDB, Oracle, MySQL, and many others.

---

## Storage Gateway

> A file gateway supports a file interface into Amazon Simple Storage Service (Amazon S3) and combines a service and a virtual software appliance. By using this combination, you can store and retrieve objects in Amazon S3 using industry-standard file protocols such as Network File System (NFS) and Server Message Block (SMB). The software appliance, or gateway, is deployed into your on-premises environment as a virtual machine (VM) running on VMware ESXi, Microsoft Hyper-V, or Linux Kernel-based Virtual Machine (KVM) hypervisor.

- A file gateway supports a file interface into Amazon Simple Storage Service (Amazon S3) and combines a service and a virtual software appliance. By using this combination, you can store and retrieve objects in Amazon S3 using industry-standard file protocols such as Network File System (NFS) and Server Message Block (SMB).

- The software appliance, or gateway, is deployed into your on-premises environment as a virtual machine (VM) running on VMware ESXi, Microsoft Hyper-V, or Linux Kernel-based Virtual Machine (KVM) hypervisor.

- AWS Storage Gateway supports the Amazon S3 Standard, Amazon S3 Standard-Infrequent Access, Amazon S3 One Zone-Infrequent Access and Amazon Glacier storage classes.

- When you create or update a file share, you have the option to select a storage class for your objects.

- Although you can write objects directly from a file share to the S3-Standard-IA or S3-One Zone-IA storage class, it is recommended that you use a Lifecycle Policy to transition your objects rather than write directly from the file share, especially if you're expecting to update or delete the object within 30 days of archiving it.

- Launch a new file gateway that connects to your on-premises data center using AWS Storage Gateway. Upload the documents to the file gateway and set up a lifecycle policy to move the data into Glacier for data archival.

- **tape gateways** provide cost-effective and durable archive backup data in Amazon Glacier, it does not meet the criteria of being retrievable immediately within minutes. It also doesn't maintain a local cache that provides low latency access to the recently accessed data and reduce data egress charges.

- EBS Volumes are not as durable compared with S3 and it would be more cost-efficient if you directly store the documents to an S3 bucket.

- You can use AWS Direct Connect with AWS Storage Gateway to create a connection for high-throughput workload needs, providing a dedicated network connection between your on-premises file gateway and AWS.

- **Snowmobile** is mainly used to migrate the _entire data of an on-premises data center to AWS_. This is not a suitable approach as the company still has a hybrid cloud architecture which means that they will still use their on-premises data center along with their AWS cloud infrastructure.

---

## AWS WAF

> The website is receiving a large number of illegitimate external requests from multiple systems with IP addresses that constantly change. To resolve the performance issues, the Solutions Architect must implement a solution that would block the illegitimate requests with minimal impact on legitimate traffic.

- **AWS WAF** is tightly integrated with Amazon CloudFront, the Application Load Balancer (ALB), Amazon API Gateway, and AWS AppSync – services that AWS customers commonly use to deliver content for their websites and applications.

- When you use AWS WAF on Amazon CloudFront, your rules run in all AWS Edge Locations, located around the world close to your end-users. This means security doesn’t come at the expense of performance.

- Blocked requests are stopped before they reach your web servers.

- When you use AWS WAF on regional services, such as Application Load Balancer, Amazon API Gateway, and AWS AppSync, your rules run in the region and can be used to protect Internet-facing resources as well as internal resources.

- A **rate-based rule** tracks the rate of requests for each originating IP address and triggers the rule action on IPs with rates that go over a limit. You set the limit as the number of requests per 5-minute time span.

- AWS WAF web ACL. There are two types of rules in creating your own web ACL rule: **regular** and **rate-based rules**.

- A regular rule only matches the statement defined in the rule. If you need to add a rate limit to your rule, you should create a rate-based rule.

- Although NACLs can help you block incoming traffic, they wouldn't be able to limit the number of requests from a _single IP address that is dynamically changing._

- **A security group can only allow incoming traffic.** You can't **deny traffic using security groups**. In addition, it is not capable of limiting the rate of traffic to your application unlike AWS WAF.
