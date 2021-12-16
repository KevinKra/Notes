# Disaster Recovery

### Questions

1. For data storage, what is the difference between Synchronous, Asynchronous, and Quorum-based replication?
1. What is RTO?
1. What is RPO?
1. Describe Backup and Restore.
1. Describe Pilot Light.
1. Describe Warm Standby.
1. Describe Multi-Site.
1. Does AWS have a suggested disaster recovery tool?

### Answers

1. **Synchronous replication** only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node. **Asynchronous replication** decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas. **Quorum-based replication** combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.
1. RTO, or **Recovery Time Objective**, is _the time it takes_ after a disruption to restore a business process to its service level.
1. RPO, or **Recovery Point Objective**, is _the acceptable amount of data loss_ measured in time.
1. **Backup and Restore** involves taking frequent backups of your most critical systems. Once disaster strikes you simply restore these backups to recover data and quickly. B&R usually has the longest RTO and your RPO will depend on how frequently you backup your data.
1. **Pilot Light** provides quicker recovery time than backup and restore because core pieces of the system are already running and are continually kept up to date. Data loss is very minimal in this scenario for the critical parts, but for the other the less critical parts, you have the same RTO and RPO as backup and restore.
1. **Warm standby** provides a scaled-down version of a fully functional environment that is always running. For example, you have a subset of undersized servers and databases that have the same exact configuration as your primary, and are constantly updated also. Once disaster strikes, you only have to make minimal reconfigurations to re-establish the environment back to its primary state. Warm standby is costlier than Pilot Light, but you have better RTO and RPO.
1. **Multi-Site** runs exact replicas of your infrastructure in an active-active configuration. In this scenario, all you should do in case of a disaster is to reroute traffic onto another environment. Multi-site is the most expensive option of all since you are essentially multiplying your expenses with the number of environment replicas. It does give you the best RTO and RPO however.
1. Yes, AWS promotes their disaster recovery tool called **CloudEndure** which they are suggesting to their customers as the preferred solution for disaster recovery workloads.

---

# EC2 Auto Scaling Group

### Questions

1. Conceptually, what is an auto-scaling group?
1. What are some benefits of using Auto Scaling Group?
1. What is an EC2 Auto Scaling group **Lifecycle Hook?**
1. What are the four EC2 Auto Scaling Group options?
1. What are the three EC2 Auto Scaling Group Policy Types?
1. What is an EC2 Auto Scaling Group **cooldown period?** What is the only policy type that has a cooldown period?
1. Describe Target Tracking Scaling.
1. Describe Step Scaling.
1. Describe Simple Scaling.
1. The dynamic scaling options, step and simple scaling, have what similarities and differences?
1. What is the primary issue with simple scaling?
1. Do Target Tracking or Step Scaling need to wait for the cooldown period to complete before initiating additional scaling activities?
1. Under what circumstances (3) does EC2 Auto Scaling mark an instance as unhealthy?
1. What is scale-in and scale-out?
1. What do you need to determine when you configure automatic scale in rules?
1. What can you use to prevent specific instances from being terminated during automatic scale-ins?
1. What are **launch templates?**
1. What is a **launch configuration?**

### Answers

1. An auto-scaling group is a collection, or group, of EC2 instances that share a common intention and design.
1. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies.
1. A Lifecycle hook allows your auto-scaling group to perform custom actions when instances launch or terminate.
1. Scale to maintain current instance levels, manual scaling, scale based on a schedule, and scale based on a demand.
1. Target Tracking Scaling, Step Scaling, and Simple Scaling.
1. A Cooldown period is a _configurable_ setting that helps ensure to not launch or terminate additional instances before previous scaling activities take effect.
1. **Target tracking scaling**—Increase or decrease the current capacity of the group based on a target value for a specific metric.
1. **Step scaling**—Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as **step adjustments**, that vary based on the size of the alarm breach.
1. **Simple scaling**—Increase or decrease the current capacity of the group based on a single scaling adjustment.
1. Both require you set up CloudWatch alarms for scaling policies, specify high and low thresholds for the alarms, whether to add or remove instances, how many, or to set an exact size. The main difference is step adjustments increase the capacity of the group, in a varying manner, based on the size of the alarm breach.
1. The primary issue with simple scaling is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms.
1. No, Target tracking or step scaling policies can trigger a scaling activity immediately without waiting for the cooldown period to expire.
1. If the instance is in a state other than running, the system status is impaired, or ELB reports the instance failed health checks.
1. Scaling in is reducing instances, scaling out is increasing instances.
1. You need to set a **termination policy** to determine which instances to delete first.
1. You can use **instance protection** to prevent instances from being terminated during automatic scale-in.
1. Launch templates specify instance configuration information when you launch EC2 instances, and they allow you to have multiple versions of a template.
1. A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances, and you specify information for the instances.

