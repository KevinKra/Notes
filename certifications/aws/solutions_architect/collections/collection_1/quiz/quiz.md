# Services: Disaster Recovery, EC2, IAM, S3, VPC

## Disaster Recovery

- For data storage, what is synchronous, async, and quorum-based replication?
- What is RTO?
- What is RPO?
- Describe Backup and Restore.
- Describe Pilot Light.
- Describe Warm Standby.
- Describe Multi-Site.
- Does AWS have a suggested disaster recovery tool?

## EC2 Auto Scaling Group

- Conceptually, what is an auto-scaling group?
- What are some benefits of using Auto Scaling Group?
- What is an EC2 Auto Scaling group **Lifecycle Hook?**
- What are the four EC2 Auto Scaling Group options?
- What are the three EC2 Auto Scaling Group Policy Types?
- What is an EC2 Auto Scaling Group **cooldown period?** What is the only policy type that has a cooldown period?
- Describe Target Tracking Scaling.
- Describe Step Scaling.
- Describe Simple Scaling.
- The dynamic scaling options: step and simple scaling, have what similarities and differences?
- What is the primary issue with simple scaling?
- Do Target Tracking or Step Scaling need to wait for the cooldown period to complete before initiating additional scaling activities?
- What is scale-in and scale-out?
- What do you need to determine when you configure automatic scale in rules?
- What can you use to prevent specific instances from being terminated during automatic scale-ins?
- What does a **launch template** define?
- What does a **launch configuration** define?

## S3

- How much storage does AWS S3 provide?
- What is the largest object file size for S3?
- What response do you get when an upload to S3 is successful?
- What are the S3 Classes/Tiers?
- How many AZ locations are used (excluding one-zone IA) to store S3 data?
- What is the durability of all S3 storage classes?
- What are the retrieval time ranges for S3 Glacier and S3 Glacier Deep-Archive?
- What is S3 lifecycle management?
- What is S3 versioning?
- Can versioning be used with lifecycle management?
- Once you setup versioning for at the bucket level, can it be removed or suspended?
- Are deleted S3 objects still stored with S3 versioning?
- If you set an S3 bucket to public, do all previous objects within the bucket become public?
- What is S3 Object Lock?
- What is the WORM model?
- Can S3 Objects be locked indefinitely?
- Which is more strict, S3 Governance mode or Compliance mode?
- Where is information regarding an S3 object's retention period stored?
- After the retention period ends, what can be used to prevent an object from being deleted or changed?
- Do legal holds have a retention periods?
- What is a Glacier Vault lock policy?
- After a Glacier Vault lock policy has been set (24hrs after init), can you change or remove it?
- Who manges the keys in these formats: SSE-S3, SSE-KMS, SSE-C.
- What are S3 prefixes and how can they be used to optimize S3 performance?
- What is S3 multipart uploading, at what file size is it recommended and required?
- What is S3 replication, what did the service used to be called?
- What has to be enabled on the both the source and destination buckets for versioning to work?
- Can the source and destination buckets be different S3 storage classes when using versioning?

## VPC

- What is the smallest and largest possible netmask sizes in AWS, how many addresses do they provide?
- How can you calculate the total amount of IP addresses within a CIDR Block using the netmask?
- How many IP addresses are reserved by AWS?
- What are the general configurations of the default VPCs provided by AWS?
- Which VPC (subnet) component can you use to block _specific_ IP addresses?
- Can subnets span multiple AZs?
- What is the purpose of a NAT gateway, are they managed?
- What Gbps range do NAT gateways have?
- If a NAT Gateway's AZ goes down, what consequence happens to all the resources downstream _in other_ AZs that use that NAT Gateway?
- NAT gateways have two connectivity types, what are the key differences?
- If you route traffic from a _private_ NAT gateway to the IGW, what happens?
- What are protocols? What ports do SSH, HTTP, HTTPS use?
- What are Security Groups?
- SGs are stateful, what does this mean?
- Can subnets have multiple NACLs?
- How are NACL rules evaluated?
- NACLs are stateless, what does this mean? What is an example?
- What does a VPC endpoint provide?
- VPC endpoints may be a better option for helping AWS services communicate amongst each other over NAT Gateways, why is that?
- What is an Interface Endpoint?
- What is a Gateway Endpoint?
- What does ENI stand for?
- What is the best way to expose a service VPC to tens, hundreds, or thousands, of customer VPCs?
- Does PrivateLink require route tables, NAT Gateways, Internet Gateways, etc?
- What does PrivateLink require on the service VPC?
- What does PrivateLink require on the customer VPC?
- Using peering, how can you connect one VPC to another?
- What configuration does VPC peering follow?
- What is transitive peering? Does VPC peering allow it?
- Can you peer VPCs with VPCs on other accounts?
- Can you have overlapping CIDR address ranges between peering VPCs?
- What is an accepter and requestor VPC?
- Is there a limit to the amount of peers a VPC can have? What's the amount?
- How many outstanding peering connections can you have on your account?
- Can you connect to the Amazon DNS in a peer VPC?
- What problem does VPN CloudHub help to resolve?
- Does VPN CloudHub operate over the public internet? Is the traffic encrypted?
- What is Direct Connect?
- What is a Dedicated Connection?
- What is a Hosted Connection?
- Which is faster, VPNs or Direct Connect?
- What is Transit Gateway?
- Does Transit Gateway provide transitive peering between VPCs?
