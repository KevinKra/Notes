# Solutions Architect Guide

---

# AWS Fundamentals

## Availability Zones and Regions

- **Region**: a geographical area composed of multiple (2 or more) Availability zones (AZs).
- **Availability Zones**: one or more discrete data centers, each with redundant power, networking, and connectivity, houses in separate facilities (2 or more Data centers per AZ) all of which are all within 100km of each other.
- **Data Centers**: buildings full of servers providing the AWS backbone infrastructure.
- **Edge Locations**: endpoints for AWS that are used for caching content. Typically consists of **Cloudfront** (Amazon's CDN). There are _many_ more edge locations than there are regions.

## Shared Responsibility Model

**Customer**: Responsible for security _in_ the Cloud.

- Customer Data
- Platform, App, IAM
- OS, Network, and Firewall configurations
- Client-side data encryption, Data integrity
- Server-side Encryption
- Network Traffic Protection (Encryption, Integrity, Identity)

**AWS**: Responsible for security _of_ the Cloud.

- Software
- Compute
- Storage
- Database
- Networking
- Hardware / AWS Global Infrastructure
- Regions
- Availability Zones
- Edge Locations

#### In a nutshell

If you can control it (security groups, IAM, patching EC2 operating systems, ect), you are responsible. If it's something you can't really manage (security cameras, cameras, patching RDS operating system), it's probably AWS's responsibility. **Encryption is a shared responsibility**, you have to enact it, but AWS also has to ensure it works.

## Compute, Storage, Databases, and Networking

### Compute

**EC2**:

**Lambda**:

**Elastic Beanstalk**:

### Storage

**S3**:

**EBS**:

**EFS**:

**FSx**:

**Storage Gateway**:

### Databases

**RDS**:

**DynamoDB**:

**Redshift**:

### Networking

**VPCs**:

**Direct Connect**:

**Route 53**:

**API Gateway**:

**AS Global Accelerator**:

### 5 Pillars of the Well-Architected Framework

- **Operational Excellence**: running and monitoring systems to deliver business value, and continually improving processes and procedures.
- **Security**: protecting information and systems.
- **Reliability**: ensuring workload performs its intended function correctly and consistently.
- **Performance Efficiency**: using IT and computing resources efficiently.
- **Cost Optimization**: avoiding unnecessary costs.

---

# IAM

> "Identity Access Management" allows you to manage users and their level of access to the AWS console

- IAM is a **Global Service**.

## Root Account

- Email address used to sign up for AWS.
- Has **full admin access** to AWS.

#### How to secure the Root Account

- Enable MFA.
- Create Admin group for administrators, assign appropriate permissions to group.
- Create user accounts for your admins.
- Add users to the admin group.

### Policy Documents

- Written in **JSON**.
- Easiest approach for establishing uniform permissions is by attaching policies to groups and then assigning users to the groups.
- **Managed Policies** are policies created _and managed_ by AWS.
- Allow statements **DO NOT** override explicit Deny statements.

```
// policy example:
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

## IAM Management

- **User**: an account bound to unique users.
- **Groups**: organizing users, ie. admins, developers, HR, ect.
- **Roles**: used by AWS services to grant access to other services. Example: S3 communicating with ECS.

IAM users are considered "permanent" because user credentials don't automatically rotate, making them "permanent" without manual human interaction.

You can create roles and attach users to them. **STS (Security Token Service)** allows the user to be attached to the role within the AWS console. The user(s) can then _temporarily assume the role_ to have access to the provided permissions provided by the role.

### Exam Tips:

- IAM is a global service.
- Access Key ID and Secret Access Keys are not the same as usernames and passwords.
- You only can view your Secret Access Key once, save it in a secure location.
- Always setup password rotations.
- IAM Federation, you can combine existing user accounts with AWS. For example, when you login using your PC (using Microsoft Active Directory), you can use the same credentials to log into AWS if you've setup federation.
- Identity Federation: uses SAML standard, which is Active Directory.

## Questions:

- What language are AWS policies written in?
- What is the easiest way to establish uniform policies across an organization?
- What are managed policies? What naming identifier do these policies share?
- Do allow statements override explicit default statements?
- What is the structure of a generic policy?
- What are the three IAM entities? Explain each.
- IAM users are considered permanent, what does this mean?
- What can you use to create roles and attach them to users to grant them temporary permissions?
- How many times can a user view their secret access key? What has to be done if it's lost or forgotten?
- What is IAM Federation and what standard does it use?

---

## S3

> "Simple Storage Solution" provides object storage for static files (media, images, code, etc.) on AWS.

- Unlimited Storage.
- Objects up to 5TB in Size.
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
