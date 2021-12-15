# AWS SA Architect Guide 

### Q

#### Five Pillars of a Well Architected Framework.

1. What are the five pillars of a "Well-Architected Framework"?
1. Describe the pillar Operation Excellence, what are it's design principles?
1. Describe the pillar Security, what are it's design principles?
1. Describe the pillar Reliability, what are it's design principles?
1. Describe the pillar Performance Efficiency, what are it's design principles?
1. Describe the pillar Cost Optimization, what are it's design principles?

#### Best Practices when Architecting in the Cloud

1. What is a **Data Lake?**
1. For data storage, what is the difference between Synchronous, Asynchronous, and Quorum-based replication?

#### Disaster Recovery in AWS

1. What is RTO?
1. What is RPO?
1. Describe Backup and Restore.
1. Describe Pilot Light.
1. Describe Warm Standby.
1. Describe Multi-Site.
1. Does AWS have a suggested disaster recovery tool?

---

### A

#### Five Pillars of a Well Architected Framework.

1. Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization.
1. **Operational Excellence** is the ability to support development and run workloads effectively, gain insight into their operations, and to continuously improve supporting processes and procedures to deliver business value. _Design principles: Perform operations as code, make frequent and small reversible changes, refine operation procedures frequently, learn from operational failures._
1. **Security** is the ability to protect data, systems, and assets to take advantage of cloud technologies to improve your security. _Design principles: implement a strong identity federation, enable traceability, apply security at all layers, automates security best practices, protect data in transit and at rest, keep people away from data, prepare for security events._
1. **Reliability** is the ability of a workload to perform its intended function correctly and consistently when it’s expected to. This includes the ability to operate and test the workload through its total lifecycle. _Design principles: automate recover from failure, test recovery procedures, scale horizontally to increase aggregate workload availability, stop guessing capacity, manage change in automation._
1. **Cost Optimization** the ability to run systems to deliver business value at the lowest price point. _Design principles: implement Cloud Financial Management, adopt a consumption model, measure overall efficiency, stop spending money on undifferentiated heavy lifting, analyze and attribute expenditure._

#### Best Practices when Architecting in the Cloud

1. A **Data Lake** is an architectural approach that allows you to store massive amounts of data in a central location so that it's readily available to be categorized, processed, analyzed, and consumed by diverse groups within your organization.
1. **Synchronous replication** only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node. **Asynchronous replication** decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas. **Quorum-based replication** combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.

#### Disaster Recovery in AWS

1. RTO, or **Recovery Time Objective**, is _the time it takes_ after a disruption to restore a business process to its service level.
1. RPO, or **Recovery Point Objective**, is _the acceptable amount of data loss_ measured in time.
1. **Backup and Restore** involves taking frequent backups of your most critical systems. Once disaster strikes you simply restore these backups to recover data and quickly. B&R usually has the longest RTO and your RPO will depend on how frequently you backup your data.
1. **Pilot Light** provides quicker recovery time than backup and restore because core pieces of the system are already running and are continually kept up to date. Data loss is very minimal in this scenario for the critical parts, but for the other the less critical parts, you have the same RTO and RPO as backup and restore.
1. **Warm standby** provides a scaled-down version of a fully functional environment that is always running. For example, you have a subset of undersized servers and databases that have the same exact configuration as your primary, and are constantly updated also. Once disaster strikes, you only have to make minimal reconfigurations to re-establish the environment back to its primary state. Warm standby is costlier than Pilot Light, but you have better RTO and RPO.
1. **Multi-Site** runs exact replicas of your infrastructure in an active-active configuration. In this scenario, all you should do in case of a disaster is to reroute traffic onto another environment. Multi-site is the most expensive option of all since you are essentially multiplying your expenses with the number of environment replicas. It does give you the best RTO and RPO however.
1. Yes, AWS promotes their disaster recovery tool called **CloudEndure** which they are suggesting to their customers as the preferred solution for disaster recovery workloads.


---

# EC2 / Application Auto Scaling Group

https://tutorialsdojo.com/aws-auto-scaling/?src=udemy#features

1. What is an **Auto Scaling Group?**

- An Auto Scaling group contains a collection of Amazon EC2 instances treated as a logical grouping for the purposes of automatic scaling and management. n Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service. The size of an Auto Scaling group depends on the number of instances that you set as the desired capacity. You can adjust its size to meet demand, either manually or by using automatic scaling.

1. What are the two policies for an Auto Scaling Group? Describe their similarities and differences.

- **Step Scaling policies** and **Simple Scaling policies**. Both require CloudWatch alarms for the scaling policies and both require high and low thresholds for the alarms. Additionally, both require you to define whether to add or remove instances, how many, or set the group to an exact size.

- **The main difference between the policy types is the step adjustments that you get with step scaling policies.** When step adjustments are applied, and they increase or decrease the current capacity of your Auto Scaling group, the adjustments vary based on the size of the alarm breach.

#### Target Tracking Scaling

