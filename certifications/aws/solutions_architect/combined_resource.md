# Disaster Recovery

### Questions

1. For data storage, what is synchronous replication?
1. For data storage, what is asynchronous replication?
1. For data storage, what is quorum-based replication?
1. What is RTO?
1. What is RPO?
1. Describe Backup and Restore.
1. Describe Pilot Light.
1. Describe Warm Standby.
1. Describe Multi-Site.
1. Does AWS have a suggested disaster recovery tool?

### Answers

1. **Synchronous replication** only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node.
1. **Asynchronous replication** decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas.
1. **Quorum-based replication** combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.
1. RTO, or **Recovery Time Objective**, is _the time it takes_ after a disruption to restore a business process to its service level.
1. RPO, or **Recovery Point Objective**, is _the acceptable amount of data loss_ measured in time.
1. **Backup and Restore** involves taking frequent backups of your most critical systems. Once disaster strikes you simply restore these backups to recover data. B&R usually has the longest RTO and your RPO will depend on how frequently you backup your data.
1. **Pilot Light** (active-passive) provides quicker recovery time than backup and restore because core pieces of the system are already running and are continually kept up to date. Data loss is very minimal in this scenario for the critical parts, but for the other the less critical parts, you have the same RTO and RPO as backup and restore.
1. **Warm standby** (active-passive) provides a scaled-down version of a fully functional environment that is always running. For example, you have a subset of undersized servers and databases that have the same exact configuration as your primary, and are constantly updated also. Once disaster strikes, you only have to make minimal reconfigurations to re-establish the environment back to its primary state. Warm standby is costlier than Pilot Light, but you have better RTO and RPO.
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
1. XX What is a CloudTrail management event?
1. XX What is a CloudTrail data event?
1. Can a trail be applied to one region or all regions? What the best practice regarding this?
1. Where are events logged, where are events logged for global services?
1. CloudTrail, with multi-region trail enabled, only enables the tracking of regional services (EC2, S3, RDS, etc.) and not global services like IAM, CloudFront, WAF, R53, and so on. What do you need to do to track global services with multi-region CloudTrail?

### Answers

1. An event in CloudTrail is the record of activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
1. Management events and data events. By default, trails log management events, but not data events.
1. XX
1. XX
1. A trail can be applied to all regions or a single region. As a best practice, create a trail that applies to all regions in the AWS partition in which you are working. This is the default setting when you create a trail in the CloudTrail console.
1. For most services, events are recorded in the region where the action occurred. For global services such as AWS Identity and Access Management (IAM), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East (N. Virginia) Region.
1. In order to satisfy the requirement, you have to add the `--include-global-service-events` parameter in your AWS CLI command.

---

# KMS / SSE-C - Encryption related

- **Client-side encryption** is the act of encrypting data _before_ sending it to S3.
- There are two options to enable client-side encryption: **AWS-managed customer key** or use a **client-side master key.**
- When you use an AWS KMS-managed customer master key to enable client-side data encryption, _you provide an AWS KMS customer master key ID (CMK ID) to AWS._
- When you use a client-side master key, **for client-side data encryption**, your client-side master keys and your encrypted data are **_never_** sent to AWS.
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

