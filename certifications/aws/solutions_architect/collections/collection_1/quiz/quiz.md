# Services: Disaster Recovery, EC2, IAM, S3, VPC

## Disaster Recovery

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

## EC2 Auto Scaling Group

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

## S3

1. How much storage does AWS provide?
1. What is the largest object file size for S3?
1. Where are files stored?
1. What does 'universal namespace' mean?
1. What response do you get when an upload to S3 is successful?
1. Is data stored redundantly in multiple AZs on AWS (non-1ZIA) S3?
1. What are the main parts of an S3 URL?
1. What are the four parts of an S3 Object?
1. What are the S3 Classes/Tiers?
1. How many AZ locations are used (excluding one-zone IA) to store S3 data?
1. What is the default storage class?
1. What is the availability of S3-IA?
1. What is the availability of S3-OZIA?
1. What is the durability of all S3 storage classes?
1. What are the retrieval time ranges for S3 Glacier and S3 Glacier Deep-Archive?
1. What is lifecycle management? Can it be used with versioning?
1. Once enabled, can versioning be disabled?
1. Can versioning be integrated into lifecycle rules?
1. Can versioning support MFA?
1. Are deleted objects still stored with S3 versioning?
1. If a bucket policy sets all objects to public, do previously versioned objects automatically become public?
1. With versioning, how would you restore a deleted object?
1. What is S3 Object Lock?
1. What is the WORM model?
1. Can S3 Object let be set to a fixed period or indefinitely?
1. What are two scenarios where S3 Object Lock can be useful?
1. Can S3 Object Lock be applied on buckets and on a per-object level?
1. What is the S3 Object Lock Governance mode?
1. What is the S3 Object Lock Compliance mode?
1. What is the difference between Governance and Compliance mode?
1. What is the retention period, is there metadata stored to indicate it?
1. What can you add to prevent an S3 object / bucket to prevent it from deleted or overwritten after the retention period expires?
1. What is the key difference between a legal hold and a retention period?
1. What permission is required for a user to replace or remove legal hold?
1. What is a Glacier Vault Lock policy?
1. Once locked, can a policy be changed on an S3 Glacier vault?
1. What are two formats of encryption in-transit?
1. What are two types of encryption at rest?
1. What are the three types server-side of encryption at rest provided through AWS?
1. What are two approaches for enforcing server-side encryption?
1. If a file is to be encrypted at upload time what parameter is included in the header? What are the two variants that this parameter header can be?
1. How can you create bucket-level policies to enforce encryption?
1. What are S3 prefixes? What are they defined by?
1. What is an S3 prefix example?
1. How are prefixes leveraged to optimize S3 performance?
1. How quickly can you get the first bytes from S3?
1. What are the PUT/POST/DELETE and GET request rates?
1. How does using SSE-KMS limit S3 performance?
1. What methods are called on KMS download and upload?
1. Are KMS request rates region specific? What are the limits to requests per-second?
1. Can you request KMS quota requests?
1. If your concerned about hitting the KMS limit, what can you use instead?
1. What are multipart uploads, do they increase efficiency?
1. What file sizes are multipart uploads recommended for?
1. What file sizes are multipart uploads required for?
1. What are S3 Byte-Range fetches?
1. Can S3 Byte-Range fetches download just parts of a file?
1. Whats the key difference between Multipart uploads and S3 Byte-Range fetches?
1. What did S3 Replication used to be called?
1. Does S3 versioning need to be enabled on both the source and destination buckets?
1. Are Objects, that already exist on a Bucket, automatically replicated once replication is turned on?
1. Will subsequent objects be replicated?
1. Are delete markers replicate by default?
1. Will deleting individual versions or delete markers be replicated across buckets?
1. Can source and destination buckets be different S3 storage classes?

## VPC

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
- What are protocols? What ports do SSH, HTTP, HTTPS?
- What are Security Groups?
- What is the default SG allow/deny config?
- At what level are Security Groups applied?
- SGs are stateful, what does this mean?
- How many subnets can an NACL be applied to?
- How many NACLs can a subnet be associated with?
- Can NACLs be used to block specific IP addresses?
- How are NACL rules evaluated?
- NACLs are stateless, what does this mean? What is an example?
- What is the allow/deny configuration for newly created NACLs?
- What is the configuration for the VPC default NACL?
- What purpose does a VPC endpoint have?
- Describe VPC endpoints?
- Do VPC endpoints impose availability risks or bandwidth constraints on your network traffic?
- VPC endpoints may be a better option for helping AWS services communicate amongst each other over NAT Gateways, why is that?
- What are the two types of VPC endpoints?
- What is an Interface Endpoint?
- What is a Gateway Endpoint?
- What does ENI stand for?
- What approaches can you use to connect your VPC to other VPCs? Name 3.
- What are the two downsides of opening your VPC up to the internet?
- What are the downsides to using VPC peering?
- VPC peering makes your whole network accessible, why could this be an issue?
- What is the best way to expose a service VPC to tens, hundreds, or thousands, of customer VPCs?
- Does PrivateLink require route tables, NAT Gateways, Internet Gateways, etc?
- What does PrivateLink require on the service VPC?
- What does PrivateLink require on the customer VPC?
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
- What problem does VPN CloudHub help to resolve?
- What topology model does VPN CloudHub use?
- What are some benefits of VPN CloudHub?
- Does VPN CloudHub operate over the internet? Is it safe?
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