- With a target tracking scaling policy, you can increase or decrease the current capacity of the group **based on a target value for a specific metric.** This policy will help resolve the _over-provisioning_ of your resource. **The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value.** In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern.

#### Simple Scaling

- **Simple scaling requires you to wait for the cooldown period to complete before initiating additional scaling activities.** Target tracking or Step Scaling policies can trigger scaling activity immediately without waiting for the cooldown period to expire.

## EC2 Auto Scaling Group

### Q

1. Conceptually, what is an auto scaling group?
1. What are some benefits of using Auto Scaling Group?
1. What is a EC2 Auto Scaling group **Lifecycle Hook?**
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
1. What can you use to prevent specific instances from being terminated during automatic scale ins?
1. What are **launch templates?**
1. What is a **launch configuration?**

### A

1. An auto scaling group is a collection, or group, of EC2 instances that share a common intention and design.
1. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies.
1. A Lifecycle hook allows your auto scaling group to perform custom actions when instances launch or terminate.
1. Scale to maintain current instance levels, manual scaling, scale based on a schedule, and scale based on a demand.
1. Target Tracking Scaling, Step Scaling, and Simple Scaling.
1. A Cooldown period is a _configurable_ setting that helps ensure to not launch or terminate additional instances before previous scaling activities take effect.
1. **Target tracking scaling**—Increase or decrease the current capacity of the group based on a target value for a specific metric.
1. **Step scaling**—Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as **step adjustments**, that vary based on the size of the alarm breach.
1. **Simple scaling**—Increase or decrease the current capacity of the group based on a single scaling adjustment.
1. Both require you setup CloudWatch alarms for scaling policies, specify high and low thresholds for the alarms, whether to add or remove instances, how many, or to set an exact size. The main difference is step adjustments increase the capacity of the group, in a varying manner, based on the size of the alarm breach.
1. The primary issue with simple scaling is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms.
1. No, Target tracking or step scaling policies can trigger a scaling activity immediately without waiting for the cooldown period to expire.
1. If the instance is in a state other than running, the system status is impaired, or ELB reports the instance failed health checks.
1. Scaling in is reducing instances, scaling out is increasing instances.
1. You need to set a **termination policy** to determine which instances to delete first.
1. You can use **instance protection** to prevent instances from being terminated during automatic scale in.
1. Launch templates specify instance configuration information when you launch EC2 instances, and they allow you to have multiple versions of a template.
1. A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances, and you specify information for the instances.

## Application Auto Scaling

### Q

1. What are the three approaches to Application Auto Scaling?
1. Explain Target tracking scaling. How does it differ from the similarly named EC2 Scaling solution of the same name?
1. What does Schedule Scaling use to scale resources?
1. Can you have multiple target tracking policies for a scalable target?

### A

1. Target tracking scaling, Step scaling, and Scheduled scaling.
1. Application Target scaling scales a resource based on a target value for a specific CloudWatch metric. Unlike EC2 Target tracking, Application Target Tracking uses data points derived from CloudWatch to trigger scaling events.
1. Scheduled Scaling uses dates/time to determine scaling.
1. Yes, you can have multiple target tracking policies for a scalable target, provided they each use different metrics.

---

## Monitoring

### Q

1. Does autoscaling provide health checks on instances in standby state?
1. What does CloudWatch metrics enable?

### A

1. Nope. Standby state can be used for performing updates/changes/troubleshooting without health checks being performed or replacement instances being launched.
1. CloudWatch metrics enables you to retrieve statistics about Auto Scaling-published data points as an ordered set of time-series data, known as metrics. You can use these metrics to verify that your system is performing as expected.

# CloudTrail

### Q

1. What is an **event** in CloudTrail?
1. What are the two types of events that can be logged in CloudTrail?
1. Can a trail be applied to one region or all regions? What the best practice regarding this?
1. Where are events logged, where are events logged for global services?
1. CloudTrail, with multi-region trail enabled, only enables the tracking of regional services (EC2, S3, RDS, etc.) and not global services like IAM, CloudFront, WAF, R53, and so on. What do you need ot to to track global services with multi-region CloudTrail?

### A

1. An event in CloudTrail is the record of an activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
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
- S3 server-side encryption potentially results in unencrypted traffic being sent to AWS (if SSL/TLS isn't used). This is a data _in-transit_ vulnerability.*
- When using S3 server-side encryption with a customer-provided key (SSE-C), **you actually provide the encryption key as part of your request to upload the object to S3.**
- You can use **SSL/TLS (Secure Socket Layer or Transport Layer Security)** or client-side encryption, to protect data in transit.

#### AWS Encryption SDK

- The AWS Encryption SDK is a client-side encryption library that is separate from the language–specific SDKs. You can use this encryption library to more easily implement encryption best practices in Amazon S3.
- Unlike the Amazon S3 encryption clients in the language–specific AWS SDKs, the AWS Encryption SDK is not tied to Amazon S3 and can be used to encrypt or decrypt data to be stored anywhere.