- SSE: Request Amazon S3 to encrypt your object before saving it on disks in its data centers and then decrypt it when you download the objects.
- CSE: encrypting your data locally to ensure its security as it passes to the Amazon S3 service. The Amazon S3 service receives your encrypted data; it does not play a role in encrypting or decrypting it.
- Client-side encryption with a KMS-managed customer master key, you provide an AWS KMS customer master key ID (CMK ID) to AWS. AWS now has the master-key.
- S3 server-side encryption potentially results in unencrypted traffic being sent to AWS (if SSL/TLS isn't used). This is a data _in-transit_ vulnerability.\*
- When using S3 server-side encryption with a customer-provided key (SSE-C), **you actually provide the encryption key as part of your request to upload the object to S3.**
- You can use **SSL/TLS (Secure Socket Layer or Transport Layer Security)** or client-side encryption, to protect data in transit.

#### AWS Encryption SDK

- The AWS Encryption SDK is a client-side encryption library that is separate from the language-specific SDKs. You can use this encryption library to more easily implement encryption best practices in Amazon S3.
- Unlike the Amazon S3 encryption clients in the language-specific AWS SDKs, the AWS Encryption SDK is not tied to Amazon S3 and can be used to encrypt or decrypt data to be stored anywhere.

### Questions

1. Explain how client-side encryption and server-side encryption is handled in relation to AWS.
1. What are the **two** options for to enabling client-side encryption for AWS?
1. If you use AWS-KMS does AWS gain access to your master-key?
1. If you use SSE-C does AWS gain access to your master-key?
1. If you provide your client-side master key to S3 encryption client does AWS gain access to your key?
1. A client requires that you never store your master-key on AWS or have unencrypted data within AWS S3, how can you accomplish this?
1. What is the AWS S3 encryption upload flow?
1. What is the encrypted data-key's metadata key on the S3 object?
1. What is the AWS encrypted S3 download flow?
1. What is SSL?
1. What is an SSL certificate?
1. What is TLS?
1. What is the recommended port to use SSL/TLS over?
1. What is the most common well-known use of SSL/TLS related to secure web browsing?
1. Users visiting an HTTPS website can be assured of what 3 things?
1. Describe HTTPS Authenticity.
1. Describe HTTPS Integrity.
1. Describe HTTPS Encryption.
1. Using just the HTTP protocol, how is data sent?
1. What three ways can you protect data in-transit to AWS?

### Answers

1. With client-side encryption, data is encrypted locally on the client and then sent in an _encrypted_ manner to AWS. At no point does AWS ever have unencrypted versions of the data. With server-side encryption, your data is sent in a secure manner (most likely) over SSL or TLS, but then arrives in AWS in an unencrypted manner and AWS _then_ encrypts it before saving it to a disk.
1. You can either let AWS manage your keys using AWS-KMS, or you can manage the keys (_depending on the pattern you use_) locally using client-side encryption.
1. Yes. If you use KMS you would provide your master-key to AWS for them to include in their management solution (KMS).
1. Yes. If you use SSE-C, you provide your master-key to AWS so it can encrypt and decrypt your data for you in AWS.
1. No. S3 encryption client will _only_ use your master-key to encrypt the data-key/symmetric-key it generated to encrypt your object.
1. You can encrypt your data locally using AWS S3 encryption client. It will send the encrypted object to AWS S3 only with the metadata pointing back to which local master-key to use to decipher the encrypted data-key.
1. Generate a random data-key per object, use data-key to encrypt object, encrypt data-key with local master-key, upload encrypted object to S3 with the encrypted data-key as object metadata.
1. `x-amz-meta-x-amz-key`.
1. Download the object from S3, reference the object's metadata to determine which local master-key is used to decipher the encrypted data-key, decipher the data-key, use the deciphered data-key to decrypt the object data.
1. SSL, or **Secure Sockets Layer**, (the predecessor of TLS) is a protocol for establishing authenticated and encrypted links between networked computers.
1. An SSL certificate is a digital document that binds the identity of a website to a cryptographic key-pair consisting of a public and private key.
1. TLS, or **Transport Security Layer**, released in 1999 is the successor to SSL. It is also a protocol for authentication and encryption between computers.
1. SSL/TLS uses port `443` as the standard, and recommended, port.
1. The most common usage for secure web-browsing that uses the SSL/TLS protocol is the **HTTPS** protocol.
1. Authenticity, Integrity, and Encryption.
1. **Authenticity:** The server presenting the certificate is in possession of the private key that matches the public key in the certificate.
1. **Integrity:** Documents signed by the certificate (e.g. web pages) have not been altered in transit by a man in the middle.
1. **Encryption:** Communications between the client and server are encrypted.
1. HTTP websites send and receive data in plain text and is readily readily available to any eavesdropper with access to the data stream.
1. You can send data securely using SSL/TLS (HTTPS) protocols or encrypt it on client before sending it out.

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

---

# CloudWatch

### Questions

1. What is CloudWatch?
1. What kind of features can CloudWatch provide?
1. What are System Metrics?
1. What are Application Metrics?
1. What are Alarms?
1. What are the two types of metrics Cloudwatch provides?
1. Describe **Default Metrics**.
1. Describe **Custom Metrics**.
1. Is EC2 Memory Utilization a custom or default metric provided by AWS?
1. Are there default alarms in AWS?
1. Can AWS see past the hypervisor level for EC2 instances?
1. What is the standard reporting interval?
1. What tool does AWS provide that helps you monitor, store, and access log files from a variety of different sources?
1. What is a **Log Event?**
1. What is a **Log Stream?**
1. What are **Filter Patterns?**
1. What are **CloudWatch Logs Insights?**
1. What command do you enter in your EC2 instance to install the cloudwatch agent? Is this the unified agent for streaming logs and custom metrics?
1. Where can we store CloudWatch logs?
1. Whats the "go to" tool for looking at CloudWatch Logs?
1. What would we use for _real-time_ log monitoring?
1. Would you use CloudWatch to track that your instances are being setup with the right AMI?
1. If your metrics come in every 5-minutes, but your alarm is set to look for data every 1-minute, what will happen?

### Answers

1. CloudWatch is a monitoring and observability platform.
1. System Metrics, Application Metrics, Alarms.
1. **System Metrics** provides metrics that you get out of the box, the more managed the service the more information you will receive.
1. **Application Metrics** by installing the CloudWatch agent, you can get information from _inside_ your EC2 instances.
1. Alarms provide warnings based on metrics it tracks.
1. Default metrics and Custom metrics.
1. Default metrics are provided out of the box by AWS and no not require any additional work on your part to configure.
1. Custom metrics need to be provided by using the CloudWatch agent installed on the host.
1. Tracking your EC2 instance memory utilization is _actually_ a custom metric you define.
1. Any alarm you want needs to be created, AWS doesn't give you default alarms.
1. AWS cannot see past the hypervisor in your EC2 instances.
1. Standard interval is 5-minutes, detailed is every 1-minute. There is a small cost for detailed metrics.
1. Cloudwatch Logs.
1. A **Log Event** is a record of an event that provides a timestamp and the data.
1. A **Log Stream** is a collection of **log events** from the same source (the _same_ EC2 instance for example.
1. A **Log Group** is a collection of **log steams**. For example, you could group all of your Apache web server logs across hosts together.
1. You can look for specific terms in your logs using Filter Patterns. Think 400 errors in your web server logs.
1. **CloudWatch Logs Insights?** allows you to query all your logs using a _SQL-like_ interactive solution.
1. `sudo yum install amazon-cloudwatch-agent -y`. Yes, amazon-cloud-watch-agent is the unified agent for all AWS metrics and logs.
1. S3 can store CloudWatch logs.
1. CloudWatch Logs is considered the _go-to_ tool for referencing logs.
1. AWS Kinesis provides real-time log details.
1. No. CloudWatch is not the ideal tool to track appropriate AMIs or similar configurations, use **AWS Config** for that.
1. You will never receive data with that mismatch. You need to match the intervals.

---

## API Gateway

### Questions

- What status code do requests get when they're over the Gateway throttling limit?
- Can you run multiple version of the same API simultaneously?
- Generally, how does the throttling work for API Gateway to limit requests to backend systems?
- Are API endpoints HTTPS or HTTP?

#### Notes

- API Gateway provides throttling at multiple levels. For example, API owners can set a rate limit of 1,000 requests per second for a specific method in their REST APIs, and also configure Amazon API Gateway to handle a burst of 2,000 requests per second for a few seconds.

- API Gateway lets you run multiple versions of the same API simultaneously with **API Lifecycle**.

- API Gateway helps you manage traffic to your backend systems by allowing you to set throttling rules based on the number of requests per second for each HTTP method in your APIs.

- **All of the APIs created expose HTTPS endpoints only. API Gateway does not support unencrypted (HTTP) endpoints.**

---

## Amazon RDS

### Questions

- Where are the logs from RDS Enhanced Monitoring stored?
- What are two ways you can view data from RDS Enhanced Monitoring?
- What is the default amount of time EM logs are stored in CloudWatch?
- CloudWatch and Enhanced Monitoring collect metrics from two different levels of an RDS instance, what are they and what is the result?
- Can you install the CloudWatch agent on RDS instances for custom metrics?
- Are `CPU%` and `MEM%` metrics readily available on the RDS console?
- When using Multi-AZ deployment, are standby replicas synchronous or asynchronous?
- Are RDS read replicas synchronous or asynchronous?

#### Notes

- Amazon RDS provides metrics in real time for the operating system (OS) that your DB instance runs on.

- You can view the metrics for your DB instance using the **console**, or consume the **Enhanced Monitoring JSON output from CloudWatch Logs** in a monitoring system of your choice.

- Enhanced Monitoring requires permission to act on your behalf to send OS metric information to CloudWatch Logs.

- By default, Enhanced Monitoring metrics are **stored in the CloudWatch Logs for 30 days.**

- Take note that there are certain differences between CloudWatch and Enhanced Monitoring Metrics. CloudWatch gathers metrics about CPU utilization from the hypervisor for a DB instance, and Enhanced Monitoring gathers its metrics from an agent on the instance. As a result, you might find differences between the measurements, because the hypervisor layer performs a small amount of work.

- `CPU%` and `MEM%` metrics are not readily available in the Amazon RDS console

- The data provided by CloudWatch is not as detailed as compared with the Enhanced Monitoring feature in RDS. **Take note as well that you do not have direct access to the instances/servers of your RDS database instance**, unlike with your EC2 instances where you can install a CloudWatch agent or a custom script to get CPU and memory utilization of your instance.

- Using Amazon CloudWatch to monitor the CPU Utilization of your database is incorrect because although you can use this to monitor the CPU Utilization of your database instance, it does not provide the percentage of the CPU bandwidth and total memory consumed by each database process in your RDS instance.

- When you create or modify your DB instance to run as a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a **synchronous standby** replica in a different Availability Zone.

- **Read Replicas provides an asynchronous replication** instead of synchronous.

---

## EC2

### Questions

- What is the AWS Nitro System?
- Can EBS persist independently from an EC2 instance?
- Can an EC2 Instance Store persist independently from an EC2 instance?
- If you want an EBS volume with up to 64,000 IOPS, what type of EC2 instance do you have to launch to guarantee that?
- Data in the an instance store is lost under what three circumstances?
- What is Amazon EFS, what type of volume is it, can it provide 64,000 IOPS?
- What is the initial level scale-in policy?
- What is the AZ level scale-in policy?
- If there are multiple valid instances in an AZ, what is the next scale-in criteria?
- If there are multiple unprotected instances close to the next billing hour, which is deleted?

#### Notes

- The AWS Nitro System is the underlying platform for the latest generation of EC2 instances that enables AWS to innovate faster, further reduce the cost of the customers, and deliver added benefits like increased security and new instance types.

- **Amazon EBS** can persist independently from the life of an EC2 instance.

- Since the scenario requires you to have an EBS volume with up to 64,000 IOPS, you have to launch a Nitro-based EC2 instance, since the maximum IOPS and throughput (64,000 IOPS) is **only** guaranteed on instances built with the Nitro system provisioned with more than 32k IOPS. Other instances guarantee up to 32,000 IOPS only.

- Although an **Instance Store** is a block storage volume, it is not persistent and the data will be gone if the instance is restarted from the stopped state. (note that this is different from the OS-level reboot. In OS-level reboot, data still persists in the instance store).

- An **instance store** only provides temporary block-level storage for your instance. It means that the **data in the instance store can be lost if the underlying disk drive fails, if the instance stops, or if the instance terminates.**

- Although **Amazon EFS** can provide over 64,000 IOPS, this solution uses a file system and not a block storage volume.

**With the default termination policy, the behavior of the Auto Scaling group is as follows:**

1. If there are instances in multiple Availability Zones, choose the Availability Zone with the most instances and at least one instance that is not protected from scale in. If there is more than one Availability Zone with this number of instances, choose the Availability Zone with the instances that use the oldest launch configuration.

2. Determine which unprotected instances in the selected Availability Zone use the oldest launch configuration. If there is one such instance, terminate it.

3. If there are multiple instances to terminate based on the above criteria, determine which unprotected instances are closest to the next billing hour. (This helps you maximize the use of your EC2 instances and manage your Amazon EC2 usage costs.) If there is one such instance, terminate it.

4. If there is more than one unprotected instance closest to the next billing hour, choose one of these instances at random.

---

## Microsoft Active Directory

### Questions

- What is Microsoft Active Directory, what language is it implemented in?
- What does SAML stand for?
- What identity federation does AWS support that many idPs use?
- What do you need to implement SAML 2.0-Based Federation in AWS?
- What is the benefit of using SAML with AWS, what process does it simplify?

#### Notes

- Microsoft Active Directory implements **Security Assertion Markup Language (SAML)**, you can set up a SAML-Based Federation for API Access to your AWS cloud. In this way, you can easily connect to AWS using the login credentials of your on-premises network.

- **AWS supports identity federation with SAML 2.0, an open standard that many identity providers (IdPs) use.** This feature enables federated single sign-on (SSO), so users can log into the AWS Management Console or call the AWS APIs without you having to create an IAM user for everyone in your organization.

- By using SAML, you can simplify the process of configuring federation with AWS, because you can use the IdP's service instead of writing custom identity proxy code.

- Implement SAML 2.0-Based Federation by using a **Microsoft Active Directory Federation Service (AD FS)**. Before you can use SAML 2.0-based federation, you must configure your organization's IdP and your AWS account to trust each other. The general process for configuring this trust is described in the following steps. Inside your organization, you must have an IdP that supports SAML 2.0, like Microsoft Active Directory Federation Service (AD FS, part of Windows Server), Shibboleth, or another compatible SAML 2.0 provider.

- Setting up SAML 2.0-Based Federation by using a **Web Identity Federation** is primarily used to let users sign in via a well-known external identity provider (IdP), such as Login with Amazon, Facebook, Google. It does not utilize Active Directory.

---

## CloudFront

### Questions

- What can you use (2) to control access to content via CloudFront?
- If your users are using a client that doesn't support cookies, which access control should you use?
- If you want to restrict access to individual files, which access control should you use?
- If you want to provide access to multiple restricted files, which access control should you use?
- If you don't want to change your current URLs in order modify access rights, which access control should you use?
- What is CloudFront Match Viewer user for?
- What is CloudFront Field-Level Encryption user for?

#### Notes

- CloudFront **signed URLs** and **signed cookies** provide the same basic functionality: they allow you to control who can access your content.

_**Use signed URLs for the following cases:**_

1. You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions.

1. **You want to restrict access to individual files**, for example, an installation download for your application.

1. Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

_**Use signed cookies for the following cases:**_

1. **You want to provide access to multiple restricted files,** for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.

1. You don't want to change your current URLs.

- Use Signed Cookies to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required _Set-Cookie_ headers to the viewer which will unlock the content only to them.

- CloudFront **Signed URLs** are primarily used for providing access to individual files.

- The scenario explicitly says that they don't want to change their current URLs which is why implementing Signed Cookies is more suitable than Signed URL.

- CloudFront **Match Viewer** is an Origin Protocol Policy which configures CloudFront to communicate with your origin using HTTP or HTTPS, depending on the protocol of the viewer request. CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols.

- CloudFront **Field-Level Encryption** only allows you to securely upload user-submitted sensitive information to your web servers. It does not provide access to download multiple private files.

---

## Lambda

### Questions

- What type of events can RDS detect?
- Native functions and stored procedures can be used to capture data-modifying events?
- When working with RDSs' what kind of procedure, or event, is capable of triggering a lambda function?
- What service is Lambda@Edge like?
- Can Lambda@Edge be used to execute auth processes closer to users?
- What can you use, through CloudFront, to alleviate the 504 errors users are experiencing?
- Does Lambda require you to provision or manage servers, what do you pay for?
- Do you need to manage the scaling and availability of lambda?
- Can you call lambda functions directly from a web or mobile app?
- What is a function's concurrency?
- Can lambda be used to accommodate the handling of high-burst traffic for a limited time?
- API Gateway with ECS, Elastic Beanstalk, Autoscaling EC2 instances, can all take minutes to spin up resources. How fast is lambda?

#### Notes

> A car dealership website hosted in Amazon EC2 stores car listings in an Amazon Aurora database managed by Amazon RDS. Once a vehicle has been sold, its data must be removed from the current listings and forwarded to a distributed processing system.

_Create a native function or a stored procedure that invokes a Lambda function. Configure the Lambda function to send event notifications to an Amazon SQS queue for the processing system to consume._

- You can invoke an AWS Lambda function from an Amazon Aurora database with a native function or a stored procedure. This approach can be useful when you want to integrate your database running on Aurora MySQL with other AWS services.

- You can trigger a Lambda function whenever a listing is deleted from the database. You can then write the logic of the function to send the listing data to an SQS queue and have different processes consume it.

- **RDS events only provide operational events** such as DB instance events, DB parameter group events, DB security group events, and DB snapshot events. **What we need in the scenario is to capture data-modifying events (INSERT, DELETE, UPDATE) which can be achieved thru native functions or stored procedures.**

- Lambda@Edge lets you run Lambda functions to customize the content that CloudFront delivers, executing the functions in AWS locations closer to the viewer.

- In the given scenario, you can use Lambda@Edge to allow your Lambda functions to customize the content that CloudFront delivers and to execute the authentication process in AWS locations closer to the users.

- **You can set up an origin failover by creating an origin group with two origins with one as the primary origin and the other as the second origin which CloudFront automatically switches to when the primary origin fails.** This will alleviate the occasional HTTP 504 errors that users are experiencing.

- AWS Lambda lets you run code without provisioning or managing servers. **You pay only for the compute time you consume.** With Lambda, you can run code for virtually any type of application or backend service - all with zero administration.

- Just upload your code, and Lambda takes care of everything required to run and scale your code with high availability.

- You can set up your code to automatically trigger from other AWS services or call it directly from any web or mobile app.

- The first time you invoke your function, AWS Lambda creates an instance of the function and runs its handler method to process the event. When the function returns a response, it stays active and waits to process additional events. If you invoke the function again while the first event is being processed, Lambda initializes another instance, and the function processes the two events concurrently.

- As more events come in, Lambda routes them to available instances and creates new instances as needed. When the number of requests decreases, Lambda stops unused instances to free up the scaling capacity for other functions.

- **Your function's concurrency is the number of instances that serve requests at a given time.**

- For an initial burst of traffic, your function's cumulative concurrency in a Region can reach an initial level of between 500 and 3000, which varies per Region.

- The first requirement (to handle the bursts of traffic to the example website) is to create a solution that will allow the users to access the data using an API. To implement this solution, you can use Amazon API Gateway. The second requirement is to handle the burst of traffic within seconds. **You should use AWS Lambda in this scenario because Lambda functions can absorb reasonable bursts of traffic for approximately 15-30 minutes.**

- Under the hood, Lambda can run your code to thousands of available AWS-managed EC2 instances (that are already running) within seconds to accommodate traffic. This is faster than the Auto Scaling process of launching new EC2 instances that could take a few minutes or so.

---

## Amazon FSx for Windows File Server

### Questions

- What is Amazon FSx for Windows File Server?
- What does EFS run as its file sharing protocol?
- What does FSx run as its file sharing protocol?
- What does NFS stand for?
- What does SMB stand for?
- What Operating systems can FSx run on?
- What Operating systems can EFS run on?
- Can thousands of compute systems access FSx concurrently?
- Can you change your AD configuration after creating it? What is the work around?
- Is Amazon EFS compatible with windows?

#### Notes

- **Amazon FSx for Windows File Server provides fully managed, highly reliable, and scalable file storage that is accessible over the industry-standard Service Message Block (SMB) protocol.** It is built on Windows Server, delivering a wide range of administrative features such as user quotas, end-user file restore, and Microsoft Active Directory (AD) integration

- Amazon FSx works with Microsoft Active Directory to integrate with your existing Microsoft Windows environments.

- Take note that after you create an Active Directory configuration for a file system, you can't change that configuration. However, you can create a new file system from a backup and change the Active Directory integration configuration for that file system.

- These configurations allow the users in your domain to use their existing identity to access the Amazon FSx file system and to control access to individual files and folders.

- **Amazon EFS does not support Windows systems, only Linux OS.** You should use Amazon FSx for Windows File Server instead to satisfy the requirement in the scenario (scenario being a company plans to migrate its on-premises workload to AWS. The current architecture is composed of a Microsoft SharePoint server that uses a Windows shared file storage.)

- Using a **Network File System (NFS)** file share using AWS Storage Gateway is incorrect because **NFS file share is mainly used for Linux systems.** Remember that the requirement in the scenario is to use a Windows shared file storage. Therefore, you must use an SMB file share instead, which supports Windows OS and Active Directory configuration.

- Amazon EFS is a managed NAS filer for EC2 instances based on **Network File System (NFS)** version 4.

- FSx for Windows, on the other hand, is a managed Windows Server that runs Windows **Server Message Block (SMB)**-based file services.

- NFS is one of the first network file sharing protocols native to Unix and Linux.

- Some Windows applications might not work on EFS or be feature-complete without access to a native Windows SMB file share.

**- FSx It is accessible from Windows, Linux, and macOS compute instances and devices that support the SMB protocol.**

- Thousands of compute instances and devices can access a file system concurrently.

- **Amazon EFS only supports Linux workloads.**

---

## EBS volumes

### Questions

- SSD-backed volumes are ideal for what kind of operations?
- HDD-backed volumes are ideal for what kind of operations?
- Which volume can be used as a bootable volume: SSD or HDD?
- Which type of SSD is more suitable for workloads on databases like MongoDB, Oracle, MySQL, etc?

#### Notes

- SSD-backed volumes, such as General Purpose SSD (gp2) and Provisioned IOPS SSD (io1), **deliver consistent performance whether an I/O operation is random or sequential.**

- HDD-backed volumes like Throughput Optimized HDD (st1) and Cold HDD (sc1) deliver optimal performance only when **I/O operations are large and sequential.**

- **SSD can be used as a bootable volume, HDD cannot.**

- **Provisioned IOPS SSD volumes are much more suitable, than general purpose SSDs (gp2), in meeting the needs of I/O-intensive database workloads such as MongoDB, Oracle, MySQL, and many others.**

---

## Storage Gateway

### Questions

- What is Storage Gateway, what does it help solve?
- Does Storage Gateway support NFS and SMB protocols?
- Where and how is the Storage Gateway deployed?
- What is a tape gateway?
- Can you use direct connect with AWS storage gateway?
- What is AWS Snowmobile? Is it suitable for creating hybrid architecture?

#### Notes

> A file gateway supports a file interface into Amazon Simple Storage Service (Amazon S3) and combines a service and a virtual software appliance. By using this combination, you can store and retrieve objects in Amazon S3 using industry-standard file protocols such as **Network File System (NFS) and Server Message Block (SMB)**. The software appliance, or gateway, is deployed into your on-premises environment as a virtual machine (VM) running on VMware ESXi, Microsoft Hyper-V, or Linux Kernel-based Virtual Machine (KVM) hypervisor.

- **AWS Storage Gateway is a hybrid cloud storage solution that allows companies to take advantage of cloud services and migrate their on-prem applications to the cloud as appropriate per performance, compliance, or IT reasons.**

- A file gateway supports a file interface into S3 and combines a service and a virtual software appliance. **By using this combination, you can store and retrieve objects in Amazon S3 using industry-standard file protocols such as Network File System (NFS) and Server Message Block (SMB).**

- The software appliance, or gateway, is deployed into your on-premises environment as a virtual machine (VM).

- **tape gateways** provide cost-effective and durable archive backup data in Amazon Glacier, it does not meet the criteria of being retrievable immediately within minutes. It also doesn't maintain a local cache that provides low latency access to the recently accessed data and reduce data egress charges.

- **You can use AWS Direct Connect with AWS Storage Gateway to create a connection for high-throughput workload needs, providing a dedicated network connection between your on-premises file gateway and AWS.**

- **Snowmobile** is mainly used to migrate the _entire data of an on-premises data center to AWS_. This is not a suitable approach as the company still has a hybrid cloud architecture which means that they will still use their on-premises data center along with their AWS cloud infrastructure.

---

## AWS WAF

### Questions

- What does WAF stand for?
- What are some AWS services that AWS WAF is tightly integrated with? Name 4.
- When you use AWS WAF with CloudFront, are your rules distributed globally to all edge locations?
- What are the two types of rules for the WAF web ACL?
- What is a rate-based rule and what, what is their time span?
- How is a regular rule different from a rate-based rule?
- Are NACLs able to block incoming requests from a single IP that is dynamically changing?
- Can security groups deny traffic?

#### Notes

> The website is receiving a large number of illegitimate external requests from multiple systems with IP addresses that constantly change. To resolve the performance issues, the Solutions Architect must implement a solution that would block the illegitimate requests with minimal impact on legitimate traffic.

- **AWS WAF**, or **Web Application Firewall**, is tightly integrated with Amazon CloudFront, the Application Load Balancer (ALB), Amazon API Gateway, and AWS AppSync – services that AWS customers commonly use to deliver content for their websites and applications.

- **When you use AWS WAF on Amazon CloudFront, your rules run in all AWS Edge Locations,** located around the world close to your end-users. This means security doesn’t come at the expense of performance.

- Blocked requests are stopped before they reach your web servers.

- When you use AWS WAF on regional services, such as Application Load Balancer, Amazon API Gateway, and AWS AppSync, your rules run in the region and can be used to protect Internet-facing resources as well as internal resources.

- A **rate-based rule** tracks the rate of requests for each originating IP address and triggers the rule action on IPs with rates that go over a limit. You set the limit as the number of requests per 5-minute time span.

- AWS WAF web ACL. **There are two types of rules in creating your own web ACL rule:** **regular** and **rate-based rules**.

- A **regular rule** only matches the statement defined in the rule. If you need to add a rate limit to your rule, you should create a rate-based rule.

- Although NACLs can help you block incoming traffic, they wouldn't be able to limit the number of requests from a _single IP address that is dynamically changing._

- **A security group can only allow incoming traffic.** You can't **deny traffic using security groups**. In addition, it is not capable of limiting the rate of traffic to your application unlike AWS WAF.

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