## Application Auto Scaling

### Questions

1. What are the three approaches to Application Auto Scaling?
1. Explain Target tracking scaling. How does it differ from the similarly named EC2 Scaling solution of the same name?
1. What does Schedule Scaling use to scale resources?
1. Can you have multiple target tracking policies for a scalable target?

### Answers

1. Target tracking scaling, Step scaling, and Scheduled scaling.
1. Application Target scaling scales a resource based on a target value for a specific CloudWatch metric. Unlike EC2 Target tracking, Application Target Tracking uses data points derived from CloudWatch to trigger scaling events.
1. Scheduled Scaling uses dates/time to determine scaling.
1. Yes, you can have multiple target tracking policies for a scalable target, provided they each use different metrics.

## Monitoring

### Questions

1. Does autoscaling provide health checks on instances in standby state?
1. What do CloudWatch metrics enable?

### Answers

1. Nope. Standby state can be used for performing updates/changes/troubleshooting without health checks being performed or replacement instances being launched.
1. CloudWatch metrics enable you to retrieve statistics about Auto Scaling-published data points as an ordered set of time-series data, known as metrics. You can use these metrics to verify that your system is performing as expected.

---

# CloudTrail

### Questions

1. What is an **event** in CloudTrail?
1. What are the two types of events that can be logged in CloudTrail?
1. Can a trail be applied to one region or all regions? What the best practice regarding this?
1. Where are events logged, where are events logged for global services?
1. CloudTrail, with multi-region trail enabled, only enables the tracking of regional services (EC2, S3, RDS, etc.) and not global services like IAM, CloudFront, WAF, R53, and so on. What do you need to do to track global services with multi-region CloudTrail?

### Answers

1. An event in CloudTrail is the record of activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
1. Management events and data events. By default, trails log management events, but not data events.
1. A trail can be applied to all regions or a single region. As a best practice, create a trail that applies to all regions in the AWS partition in which you are working. This is the default setting when you create a trail in the CloudTrail console.
1. For most services, events are recorded in the region where the action occurred. For global services such as AWS Identity and Access Management (IAM), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East (N. Virginia) Region.
1. In order to satisfy the requirement, you have to add the `--include-global-service-events` parameter in your AWS CLI command.

---

# KMS / SSE-C - Encryption related

- **Client-side encryption** is the act of encrypting data _before_ sending it to S3.
- There are two options to enable client-side encryption: **AWS-managed customer key or use a client-side master key.**
- When you use an AWS KMS-managed customer master key to enable client-side data encryption, _you provide an AWS KMS customer master key ID (CMK ID) to AWS._
- When you use a client-side master key, for client-side data encryption, your client-side master keys and your encrypted data are **_never_** sent to AWS.
- When you provide a client-side master key to the **Amazon S3 encryption client**. **The S3 encryption client uses the master key only to encrypt the data encryption key** that it generates randomly.

### S3 Encryption Client Steps

> This is how client-side encryption using client-side master key works:

#### Uploading

1. The Amazon S3 encryption client generates a one-time-use **symmetric key** (also known as a **data encryption key** or **data key**) locally.
1. It uses the symmetric key to encrypt the data of a **single Amazon S3 object**.**The S3 encryption client generates a separate data key for each object.**
1. The S3 encryption client encrypts the data encryption key using the master key that you provide.
1. The S3 encryption client uploads the encrypted data key and its material description as part of the object metadata.
1. The S3 encryption client uses the material description to determine which client-side master key to use for decryption.
1. The S3 encryption client uploads the encrypted data to Amazon S3 and saves the encrypted data key as object metadata (`x-amz-meta-x-amz-key`) in Amazon S3.

- S3 encrypt client generates a data-key, uses that key to encrypt the data of a single s3 object, it then encrypts the data-key based on your master key, then it uploads the newly made encrypted data-key using its material description as metadata -- this metadata is used by the s3 encryption client to determine which client-side master key to use for decryption, it then uploads the encrypted data to S3 and saves the encrypted data-key as object metadata in S3.

#### Downloading

1. Client downloads the encrypted object from S3.
1. Using the material description from the object's metadata, the client determines which master key to use for decrypting the data-key.
1. The master-key is used to decrypt the data-key, then the decrypted data-key is used to decrypt the object.

- encrypted object is downloaded from S3, local customer-key is used to decrypt data-key, then the decrypted data-key is used to decrypt the object.

---

- SSE: Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
- CSE: encrypting your data locally to ensure its security as it passes to the Amazon S3 service. The Amazon S3 service receives your encrypted data; it does not play a role in encrypting or decrypting it.
- Client-side encryption with a KMS-managed customer master key, you provide an AWS KMS customer master key ID (CMK ID) to AWS. AWS now has the master-key.
- S3 server-side encryption potentially results in unencrypted traffic being sent to AWS (if SSL/TLS isn't used). This is a data _in-transit_ vulnerability.\*
- When using S3 server-side encryption with a customer-provided key (SSE-C), **you actually provide the encryption key as part of your request to upload the object to S3.**
- You can use **SSL/TLS (Secure Socket Layer or Transport Layer Security)** or client-side encryption, to protect data in transit.

#### AWS Encryption SDK

- The AWS Encryption SDK is a client-side encryption library that is separate from the language-specific SDKs. You can use this encryption library to more easily implement encryption best practices in Amazon S3.
- Unlike the Amazon S3 encryption clients in the language-specific AWS SDKs, the AWS Encryption SDK is not tied to Amazon S3 and can be used to encrypt or decrypt data to be stored anywhere.

---

# S3

> "Simple Storage Solution" provides object storage for static files (media, images, code, etc.) on AWS.

- Unlimited Storage.
- Objects up to 5TB in size.
- Files stored in Buckets (similar to folders).
- Universal Namespace.
- Returns a 200 status code when an upload is successful.
- Data in S3 is stored across multiple devices and facilities to ensure availability and durability.

#### Example S3 URL

`https://bucket-name.s3.amazonaws.com/key-name`

### S3 Object Overview

- **Key**: the name of the object (myImage.jpeg).
- **Value**: the data itself. Made up of a sequence of bytes.
- **Version ID**: important for storing multiple versions of the same object.
- **Metadata**: data about the data, ie. content-type, last-modified, etc.

#### Highly Available and Highly Durable

- Built for 99.95% - 99.99% service availability (depending on S3 tier).
- Designed for 99.999999999% durability for **data stored in S3**.

### Questions:

- How much storage does AWS provide?
- What is the largest object file size for S3?
- Where are files stored?
- What does 'universal namespace' mean?
- What response do you get when an upload to S3 is successful?
- Is data stored redundantly in multiple AZs on AWS (non-1ZIA) S3?
- What are the main parts of an S3 URL?
- What are the four parts of an S3 Object?

## S3 Tiers

### S3 Standard

> Data is stored redundantly across multiple devices in multiple facilities in >=3 AZs.

- 99.99% availability.
- 99.999999999% durability (11 9's).
- No retrieval fee.
- Stored in 3 or more AZs.
- Designed for frequent access.
- Suitable for most workloads.
- Default storage class.

### S3 Standard-Infrequent Access (S3-IA)

> Data that is accessed less frequently but requires rapid access when needed.

- 99.9% availability.
- 99.999999999% durability (11 9's).
- Stored in 3 or more AZs.
- Low per-GB storage price.
- Per-GB retrieval fee.
- Long-term storage, data store, disaster recovery.

### S3 Standard-Infrequent One-Zone Access

> IA Data stored redundantly in **one** AZ.

- 99.5% availability.
- 99.999999999% durability (11 9's).
- Stored in 1 AZs.
- Costs 20% less than S3 Standard-IA.
- Great for long-lived, infrequently access, non-critical data.

### S3 Glacier

> Cheap storage for rarely used data.

- 99.99% availability.
- 99.999999999% durability (11 9's).
- Stored in 3 or more AZs.
- Pay each time data is accessed.
- Used for archiving data.
- Retrieval time of 1 minute to 12 hours.
- Data that is only accessed a few times a year.

### S3 Glacier Deep-Archive

> Cheapest long-term rarely accessed data.

- 99.99% availability.
- 99.999999999% durability (11 9's).
- Stored in 3 or more AZs.
- Pay each time data is accessed.
- Default retrieval time of 12 hours.
- Data that is accessed roughly once or twice a year at most.

### S3 Intelligent Tiering

> Moves data automatically (using ML) to most cost-effective tier.

- 99.99% availability.
- 99.999999999% durability (11 9's).
- Has a monthly fee of $0.0025 per 1,000 objects.

## Lifecycle Management

> Define rules to automatically transition objects to cheaper storage tier or delete objects that are no longer required after a set period of time.

- Can be used with Versioning. You can use lifecycle management to **move different versions** of objects to **different storage tiers.**

## Versioning

> With versioning, all versions of an object are stored and can be retrieved, including deleted objects.

- All versions of an object are stored in S3. This includes writes and **even if you delete an object.**
- **Once enabled, cannot be disabled.**
- Can be integrated with Lifecycle rules.
- Can support MFA.

### Exam Tips:

- Even if the access policy of a bucket is to set all objects as public, previous versions of an object do not default to those rules. **You need to manually set the previous version of the object to public.**
- With versioning enabled, deleted objects **are not actually deleted.** If you want to _restore_ a deleted object, you can delete the "delete marker" to restore the object.

## Questions:

- What are the S3 Classes/Tiers?
- How many AZ locations are used (excluding one-zone IA) to store S3 data?
- What is the default storage class?
- What is the availability of S3-IA?
- What is the availability of S3-OZIA?
- What is the durability of all S3 storage classes?
- What are the retrieval time ranges for S3 Glacier and S3 Glacier Deep-Archive?
- What is lifecycle management? Can it be used with versioning?
- Once enabled, can versioning be disabled?
- Can versioning be integrated into lifecycle rules?
- Can versioning support MFA?
- Are deleted objects still stored with S3 versioning?
- If a bucket policy sets all objects to public, do previously versioned objects automatically become public?
- With versioning, how would you restore a deleted object?

## Securing S3 Data

- **Server-Side Encryption**: you can set default encryption on a bucket to encrypt all new objects when they are stored in the bucket.
- **Access Control Lists (ACLs)**: define which AWS accounts or groups are granted access and the type of access. You can attach S3 ACLs to **individual objects** within a bucket.
- **Bucket Policies**: S3 bucket policies specify what actions are allowed or denied on a per-bucket basis. Policies that modify policies for the **entire bucket**.

### S3 Data Consistency

- **Strong Read-after-Write consistency**: after you create a new object (POST) or update (PUT) an object within a bucket, any subsequent read request immediately receives the _latest_ version of the object.

#### Exam Tips:

- Buckets are private by default.
- Object ACLs can make **individual objects** public.
- Bucket policies can make **entire buckets** public.
- HTTP status code 200 is return on successful upload.
- S3 can host static websites. Since S3 **Scales automatically** this can be very powerful for simple static websites (a marketing website for example).

## S3 Object Lock

> Write once, read many (WORM).

- Helps prevent S3 Objects from being deleted or modified for a fixed period, or even indefinitely.
- You can use **S3 Object Lock** to meet regulatory requirements that require WORM storage or add an extra layer of protection against object changes or deletion.
- Can be applied to **individual objects or applied across an entire bucket.**

### Governance Mode

- **Users cannot overwrite or delete** an object version or alter its lock settings **unless they have special permissions.**

### Compliance Mode

- A protected object version **cannot be overwritten or deleted by any user**, including the root user of the account. Compliance mode ensures an object version **can't be overwritten or deleted** for the duration of the retention period.

#### Retention Periods

- Protects an object version for a fixed about of time. When you place a retention period on an object version, S3 adds a timestamp in the versions metadata indicating when the retention period expires.
- After the retention period expires, the object can be overwritten or deleted **unless you place a legal hold on the object.**

#### Legal Holds

- S3 Object Lock also enables you to place a legal hold on an object version. Like a retention period, a legal hold prevents an object version from being overwritten or deleted. However, **a legal hold doesn't have an associated retention period and remains in effect until removed.**
- Legal holds can be placed and removed by users with `s3:PutObjectLegalHold` permission.

### Glacier Vault Lock

> Easily deploy and enforce compliance controls for individual S3 Glacier vaults with a vault lock policy.

- You can specify controls, such as WORM, in a vault lock policy and lock the policy from future edits. Once locked, the policy can no longer be changed.

## Questions

- What is S3 Object Lock?
- What is the WORM model?
- Can S3 Object let be set to a fixed period or indefinitely?
- What are two scenarios where S3 Object Lock can be useful?
- Can S3 Object Lock be applied on buckets and on a per-object level?
- What is the S3 Object Lock Governance mode?
- What is the S3 Object Lock Compliance mode?
- What is the difference between Governance and Compliance mode?
- What is the retention period, is there metadata stored to indicate it?
- What can you add to prevent an S3 object / bucket to prevent it from deleted or overwritten after the retention period expires?
- What is the key difference between a legal hold and a retention period?
- What permission is required for a user to replace or remove legal hold?
- What is a Glacier Vault Lock policy?
- Once locked, can a policy be changed on an S3 Glacier vault?

## Encrypting S3 Data

### Types of Encryption

#### Encryption in Transit

- **SSL/TLS**
- **HTTPS**

#### Encryption at Rest: Server-side Encryption

- **SSE-S3**: S3-managed keys, uses AES 256-bit encryption.
- **SSE-KMS**: AWS Key Management Service - managed keys.
- **SSE-C**: Customer-provided keys.

#### Encryption at Rest: Client-side Encryption

- You encrypt the files before you upload them to S3.

### Enforcing Server-side Encryption

- **Console**: select the encryption settings on your S3 Bucket.
- **Bucket-policy**: Enforce encryption using a bucket policy.

if a file is to be encrypted at **upload time** the `x-amz-server-side-encryption` parameter will be included in the request header. There are two variants of the parameter: `AES256` and `aws:kms`. When the parameter is included in the header of the **PUT** request, it tells S3 to encrypt the object at the time of the upload, using the specified encryption method.

- you can create a **Bucket-Policy** that denies _any_ request that doesn't have the `x-amz-server-side-encryption` parameter in the request header. This allows us to enforce encryption policy with bucket-level policies.

- there is no point to having server-side encryption and making a bucket public.

## Optimize S3 Performance

### S3 Prefixes

- prefixes just use the **folders** in the prefixes.

#### examples:

`mybucketname/folder1/subfolder1/myfile.jpg` prefix: `/folder1/subfolder1`
`mybucketname/folder2/subfolder1/myfile.jpg` prefix: `/folder2/subfolder1`
`mybucketname/folder3/myfile.jpg` prefix: `/folder3`

### S3 Performance

- S3 has extremely low latency. You can get the first byte out of S3 within **100-200 milliseconds.**
- You can achieve a high number of requests: **3,500 PUT/POST/DELETE** and **5,500 GET requests**, per-second per-prefix.
- The more prefixes you have in a bucket, the better performance you get since CRUD operations are distributed across prefixes. For example, two prefixes provide 11,000 GET requests per-second.

### Limitations with KMS

- If you are using SSE-KMS, you must keep in mind the KMS-limits. For example, when you **upload** a file, you will call `GenerateDataKey` in the KMS API. When you **download** a file, you will call `Decrypt` from the KMS API.
- Uploading/Downloading will count towards the **KMS quota**.
- KMS Request Rates are **region-specific**. However, it's either 5,500, 10,000, or 30,000 requests per-second.
- Currently, you **cannot** request a KMS quota increase.
- If you're concerned about hitting the KMS limit, it may be ideal to simply use the native S3 encryption that's built in.

#### Multipart Uploads (uploads)

- Recommended for files **over 100MB**.
- **Required** for files over 5GB.
- Paralleled uploads increase efficiency.
- Break big file into smaller parts and upload smaller parts simultaneously.

#### S3 Byte-Range Fetches (downloads)

- Parallelize downloads by specifying byte ranges.
- If there is a failure in the download, it's only for a specific byte range (partial failure).
- Can be used to speed up downloads.
- Can be used to download parts of a file. For example, just header information.

## Questions:

- What are the two types of encryption?
- What are two formats of encryption in-transit?
- What are two types of encryption at rest?
- What are the three types server-side of encryption at rest provided through AWS?
- What are two approaches for enforcing server-side encryption?
- If a file is to be encrypted at upload time what parameter is included in the header? What are the two variants that this parameter header can be?
- How can you create bucket-level policies to enforce encryption?
- What are S3 prefixes? What are they defined by?
- What is an S3 prefix example?
- How are prefixes leveraged to optimize S3 performance?
- How quickly can you get the first bytes from S3?
- What are the PUT/POST/DELETE and GET request rates?
- How does using SSE-KMS limit S3 performance?
- What methods are called on KMS download and upload?
- Are KMS request rates region specific? What are the limits to requests per-second?
- Can you request KMS quota requests?
- If your concerned about hitting the KMS limit, what can you use instead?
- What are multipart uploads, do they increase efficiency?
- What file sizes are multipart uploads recommended for?
- What file sizes are multipart uploads required for?
- What are S3 Byte-Range fetches?
- Can S3 Byte-Range fetches download just parts of a file?
- Whats the key difference between Multipart uploads and S3 Byte-Range fetches?

## Backing up Data with S3 Replication

> A way to replicate objects from one bucket to another.

- Used to be called Cross-Region Replication.
- Versioning **must be enabled** on the source and target buckets. May be easiest to create two new buckets, a source and destination.
- Objects already in existing buckets **are not** automatically replicated. Once replication is turned on, all _subsequent_ updated objects will be replicated automatically.
- Delete markers are not replicated by default. Deleting individual versions or delete markers will not be replicated.
- source and destination buckets **can be different storage classes.**

## Questions:

- What did S3 Replication used to be called?
- Does S3 versioning need to be enabled on both the source and destination buckets?
- Are Objects, that already exist on a Bucket, automatically replicated once replication is turned on?
- Will subsequent objects be replicated?
- Are delete markers replicate by default?
- Will deleting individual versions or delete markers be replicated across buckets?
- Can source and destination buckets be different S3 storage classes?

---

# VPC

## VPC Basics

- Smallest AWS IP netmask is 28 (16 addresses)
- Largest AWS IP netmask is 16 (65,536 addresses)
- "Block sizes must be between a 16 and 28 netmask."
- `2^(32-n)`, 'n' being netmask value.
- AWS reserves 5 IP addresses within a given subnet CIDR block.
- Every region comes with a `Default VPC` with `Default Subnets` contained within. They are public-facing, with one default subnet provided for each AZ in the region.
- To connect instances in a private subnet to a corporate data center a `Virtual Private Gateway` could be connected to the VPC to establish a VPN connection.
- You can block **Specific IP addresses** with `NACLs`.
- all subnets in a default VPC have a route out to the internet.
- each EC2 instance, in a default subnet, has a public and private IP address.
- VPCs consist of internet gateways (or virtual private gateways), route tables, NACLs, subnets, and security groups.
- 1 subnet is always in 1 availability zone, they _cannot_ span multiple AZs.
- If you go to a subnet's page, you can set it to auto-assign IP addresses.
- You can only have one internet gateway on a VPC.
- By default, every time you create a subnet in a VPC it will be associated with the `Main Route Table` of the VPC.
- Since your `Main Route Table` is the default table for new subnets, it's a good idea to _not_ have your default route table be internet facing.

### Questions

- What is the smallest and largest possible netmask sizes in AWS, how many addresses do they provide?
- How can you calculate the total amount of IP addresses within a CIDR Block using the netmask?
- How many IP addresses are reserved by AWS?
- Does AWS provide a default VPC with Subnets in each region? What are the default configurations for these resources?
- Which VPC (subnet) component can you use to block _specific_ IP addresses?
- Do EC2 instances in a default subnet have private and public IP addresses?
- Can subnets span multiple AZs?
- Is there a way to configure subnets, specifically public-facing ones, to auto-assign public IP addresses to instances set in them?
- How many internet gateways (IGWs) can you attach to a VPC?
- When you create a new subnet in a VPC, which route table does it automatically associate with? What security considerations arise from this?

---

## NAT Gateways

- You can use a NAT gateway so that instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.
- Each NAT gateway is created in a specific Availability Zone and implemented with redundancy in that zone.
- NAT gateways have a range of 5-45 Gbps.
- NAT gateways are managed, no need to patch.
- NAT gateways are not associated with Security Groups.
- NAT gateways are automatically assigned a public IP address.
- If you have resources in multiple AZs and they share a NAT gateway, in the event that the NAT gateway's AZ goes down, resources in other AZs will lose internet access.
- To create an AZ-independent architecture, create a NAT gateway in each AZ and configure your routing to ensure resources use the NAT gateway in the same AZ.
- **Public:** (Default) Instances in private subnets can connect to the internet through a public NAT gateway, but cannot receive unsolicited inbound connections from the internet.
- **Private:** Instances in private subnets can connect to other VPCs or your on-premises network through a private NAT gateway. You can route traffic from the NAT gateway through a transit gateway or a virtual private gateway. You cannot associate an elastic IP address with a private NAT gateway. You can attach an internet gateway to a VPC with a private NAT gateway, but if you route traffic from the private NAT gateway to the internet gateway, the internet gateway drops the traffic.

### Questions

- What is the purpose of a NAT gateway?
- What does it mean to say NAT gateways are redundant inside an AZ?
- What Gbps range do NAT gateways have?
- Do you need to patch and maintain your NAT gateways?
- Are NAT Gateways associated with security groups?
- Are NAT Gateways automatically assigned a public IP address?
- If a NAT Gateway's AZ goes down, what consequence happens to all the resources downstream in other AZs that use that NAT Gateway?
- How can you create a more durable architecture for your NAT Gateway - Resource configurations?
- NAT gateways have two connectivity types, what are the key differences?
- Can you connect an IG to a private NAT gateway, can traffic routed from the private NAT gateway pass through the IG?

---

## Security Groups

- Computers communicate through using protocols, and protocols are associated to ports. ie. port 22 is reserved for the SSH protocol. 443 for HTTPS, 80 for HTTP.
- SGs are essentially virtual firewalls for an EC2 (or other) instances. By default, everything is blocked.
- SGs are applied on a per-instance level.
- SGs are stateful, if you send a request from your instance, the response traffic is allowed to flow back in _regardless_ of inbound security group rules.

### Questions

- What are protocols? What ports do SSH, HTTP, HTTPS?
- What are Security Groups?
- What is the default SG allow/deny config?
- At what level are Security Groups applied?
- SGs are stateful, what does this mean?

---

## NACLs

- NACLs are the first line of defense and act as a firewall for one or more instances.
- NACLs can be used to block specific IP addresses.
- Subnets _can_ only be associated with one NACL at a time, yet NACLs can be associated with multiple subnets.
- NACLs have a **numbered list of rules** that are evaluated in order, starting at the lowest.
- NACLs have separate inbound and outbound rules. Each rule can either allow or deny traffic.
- NACLs are **stateless**; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).
- new NACLs deny everything (inbound/outbound) by default.
- the default NACL allows everything.

### Questions

- How many subnets can an NACL be applied to?
- How many NACLs can a subnet be associated with?
- Can NACLs be used to block specific IP addresses?
- How are NACL rules evaluated?
- NACLs are stateless, what does this mean? What is an example?
- What is the allow/deny configuration for newly created NACLs?
- What is the configuration for the VPC default NACL?

---

## VPC Endpoints

- VPC endpoints enabled you to _privately connect_ your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an Internet Gateway, NAT device, VPN connection, or AWS Direct Connect connection.
- VPC endpoints are virtual devices that are horizontally scaled, redundant, and highly available. They allow communication between instances in your VPC and services **without imposing availability risks or bandwidth constraints on your network traffic.**
- NAT gateways have a maximum amount of bandwidth if you have EC2 instances that are communicating with S3 for example, using a VPC endpoint may actually be better to due bandwidth limitations.
- **Interface Endpoints**: an interface endpoint is an Elastic Network Interface (ENI) with a private IP address that serves as an entry point for traffic headed to a supported service. They support a large number of AWS services.
- **Gateway Endpoints**: similar to NAT gateways, a gateway endpoint is a **virtual device you provision**. It supports connection to (currently) S3 and DynamoDB.

### Questions

- What purpose does a VPC endpoint have?
- Describe VPC endpoints?
- Do VPC endpoints impose availability risks or bandwidth constraints on your network traffic?
- VPC endpoints may be a better option for helping AWS services communicate amongst each other over NAT Gateways, why is that?
- What are the two types of VPC endpoints?
- What is an Interface Endpoint?
- What is a Gateway Endpoint?
- What does ENI stand for?

---

## PrivateLink

- You can open your VPC up to other VPCs by either opening it up to the internet using Internet Gateway (IGW) or using VPC peering.
- Opening a VPC up to the internet comes with some security considerations. There is a lot more to manage, IGW, Route tables, etc.
- Using VPC peering, you will have to create and manage many different peering relationships, including scaling.
- With peering, the whole network is accessible, this isn't great if you have multiple applications in your VPC.
- The best way to expose a service VPC to tens, hundreds, or thousands of customer VPCs is to use **PrivateLink**.
- PrivateLink doesn't require route tables, NAT gateways, Internet gateways, etc.
- PrivateLink requires a Network Load Balancer _on the service VPC_ and an ENI (Elastic Network Interface) _on the customer VPC_.

### Questions

- What approaches can you use to connect your VPC to other VPCs? Name 3.
- What are the two downsides of opening your VPC up to the internet?
- What are the downsides to using VPC peering?
- VPC peering makes your whole network accessible, why could this be an issue?
- What is the best way to expose a service VPC to tens, hundreds, or thousands, of customer VPCs?
- Does PrivateLink require route tables, NAT Gateways, Internet Gateways, etc?
- What does PrivateLink require on the service VPC?
- What does PrivateLink require on the customer VPC?

---

## VPC peering

- VPC peering allows you to use multiple VPCs for different environments and connect them together. Example: Production Web VPC, Content VPC, Intranet (backend) sales.
- VPC peering allows you to connect one VPC to another via direct network Route using **private IPv4 or IPv6 addresses.**
- Instances behave as if they were on the same private network.
- You can peer VPCs with other AWS accounts as well as with other VPCs in the same account.
- Peering is in a star configuration, e.g., one central VPC peers with 4 others. **No transitive peering!**
- You can peer between regions.
- You cannot communicate through the center star to another VPC, if you need to communicate with a sibling on another end of the star center, you need to create a direct peering connection between the two VPCs in question. No transitive peering means you cant talk through the middle connector VPC. **Must be the hub-and-spoke model.**
- You _cannot_ have overlapping CIDR address ranges between peering VPCs.
- Inter-Region VPC peering is when two VPCs communicate with one another across regions.
- VPC peering requires a "requester VPC" and an "accepter VPC", the requester VPC needs to send a request to the accepter VPC to establish a peering connection.
- VPC peering is a 1-to-1 connection between two VPCs.
- Edge-to-Edge routing does not exist for VPC peering. Conceptually, it's very similar to the restrictions on transitive peering. If one VPC uses a VPN tunnel to a corporate data center, a VPC peered to the tunneling VPC does not also have VPN access to the data center. The same concept applies to a VPC using IG to connect to the internet and VPCs using VPC endpoints.
- You can have a default of 50 active VPC peering connections, with a max quota of 125 active peering connections per VPC.
- You can have 25 outstanding VPC peering requests on your account.
- The Expiry time for an outstanding VPC request is 1 week or 168 hours.
- You cannot have more than one VPC peering connection between two VPCs.
- You cannot query the Amazon DNS server in a peer VPC.

### Questions

- Using peering, how can you connect one VPC to another?
- Do instances behave like they are on the same public or private network?
- What configuration does VPC peering follow?
- What is transitive peering? Does VPC peering allow it?
- Can you peer VPCs with VPCs on other accounts?
- How would you configure your VPC communication pattern to communicate between two VPCs directly if you can't use transitive peering? What model is this called?
- Can you have overlapping CIDR address ranges between peering VPCs?
- What is Inter-Region VPC peering?
- What is an accepter and requestor VPC?
- What kind of relationship is VPC peering?
- Explain three examples of Edge-to-Edge routing in relationship to VPC peering.
- What is the default amount of VPC peering connections you can have on a given VPC? What is the maximum quota?
- How many outstanding VPC peering requests can you have on your account?
- What is the expiry time for an outstanding VPC request?
- Can you have more than one VPC peering connection between two VPCs?
- Can you connect to the Amazon DNS in a peer VPC?

---

## VPN CloudHub

- VPN CloudHub, if you have multiple sites, each with its own VPN connection, you can use AWS VPN CloudHub to connect those sites together.
- CloudHub uses the hub-and-spoke model.
- Low cost and easy to manage.
- VPN CloudHub operates over the public internet, but all traffic between the customer gateway and the AWS VPN CloudHub is encrypted.

### Questions

- What problem does VPN CloudHub help to resolve?
- What topology model does VPN CloudHub use?
- What are some benefits of VPN CloudHub?
- Does VPN CloudHub operate over the internet? Is it safe?

---

## Direct Connect

- Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection between your on-premises and AWS.
- You can establish private connectivity between AWS and your data center or office.
- In many cases, you can reduce your network costs, increase bandwidth throughput, and provide a more consistent network experience than internet-based connections.
- Two types of Direct Connect Connection: Dedicated Connection, a physical ethernet connection associated with a single customer. Hosted Connection, a physical ethernet connection that an AWS Direct Connect Partner provisions on behalf of a customer.
- Dedicated Connection: customer requests a dedicated connection through the AWS Direct Connect console, the CLI, or the API.
- Hosted Connection: customers request a hosted connection by contacting a partner in the AWS Direct Connect Partner Program, who provides the connection.
- Direct Connect (DX) Locations are physical locations.
- VPNs vs Direct Connect, VPNs allow private communication, it traverses over the public internet to get the data delivered. While secure, it can be painfully slow.
- Direct Connect is very fast, secure, reliable, and able to take massive throughput. If the option is between setting a customer with a VPN or Direct Connect, DX is much more powerful.
- DX directly connects your data center to AWS.
- Useful for high-throughput workloads (e.g., lots of network traffic).
- Helpful when you need a stable and reliable internet connection.

### Questions

- What service does AWS Direct Connect provide?
- Is Direct Connect connectivity transmitted privately or publicly?
- What are 3 benefits that generally come in when you use AWS Direct Connect over internet-based connections?
- What are the two types of AWS Direct Connect connections?
- Describe AWS Dedicated Connection.
- Describe AWS Hosted Connection.
- What approaches are used to set up a dedicated connection versus a hosted connection?
- Are DX locations actual physical locations?
- What are the key differences between VPN and AWS Direct Connect connections?
- Do VPNs traverse over the public internet?
- What are the four key benefits of AWS Direct connect?

---

## Transit Gateway

- Transit Gateway, dramatically simplifies network topologies.
- Transit Gateway connects your on-premises networks through a central hub. This simplifies your network and puts an end to complex peering relationships. It acts as a cloud router -- each new connection is only made once.
- Allows you to have transitive peering between thousands of VPCs and on-premise data centers.
- Works with the hub-and-spoke model.
- Works on a regional basis, but you can have it across multiple regions.
- You can use it across multiple AWS accounts using RAM (Resource Access Manager.)
- You can use route tables to limit how VPCs talk to one another.
- Transit Gateway works with Direct Connect as well as VPN connections.
- Supports IP multicast (not supported by any other AWS service).

### Questions

- What does Transit Gateway dramatically simplify?
- How does Transit Gateway work? How does this simplify things?
- Does AWS Transit Gateway allow transitive peering between VPCs and on-premise data centers?
- What model does AWS Transit Gateway work with?
- Can AWS Transit Gateway work across multiple regions?
- Can you use AWS Transit Gateway across multiple AWS accounts? What does it use?
- What does RAM stand for?
- What can you use to limit how VPCs talk to one another over Transit Gateway?
- Does AWS Transit Gateway work with AWS Direct Connect and VPN connections?
- Does AWS Transit Gateway support IP multicast?
- What is IP multicast?

---

# Core Concept Review

### Questions

> Describe and explain the use-cases for each of the following.

- VPC
- Subnets
- CIDR Blocks
- Route Tables
- Security Groups
- NACLs
- VPC Peering
- VPC Endpoints
- NAT Devices
- Internet Gateways
- PrivateLink
- VPN CloudHub
- Direct Connect
- Transit Gateway
