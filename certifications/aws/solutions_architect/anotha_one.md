# IAM

- You can use an **IAM Role** to delegate access to resources that are in _different AWS accounts_ that you own. By setting up cross-account access in this way, you don't need to create individual IAM users in each account. In addition, users don't have to sign out of one account and sign into another in order to access resources that are in different AWS accounts.
- It is not possible to create a common IAM policy for multiple AWS accounts.
- Although you can set up cross-account access to each department, this entails a lot of configuration compared with using AWS Organizations and Service Control Policies (SCPs). **Cross-account access would be a more suitable choice if you only have two accounts to manage, but not for multiple accounts** (use organizations in that case).
- Cross-Account Access delegate access across AWS accounts using IAM roles
- By default, a brand new IAM user created using the AWS CLI or AWS API has no credentials of any kind. You must create the type of credentials for an IAM user based on the needs of your user. You must choose to at least include a console password or access keys when creating a new IAM user.

### Advanced IAM Policy Documents

- ARN (Amazon Resource Name): `arn:partition:service:region:account_id:resource || resource_type/ ...`
- ARN `:region` is omitted from global service ARNs (like IAM).
- Policies attached to users = Identity Policy
- Policies attached to resources = Resource Policy
- Policies consist of statements, which themselves are composed of AWS api requests.

### IAM Questions

- Can you create a role that allows users to access resources in other AWS accounts?
- Can you create a policy that exists across multiple AWS accounts?
- Describe IAM cross-account access.
- Is cross-account access suitable for situations where there are more than 2 accounts to manage? What is a better alternative?
- By default, do brand new IAM users have credentials of any kind?

#### IAM Advanced Policies Questions

- What is the structure of an IAM policy ARN?
- What segment of the ARN is omitted if the service is global?
- What are policies called when attached to users?
- What are policies called when attached to resources?

---

# AWS Certificate Manager

- Allows you to create, manage, and deploy public and private SSL certificates for use with other AWS services.
- Don't have to pay for SSL certificates.
- **Public and private certificates are free.**
- **Automates the renewals and deployment of your certificates.**
- Integrates with ELB, CloudFront, and API Gateway.

### AWS Certificate Manager Questions

- What does AWS Certificate Manager do?
- Do SSL certificates cost money in AWS Certificate Manager?
- Does AWS Cert Manager automatically renew and deploy your certificates?
- What AWS services does AWS Cert Manager integrate with?

---

# VPC

### Peering

- You can create a VPC peering connection between your own VPCs, with a VPC in another AWS account, or with a VPC in a different AWS Region.
- A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them privately.

### Egress-only Internet Gateway

- EO Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows outbound communication over IPv6 from instances in your VPC to the Internet and prevents the internet from initiating an IPv6 connection to your instance.
- **Egress-Only Internet Gateway is for use with _IPv6 traffic only_.**
- To enable outbound-only Internet communications **over IPv4** use a NAT gateway.

### NAT Gateways

- NAT devices allow resources in private subnets to communicate over the internet while restricting unsolicited inbound traffic.
- **AWS offers two kinds of NAT devices — a NAT gateway or a NAT instance.** It is recommended to use NAT gateways, as they provide better availability and bandwidth over NAT instances.
- The NAT Gateway service is a managed service that does not require your administration efforts. **A NAT instance is launched from a NAT AMI.**
- NAT Instance is not as highly available compared to a NAT Gateway.

### VPC Questions

- Can you peer VPCs across regions?
- Is traffic routed privately between peered VPCs?

#### NAT Gateways Questions

- What are they two types of NAT devices? Which variant does AWS recommend and why?
- Which NAT variant is managed and which is launched from a NAT AMI?
- Which NAT variant is more highly available?

---

# AWS Transit Gateway

- AWS Transit Gateway connects VPCs and on-premises networks through a central hub and acts as a cloud router where each new connection is only made once.

### AWS Transit Gateway Questions

- What is AWS Transit Gateway?
- What problems does AWS Transit Gateway Solve?

---

# AWS Organizations

- AWS Organizations offers **policy-based management** for multiple AWS accounts.
- With Organizations, you can create groups of accounts, automate account creation, apply and manage policies for those groups.
- You can use the consolidated billing feature in AWS Organizations to consolidate payment for multiple AWS accounts or multiple AISPL accounts. With consolidated billing, you can see a combined view of AWS charges incurred by all of your accounts.
- Organizations are composed of **OUs or Organizational Units.**
- Organizations enables you to centrally manage policies across multiple accounts, without requiring custom scripts and manual processes.
- It allows you to create **Service Control Policies (SCPs)** that centrally control AWS service use across multiple AWS accounts.
- AWS Organizations SCPs _don't_ replace associating IAM policies within an AWS account.
- **You can use SCPs to allow or deny access to AWS services for individual AWS accounts within AWS Organizations member accounts, or for groups of accounts within an organizational unit (OU). The specified actions from an attached SCP affect all IAM identities including the root user of the member account.**

### AWS Organizations Questions

- What kind of management do Organizations provide?
- **What is Policy-Based Management?**
- Can you create groups of accounts with Organizations?
- Can you automate the account creation process with Organizations?
- Can you apply and manage policies for groups of accounts with Organizations?
- What feature in AWS Organizations can you use to get a consolidate report of all the expenses of the accounts within your Organization?
- What are Organizations composed of?
- Does AWS Organizations allow you to centrally manage policies across multiple accounts without requiring custom scripts and manual processes?
- What are SCPs?
- Do SCPs replace IAM policies within an AWS account?
- Do the actions from an SCP attached to an OU account, or collection of accounts, affect all IAM identities within -- including the root users of each member account?

---

# Trusted Advisor

- Trusted Advisor is an online tool that provides you real-time guidance to help you provision your resources following AWS best practices. It only provides you alerts on areas where you do not adhere to best practices and tells you how to improve them.
- It does _not_ assist in maintaining governance over your AWS accounts.

### Trusted Advisor Questions

- What is Trusted Advisor?
- Does Trusted Advisor help maintain governance over your accounts?

---

# Protocols

- RDP, Remote Desktop Protocol TCP/UDP port 3389.
- Port 22 SSH.

### Protocol Questions

- What is the Remote Desktop protocol?
- What is the SSH protocol?

---

# Route 53

- You can use Route 53 to distribute the traffic load to the multiple EC2 instances across multiple AWS Regions.
- ELBs can only route traffic to resources behind them in the same region (across AZs), Route 53 _can_ route traffic across regions.
- If your application is hosted in multiple AWS Regions, you can improve performance for your users **by serving their requests from the AWS Region that provides the lowest latency.**
- **You can create latency records for your resources in multiple AWS Regions by using latency-based routing.** In the event that Route 53 receives a DNS query for your domain or subdomain such as `tutorialsdojo.com` or `portal.tutorialsdojo.com`, it determines which AWS Regions you've created latency records for, determines which region gives the user the lowest latency and then selects a latency record for that region.

### Route 53 Questions

- Can Route 53 distribute traffic load to EC2 resources across multiple AWS regions?
- What is a core difference between ELB traffic routing and Route 53 traffic routing?
- If your application is hosted _across multiple regions_ are you able to route traffic based on latency?
- Describe how latency records work and what sort of routing leverages them.

---

# Elastic Load Balancer (ELB)

- Elastic Load Balancing offers three types of load balancers that all feature the **high availability, automatic scaling, and robust security necessary to make your applications fault-tolerant**. They are: **Application Load Balancer, Network Load Balancer, and Classic Load Balancer.**
- **Load balancers distribute traffic only within their respective regions and not to other AWS regions. They cannot push traffic to another region.**
- **Network Load Balancers support connections from clients to IP-based targets in peered VPCs across different AWS Regions.**
- ELB sticky sessions _routes a user_ to the particular web server that is managing that individual user’s session.
- By default, each load balancer node distributes traffic across the registered targets in its Availability Zone only.
- If you enable cross-zone load balancing, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones.
- **With cross-zone load balancing enabled, your load balancer nodes distribute incoming requests evenly across the Availability Zones enabled for your load balancer. Otherwise, each load balancer _node_ distributes requests only to instances in its Availability Zone.**
- **Cross-zone load balancing reduces the need to maintain equivalent numbers of instances in each enabled Availability Zone,** and improves your application's ability to handle the loss of one or more instances. However, we still recommend that you maintain approximately equivalent numbers of instances in each enabled Availability Zone for higher fault tolerance.
- When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled availability zones.

### Network Load Balancer

- Network Load Balancer is best suited for load balancing of TCP traffic where extreme performance is required.
- Operates at the connection level (Layer 4).
- Network Load Balancer is also optimized to handle sudden and volatile traffic patterns.
- Network Load Balancer routes traffic to targets within a VPC and is capable of **handling millions of requests per second while maintaining ultra-low latencies.**
- Network Load Balancer is used for applications that need extreme network performance.
- **Network Load Balancer automatically provides a static IP per Availability Zone (subnet) that can be used by applications as the front-end IP of the load balancer.**
- Network Load Balancer also allows you the option to **assign an Elastic IP per Availability Zone (subnet)** thereby providing your own fixed IP.
- It does not support path-based routing.
- **Cross-zone load balancing is disabled by default in NLBs.**

### Application Load Balancer

- It cannot handle TCP or Layer 4 connections, only Layer 7 (HTTP and HTTPS).
- You can place an ALB / NLB in front of your AWS Fargate cluster.
- If your application is composed of several individual services, an Application Load Balancer can route a request to a service based on the content of the request such as **Host field, Path URL, HTTP header, HTTP method, Query string, or Source IP address.**
- **You can use path conditions to define rules that forward requests to different target groups based on the URL in the request** (also known as path-based routing).
- Host-based routing defines rules that forward requests to different target groups based on the hostname in the host header instead of the URL.
- Path-based routing allows you to route a client request based on the URL path of the HTTP header.
- **With Application Load Balancers, cross-zone load balancing is always enabled.**

### Gateway Load Balancer???

- A Gateway Load Balancer does not support path-based routing. You must use an Application Load Balancer.

### Classic Load Balancer

- If the load balancer nodes for your Classic Load Balancer can distribute requests regardless of Availability Zone, this is known as **cross-zone load balancing**.
- **Cross-zone load balancing is disabled by default CLBs.**
- Support for EC2-Classic.

### ELB Questions

- What are benefits of using an ELB?
- What three types of Load Balancers are provided by ELB?
- Are Load Balancers able to serve across regions?
- Can NLBs support connections across regions in through peered VPCs?
- What are ELB sticky sessions?
- By default, does each load balancer node distributes traffic across the registered targets in its Availability Zone only?
- If you enable cross-zone load balancing, does each load balancer node distribute traffic across the registered targets in all enabled Availability Zones?
- What does enabling cross-zone load balancing help mitigate?
- Which Load Balancer has cross-zone load balancing enabled by default?

#### Network Load Balancer Questions

- What situations are NLBs optimal for?
- What connection layer does NLB operate on?
- Is NLB optimal for handling spiking and sudden high-traffic patterns?
- How many requests per second can NLB handle while maintaining ultra-low latencies?
- Does NLB provide a static IP per subnet?
- Does NLB allow the assignment of Elastic IPs per subnet?
- Does NLB support path-based routing?
- Is cross-zone load balancing always enabled with NLB?

#### Application Load Balancer Questions

- What connection layer does ALB support?
- Can you place an ALB (or NLB) in front of a Fargate Cluster?
- An Application Load Balancer can route a request to a service based on the which types of content from a request?
- You can forward requests to different ALB target groups based on the URL provided in the HTTP Header provided with the request. What type of routing is this called?
- Is cross-zone load balancing always enabled with ALB?

#### Classic Load Balancer Questions

- Is cross-zone load balancing always enabled with CLB?

---

# AWS DataSync

- AWS DataSync service simply provides a fast way to move large amounts of data online between on-premises storage and **Amazon S3 or Amazon Elastic File System (Amazon EFS).**
- DataSync is _not_ a hybrid solution. It integrates on-prem data to AWS.

### AWS DataSync Questions

- To what storage services does DataSync move large amounts of data potentially to?
- Is DataSync suitable for hybrid architectures?

---

# AWS Shield

- Free DDoS protection.
- Protects Layer 3 / Layer 4 attacks.
- **Protects all AWS customers on: ELB, Cloudfront, and Route 53.**
- Protects against SYN/UDP floods, reflection attacks, and other layer 3/layer 4 attacks.

### AWS Shield Advanced

- Costs $3,000 a month.
- Provides enhanced protections for ELB, CloudFront, Route 53 against larger and more sophisticated attacks.
- Offers always-on, flow based monitoring of network traffic and active application monitoring to provide near real-time notifications of DDoS attacks.
- Give you 24/7 access to the **DDoS Response Team (DRT)** to help manage and mitigate application-layer DDoS attacks.
- Protects AWS bill against higher fees caused by DDoS attacks.

### AWS Shield Questions

- What does AWS Shield provide?
- Is AWS Shield free?
- What AWS services does Shield provide protection for?
- What connection layer attacks does Shield protect against?

#### AWS Shield Advanced Questions

- Is AWS Shield Advanced free?
- What does AWS Shield Advanced protect against?
- What services does AWS Shield Advanced protect?
- Does Shield Advanced provide always-on active monitoring?
- What is DRT? What level of access do you have to DRT?
- Does Shield Advanced protect your AWS bill against higher fees caused by DDoS attacks?

---

# AWS WAF

- **Web Application Firewall**, lets you monitor **(layer 7) HTTP/HTTPs requests forwarded to Cloudfront or Application Load Balancer.**
- Operates at Layer 7.
- Allows 3 behaviors: Allows all requests except the ones you specify, block all requests except the ones you specify, Count the requests that match the properties you specify.
- Blocks layer 7 attacks like layer 7 DDoS attacks, Cross-site scripting, SQL injections.
- Block access to specific countries or IP addresses. WAF is the primary solution for restricting traffic from countries.

### AWS WAF Questions

- What does WAF stand for?
- What does WAF provide and what connection layer does it operate on?
- AWS WAF monitors requests to what AWS Services?
- What are some examples of layer 7 attacks?
- Is WAF the ideal solution for restricting traffic from specific countries?

---

# AWS GuardDuty

- Threat detection service that uses machine learning to continuously monitor for malicious behavior.
- Tracks unusual API calls, calls from known malicious IPs, attempts to disable CloudTrail logging, unauthorized deployments, compromised instances, recon by would-be attackers, port scanning and failed logins.
- Centralize threat detection across multiple AWS accounts.
- Takes 7-14 days to set baseline (determine normal account behavior.)
- Once active, **findings are found on GuardDuty console and CloudWatch events.**
- 30 days free, then charges based on quantity of CloudTrail events and DNS / VPC flow logs data.
- Updates a database of known malicious domains using external feeds from 3rd party resources.
- **Monitors: CloudTrail logs, VPC flow logs, and DNS logs.**

### AWS GuardDuty Questions

- What does AWS GuardDuty do?
- What are examples of things GuardDuty protects against?
- Can GuardDuty provide centralized protection for _multiple_ AWS accounts?
- How long does it take for GuardDuty to determine a baseline?
- What two locations are GuardDuty findings posted to?
- What is the general cost structure like for GuardDuty?
- Is AWS GuardDuty's database of known malicious domains provided by 3rd party resources?
- What types of logs does GuardDuty monitor?

---

# AWS Macie

- Service that uses ML and pattern matching to monitor S3 buckets for PII vulnerabilities.
- Alerts you to unecrypted buckets.
- Alerts you about public buckets.
- Macie alerts are sent to Amazon EventBridge.
- Can be integrated with AWS Security Hub for broader analysis of your organizations security poster.
- Can be integrated with step functions to immediately take remediation actions.
- Helps prevent identity theft.
- Great for HIPAA and GDPR compliance.

### AWS Macie Questions

- What does Macie do?
- What AWS Service does Macie monitor specifically?
- What are some bucket conditions Macie will alert you to?
- Can Macie be integrated with AWS Security Hub? -- What benefits does this provide?
- What types of compliances is Macie ideal for?

---

# AWS Inspector

- Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS.
- Used to perform **vulnerability scans** on both EC2 instances and VPCs.
- Assesses applications for vulnerabilities or deviations from best practices.
- **Two types of assessments: network assessments and host assessments.**
- **Network (VPC)** assessments checks for ports reachable from outside the VPC, inspector agent _not_ required.
- **Host (EC2)** assessments checks for vulnerable software, host hardening, and security best practices. Inspector agent _is_ required.
- Steps: create assessment target, install agents on EC2 (if Host assessment) instances, create assessment template, perform an assessment run, review findings.

### AWS Inspector Questions

- What does AWS Inspector help with?
- What kind of scans does Inspector provide and on what AWS services are these scans applied to?
- What are the two types of assessments AWS Inspector applies?
- Network assessments conduct what type of checks -- is the inspector agent required?
- Host assessments conduct what type of checks -- is the inspector agent required?
- What is the network or host in these assessment types?
- What are the assessment steps?

---

# CloudTrail

- Tracks API calls, made in AWS, and logs them in an S3 bucket.
- Doesn't track RDP or SSH calls.
- Captures the **metadata of the api call, identity of caller, time of api call, source IP of API caller, request parameters, and response elements returned by the service.**
- provides near real-time intrusion detection.

### AWS CloudTrail Questions

- What does CloudTrail do?
- Can CloudTrail track SSH or RDP calls -- bonus, what is their port number?
- CloudTrail captures what types of data returned from a service?
- Does CloudTrail provide near real-time intrusion detection?

---

# ECS

- Although ECS provides **Service Auto Scaling, Service Load Balancing, and Monitoring with CloudWatch, these features are not automatically enabled by default unlike with Elastic Beanstalk.**

### ECS Auto Scaling

- The following metrics are available for **ECS Service**:
  `ECSServiceAverageCPUUtilization`—Average CPU utilization of the service.
  `ECSServiceAverageMemoryUtilization`—Average memory utilization of the service.
  `ALBRequestCountPerTarget`—Number of requests completed per target in an Application Load Balancer target group.

### ECS Questions

- What features are not automatically enabled by default for ECS?

#### ECS Auto Scaling Questions

- What metrics are available for ECS?

---

# Aurora

- Amazon Aurora is fully managed by Amazon RDS, which automates time-consuming administration tasks like hardware provisioning, database setup, patching, and backups.
- **Read replication latency of less than 1 second** is only possible if you would use Amazon Aurora replicas.
- **You can create up to 15 Aurora replicas within an AWS Region.**
- MySQL replicas won't provide you a read replication latency of less than 1 second. RDS Read Replicas can only provide asynchronous replication in seconds and not in milliseconds. You have to use Amazon Aurora replicas in this scenario.

### Aurora Questions

- Is AWS Aurora fully managed?
- What is the Read replication latency for Aurora?
- How many Aurora Replicas can you create in an AWS region?
- Can MySQL, and other RDS solutions, provide sub 1-second read replica latency?

---

# Caching

- AWS provides internal and external cache solutions.
- **Internal Caching:** putting a cache layer in front of our database, the less we talk to the database the better. -- ElastiCache.
- **External Caching:** caching images, videos, static content, etc. -- CloudFront.
- DAX: an internal caching solution specifically for DynamoDB.
- Global Accelerator: speeds up external client connections to AWS resources by providing a (2) static IPs.

### Caching Questions

- What are two types of caching available in AWS?
- What is internal caching, what service is used for this?
- What is external caching, what service is used for this?
- In addition to Elasticache and Cloudfront, what are two other caching systems that exist in AWS?

---

# AWS Global Accelerator

- A networking service that sits in front of our applications and sends your users' traffic through AWS's global network infrastructure where it can increase performance and help deal with IP caching.
- Global Accelerator helps your infrastructure by providing **IP caching.**
- A endpoint group is a collection of resources, or endpoints, grouped together and served to the Global Accelerated. Think, ELBs and their subsequent resources within a given region being attached to GA.
- Global Accelerator provides 2 static IP addresses.
- Masks complex architecture. As your application grows and and change, your users wont notice. They will see the same IPs no matter what.
- Global Accelerator speeds things up since **traffic is routed through AWS's global network infrastructure.**
- By creating **Weighted Pools** you can create weighted groups behind the IPs to test our new features or handle failures in your environment.
- AWS Global Accelerator is a service that improves the availability and performance of your applications with local or global users.
- **It provides static IP addresses that act as a fixed entry point to your application endpoints in a single or multiple AWS Regions, such as your Application Load Balancers, Network Load Balancers, or Amazon EC2 instances.**
- When the application usage grows, the number of IP addresses and endpoints that you need to manage also increase. AWS Global Accelerator allows you to scale your network up or down.
- AWS Global Accelerator lets you associate regional resources, such as load balancers and EC2 instances, to **two static IP addresses**. You only whitelist these addresses once in your client applications, firewalls, and DNS records.
- If you have multiple resources in multiple regions, you can use AWS Global Accelerator to reduce the number of IP addresses.
- By creating an **endpoint group, you can add all of your EC2 instances from a single region in that group.** You can add additional endpoint groups for instances in other regions. After that, you can then associate the appropriate ELB endpoints to each of your endpoint groups.
- Global Accelerator has two static IP addresses that you can use to create a security rule in your firewall device. Instead of regularly adding the Amazon EC2 IP addresses in your firewall, you can use the static IP addresses of AWS Global Accelerator to automate the process and eliminate this repetitive task.
- It is better to create one endpoint group instead of multiple endpoints. Moreover, you should to associate the ALBs to AWS Global Accelerator and not the underlying EC2 instances.

### AWS Global Accelerator Questions

- What is AWS Global Accelerator?
- What kind of caching does AWS global accelerator provide?
- How many static IP addresses does Global Accelerator provide?
- Global Accelerator provides static IP addresses for the applications it masks, how does this help reduce architectural complexity as the underlying application grows and changes?
- Does traffic routed through the Global Accelerator travel through AWS's global infrastructure? Does this speed things up?
- What are Weighted Pools, what purpose do they provide?
- Do Global Accelerator's static IP addresses provide a fixed entry point for ELB/EC2 instances across multiple regions?
- What are Global Accelerator **endpoint groups**, how are they useful?
- How can the static IP addresses provided by Global Accelerator simplify firewall policies?
- Should you aim to create on endpoint group or multiple and is it better to have ALBs in front of the EC2 resources point to the endpoint?

---

# ElastiCache

- **Elasticache is a managed version of 2 open-source technologies: Memcached and Redis.** Neither of these tools are specific to AWS, but by using ElastiCache you avoid a lot of the common issues you may encounter.
- Amazon ElastiCache isn't able to improve the database performance of a database experiencing highly dynamic reads. This option would be helpful if the database frequently receives the same queries.
- ElastiCache provides sub-millisecond latency caching.
- In order to address scalability and to provide a shared data storage for sessions that can be accessed from any individual web server, you can abstract the HTTP sessions from the web servers themselves. A common solution for this is to leverage an In-Memory Key/Value store such as Redis or Memcached.
- Although you can use DynamoDB and RDS for storing session state, these two are not the best choices in terms of cost-effectiveness and performance when compared to ElastiCache. There is a significant difference in terms of latency if you used DynamoDB and RDS when you store the session data.

### Memcached

- Traditionally sits in front of a database (like an RDS database)
- Sits inside database layer and caches common queries made to datastore.
- Nothing stored here is permanent, not a source of truth.
- No failovers or multi-AZ support.
- No backups.

### ElastiCache for Redis

- Traditionally sits in front of a database (like an RDS database)
- Supported as a caching solution, but can also function as a standalone database.
- No-SQL database with failover and multi-AZ support.
- Supports backups.
- Using Redis `AUTH` command can improve data security by requiring the user to enter a password before they are granted permission to execute Redis commands on a password-protected Redis server.
- Authenticate the users using Redis `AUTH` by creating a new Redis Cluster with both the `--transit-encryption-enabled` and `--auth-token` parameters enabled.
- To require that users enter a password on a password-protected Redis server, include the parameter `--auth-token` with the correct password when you create your replication group or cluster and on all subsequent commands to the replication group or cluster.
- Redis `AUTH` option is disabled by default.

### ElastiCache Questions

- Elasticache is a managed version of what 2 open-source technologies?
- If a database is experience highly dynamic reads would ElastiCache be able to improve database performance?
- What latency speed does ElastiCache provide?
- Can DynamoDB or RDS capture session state as cheaply and effectively as ElastiCache?

#### Memcached Questions

- Where does Memcached typically sit? -- What type of caching is it?
- Is cached data within memcached permanent?
- Can memcached be seen as a source of truth?
- Does memcached provide failovers or multi-AZ support?
- Does memcached provide backups?

#### ElastiCache for Redis Questions

- Where does ElastiCache for Redis typically sit? -- What type of caching is it?
- Can ElastiCache for Redis serve as both a caching solution _and_ a standalone database?
- Is ElastiCache for Redis a SQL or NoSQL solution?
- Does ElastiCache for Redis provide multi-AZ supports and failovers?
- Does ElastiCache for Redis support backups?
- What command can improve data security for redis?
- Is the Redis AUTH option disabled by default?

---

# AWS Storage Gateway

- Depending on your use case, Storage Gateway provides **three types of storage interfaces for your on-premises applications:** **file, volume, and tape.**
- When you deploy File Gateway, you specify how much disk space you want to allocate for local cache. This local cache acts as a buffer for writes and provides low latency access to data that was recently written to or read from Amazon S3.
- **When a client writes data to a file via File Gateway, that data is first written to the local cache disk on the gateway itself.** **Once the data has been safely persisted to the local cache, only then does the File Gateway acknowledge the write back to the client. From there, File Gateway transfers the data to the S3 bucket asynchronously in the background, optimizing data transfer using multipart parallel uploads, and encrypting data in transit using HTTPS.**

#### Amazon S3 File Gateway (file)

- Amazon S3 File Gateway presents a file interface that enables you to store files as objects in Amazon S3 using the industry-standard **NFS and SMB file protocols**, and access those files via NFS and SMB from your data center or Amazon EC2, or access those files as objects directly in Amazon S3.

#### Amazon FSx for Windows Gateway (file)

- The Amazon FSx File Gateway enables you to store and retrieve files in Amazon FSx for Windows File Server using the **SMB protocol**. Files written through Amazon FSx File Gateway are directly accessible in Amazon FSx for Windows File Server.

#### Volume Gateway

- **The Volume Gateway provides block storage to your on-premises applications using iSCSI connectivity.** Data on the volumes is stored in Amazon S3 and you can take **point in time copies of volumes which are stored in AWS as Amazon EBS snapshots.** You can also take copies of volumes and manage their retention using AWS Backup. **You can restore EBS snapshots to a Volume Gateway volume or an EBS volume.**

#### Tape Gateway

- **The Tape Gateway provides your backup application with an iSCSI virtual tape library (VTL) interface**, consisting of a virtual media changer, virtual tape drives, and virtual tapes. **Virtual tapes are stored in Amazon S3 and can be archived to Amazon S3 Glacier or Amazon S3 Glacier Deep Archive.**

### AWS Storage Gateway Questions

- What three types of storage interfaces does AWS Storage Gateway provide?
- When you deploy a File Gateway, what do you need to specify?
- How is data managed and transferred across a file gateway.

#### S3 File Gateway Questions

- What industry-standard protocols does S3 File Gateway use?
- What three locations you retrieve your data using S3 File Gateway?

#### FSx for Windows Gateway Questions

- What industry-standard protocols does FSx for Windows Gateway use?
- Where are files stored with this gateway?

#### Volume Gateway Questions

- What does Volume Gateway provide for you on-prem apps using iSCSI connectivity?
- Where is the data on the volumes stored?
- Point in time copies of volumes are stored as what in AWS?
- Where can you restore EBS snapshots to?

#### Tape Gateway Questions

- What does Tape Gateway provide your backup application with?
- Where are virtual tapes stored, can they be archived?

---

# S3

- **Data transferred between S3 buckets in the same AWS Region is free.**
- **Data transferred from an Amazon S3 bucket to any AWS service(s) within the same AWS Region as the S3 bucket (including to a different account in the same AWS Region) is free.**
- **Data transferred in from the internet is free.**
- **Data transferred out to the internet for the first 100 GB per month is free.**
- **Data transferred out to Amazon CloudFront (CloudFront).**
- To enable the cross-region replication feature in S3, the following items should be met: **The source and destination buckets must have versioning enabled. The source and destination buckets must be in different AWS Regions.**
- By default, **an S3 object is owned by the AWS account that uploaded it even though the bucket is owned by another account.**
- **To get full access to the object, the object owner must explicitly grant the bucket owner access.** You can create a bucket policy to require external users to grant `bucket-owner-full-control` when uploading objects so the bucket owner can have full access to the objects.
- **With "Requester Pays buckets" policy, the requester, instead of the bucket owner, pays the cost of the request and the data download from the bucket.**
- Using multipart upload provides the following advantages:
  - **Improved throughput** - You can upload parts in parallel to improve throughput.
  - **Quick recovery from any network issues** - Smaller part size minimizes the impact of restarting a failed upload due to a network error.
  - **Pause and resume object uploads** - You can upload object parts over time. Once you initiate a multipart upload, there is no expiry; you must explicitly complete or abort the multipart upload.
  - **Begin an upload before you know the final object size** - You can upload an object as you are creating it.
- **Only Amazon S3 Standard has the feature of no minimum storage duration.**
- S3 Standard is also the most cost-effective storage service because you will only be charged for the last 12 hours, unlike in other storage classes where you will still be charged based on its respective storage duration (e.g. 30 days, 90 days, 180 days).
- **Data that is deleted from S3 Standard-IA within 30 days will still be charged for a full 30 days.**
- **S3 Standard-IA storage class has a minimum storage duration of at least 30 days.**
- **S3 Glacier Deep Archive has a minimum storage duration of at least 180** days which is only suitable for backup and data archival. If you store your data in Glacier Deep Archive for only 12 hours, you will still be charged for the full 180 days.
- If you triggered an S3 API call and got **HTTP 200 result code and MD5 checksum,** then it is considered as a successful upload. The S3 API will return an error code in case the upload is unsuccessful.

### S3 Questions

- What are 5 data transfer scenarios that are _free_ for S3.
- What 2 conditions must be met to allow for cross-region replication in S3?
- By default, who owns an an object when it's uploaded to S3?
- How can a bucket owner gain access to an object uploaded by a user in S3? -- what bucket policy provides this access?
- What policy makes the requestor, instead of the bucket owner, pay the cost for requesting and downloading data from a bucket?
- What advantages (4) does multipart uploading provide?
- What is the _only_ S3 storage format that has **no minimum storage duration?**
- Is data that is deleted before the storage duration is met still charged for the whole duration?
- What are the minimum storage durations for S3 Standard, S3 OZIA/IA, S3 Glacier, and S3 Glacier DA?
- When response does S3 provide if an upload was successful?

---

# CloudFormation

- **CloudFormation is used for provisioning and managing stacks of AWS resources based on templates you create to model your infrastructure architecture.** CloudFormation is recommended if you want a tool for granular control over the provisioning and management of your own infrastructure.
- The `Resources` section is the only required section.

### CloudFormation Questions

- What is CloudFormation used for?
- What is the _only_ required section in the CloudFormation script?

---

# EC2

- The Reserved Instance Marketplace is a platform that supports the sale of third-party and AWS customers' unused Standard Reserved Instances, which vary in terms of length and pricing options.
- An Elastic IP address doesn’t incur charges as long as the following conditions are true: **The Elastic IP address is associated with an Amazon EC2 instance, the instance associated with the Elastic IP address is running, the instance has only one Elastic IP address attached to it.**
- If you’ve stopped or terminated an EC2 instance with an associated Elastic IP address and you don’t need that Elastic IP address anymore, consider disassociating or releasing the Elastic IP address.
- If you recall, **data transferred between EC2, RDS, Redshift, ElastiCache instances, and Elastic Network Interfaces in the _same Availability Zone_ is free.**

### Enhanced Networking

- There is no additional charge for using enhanced networking.
- Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types.
- Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies.

### EC2 Questions

- What is the Reserved Instance Marketplace?
- Under what 3 conditions will an Elastic IP address no incur charges? -- What should you consider doing if these conditions aren't met?
- Data transferred between EC2, RDS, Elasticache, and ENI is free under what condition?

#### Enhanced Networking Questions

- Is there an additional charge for using Enhanced Networking with your EC2 instances?
- What does Enhanced Networking provide for your EC2 infrastructure?

---

# EBS

- **You can configure your AWS account to enforce the encryption of the new EBS volumes and snapshot copies that you create.**
- **Encryption by default is a Region-specific setting.**
- If you enable it (Encryption) for a Region, you **cannot disable it for individual volumes or snapshots in that Region.**
- **Amazon EBS does not support asymmetric CMKs.**
- Although **there is no direct way to encrypt an existing unencrypted volume or snapshot**, you can encrypt them by creating either a new volume or a snapshot. If you enabled encryption by default, Amazon EBS encrypts the resulting new volume or snapshot using your default key for EBS encryption. Even if you have not enabled encryption by default, you can enable encryption when you create an individual volume or snapshot.
- EBS does not support asymmetric CMKs. To encrypt an EBS snapshot, you need to use symmetric CMK.
- The Encryption By Default feature is a Region-specific setting and thus, you can't enable it to selected EBS volumes only.

### EBS Questions

- Can you configure your AWS account to _enforce_ the encryption of the new EBS volumes and snapshots that you create?
- Is Encryption by default, for EBS, region specific?
- If you enable encryption for a region, can you disable EBS encryption for individual volumes or snapshots within that region?
- Does EBS support asymmetric CMKs? -- what keys does EBS support?
- Is there a way to encrypt an already existing _unecrypted_ EBS volume or snapshot?
- What steps do you need to take if you want to encrypt previously unecrypted EBS volumes or snapshots?

---

# Direct Connect

- AWS Direct Connect is a network service that provides an alternative to using the Internet to connect customer's on-premises sites to AWS. AWS Direct Connect does not involve the Internet; instead, it uses dedicated, private network connections between your intranet and Amazon VPC.

---

# Amazon Connect

- Amazon Connect is based on the same contact center technology used by Amazon customer service associates around the world to power millions of customer conversations.

---

# API Gateway

- Supports RESTful APIs and Websocket APIs.
- API Gateway provides a _single endpoint_ for all client traffic interacting with the backend of your application.
- Allows you to connect to applications running on Lambda, EC2, Elastic Beanstalk, and services like DynamoDB, Kinesis, and more.
- Supports multiple endpoints and targets.
- Allows you to maintain multiple versions of your API.
- API Gateway is serverless.
- API Gateway integrates with CloudWatch. It logs API calls, latencies, and error rates to CloudWatch.
- API Gateway supports throttling. This allows backend services to withstand traffic spikes and denial of service attacks.

#### Restful APIs

- **Re**presentational **S**tate **T**ransfer.
- Optimized for serverless and web applications.
- Stateless

---

# Lambda

- Lambda functions can’t process long-running processes.
- Take note that a Lambda function has a **maximum processing time of 15 minutes**.
- To prevent your Lambda function from running indefinitely, you specify a timeout.
- **The default timeout is 3 seconds and the maximum execution duration per request in AWS Lambda is 900 seconds, which is equivalent to 15 minutes.**
- **By default, the AWS Lambda limits the total concurrent executions across all functions within a given region to 1000.**
- **By setting a concurrency limit on a function, Lambda guarantees that allocation will be applied specifically to that function, regardless of the amount of traffic processing the remaining functions. If that limit is exceeded, the function will be throttled but not terminated.**
- Having a recursive code in your Lambda function does not directly result to an abrupt termination of the function execution. This could lead to an unintended volume of function invocations and escalated costs, but not an abrupt termination because Lambda will throttle all invocations to the function.
- It is unlikely to have several `ServiceException` errors throughout the day unless there is an outage or disruption in AWS.

#### Lambda Versions

- When you create a lambda function, there is only one version: `$LATEST`
- You can create multiple versions of your function code and use aliases to reference the version you want to use as part of the ARN.
- An **lambda alias** is like a pointer to a specific version of the function code. Example: Version 2 $LATEST Alias: Test seen in an ARN `...arn:function:mylambda:Test || $LATEST`

#### Concurrent Lambda Executions Limit

- Lambda has a concurrent execution limit, or a limit to the number of concurrent executions across _all_ functions in a given region per account.
- Default limit is 1,000 concurrent executions per region.
- Results in a HTTP 429 error.
- **Reserved Concurrency**: guarantees that a set number of executions that will always sbe available for your critical functions. Also serves as a limit.

#### Lambda and VPC

- By default, lambdas cannot connect to private subnets.
- Lambda needs the following VPC Configuration information so it can connect to a VPC: **Private Subnet ID and Security Group ID (with req'd access).** Lambda uses this information to set up ENIs using an available IP address from your private subnet.
- You can use the `vpc-config` parameter to add VPC information to your Lambda function config.

#### Step Functions

- Allows you to build and run serverless applications as a series of steps.
- Step functions log the state of each step, so when things do go wrong, you can diagnose and debug problems quickly.
- Step functions are a great way to visualize your serverless application.
- Step functions log the state of each step, so if something goes wrong you can you can track what went wrong.
- The output of one step function is often the input of the next.
- Step functions are composed of a **State Machine** made up of **Tasks**, where the State Machine is the workflow with a Task being a unit of work in the workflow.
- State Machines are defined using **Amazon States Language.**
- **Parallel Workflow** is when tasks are done together and then combined as an output to the next step.
- **Branching Workflow** is when there are branches for the State Machine can follow. For example, if all tests passed do one next step and if tests fail, do another step.

### Lambda@Edge

- Lambda@Edge is primarily used for serverless edge computing.

- You can't set Lambda@Edge functions as part of your origin group in CloudFront.

### Lambda Questions

- Can Lambda process long-running processes?
- What is the maximum processing time of lambda functions?
- What can provide to prevent a lambda function from running indefinitely?
- What is the default timeout for a lambda function?
- What is the maximum execution duration per request in AWS Lambda?
- What is the default limit of total concurrent executions across all lambda functions in a region?
- What does lambda guarantees when you set a concurrency limit on a function?
- If you have recursive code in your lambda function, will that result in an abrupt termination of the function's execution?
- Is it likely to have several `ServiceException` errors for lambda throughout the day?

---

# CloudFront

- CloudFront is a service that speeds up the distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users.
- **Defaults to HTTPS with the ability to add custom SSL certificate.**
- You can't pick specific countries -- just general areas around the globe.
- **CloudWatch supports BOTH AWS endpoints along with non-AWS applications. If you want to use CloudFront with another cloud provider, you can.**
- **You can proactively expire content from the cache if you can't wait for the TTL.**
- Works for both AWS and on-site architecture.
- Typically, CloudFront serves an object from an edge location until the cache duration that you specified passes — that is, until the object expires. **After it expires, the next time the edge location gets a user request for the object, CloudFront forwards the request to the origin server to verify that the cache contains the latest version of the object.**
- The `Cache-Control` and `Expires` headers control how long objects stay in the cache.
- The `Cache-Control max-age` directive lets you specify how long (in seconds) you want an object to remain in the cache before CloudFront gets the object again from the origin server.
- **You can use signed-urls or signed-cookies to restrict access to content.**
- **The minimum expiration time CloudFront supports is 0 seconds for web distributions and 3600 seconds for RTMP distributions.**
- You cannot set two primary origins in CloudFront.
- **An origin group includes two origins which are the primary origin and the secondary origin that will be used for the actual failover.** It also includes the failover criteria that you need to specify.
- You only store cache data in CloudFront, but **you can't host applications or web pages using this service.** You have to use Amazon S3 to host the static web pages and use CloudFront as the CDN.
- **An origin is a location where content is stored, and from which CloudFront gets content to serve to viewers.**
- **You can set up CloudFront with origin failover for scenarios that require high availability.** An origin group may contain two origins: a primary and a secondary. If the primary origin is unavailable or returns specific HTTP response status codes that indicate a failure, CloudFront automatically switches to the secondary origin.
- To set up origin failover, you must have a distribution with at least two origins.

### CloudFront Questions

- What is CloudFront?
- What protocol does CloudFront default to?
- Can you add custom SSL certifications to CloudFront?
- Can you set CloudFront support to specific countries?
- Is CloudFront restricted _only_ to AWS endpoints? Can you use Cloudfront with other cloud providers and non-AWS applications?
- Can you proactively expire content in your cache and circumvent the TTL?
- Is CloudFront suitable for both AWS and on-site architectures?
- What happens when the cached data (object) at the cloudfront edge location expires and another (identical) request comes through?
- What headers determine how long an object stays in the cache?
- What directive sets how long, in seconds, you want an object to remain in the cache before CloudFront gets the object again from the origin server?
- What two solutions can you use to restrict access to content in CloudFront?
- What is the minimum amount of time CloudFront supports for expiration in web distributions and RTMP distributions?
- Can you have two primary origins in CloudFront?
- How many origins does a CloudFront origin group include, what is their purpose?
- Can you host applications or web pages in CloudFront?
- What is an origin?
- What can you do to setup CloudFront to have high availability? -- what is required to setup the failover?

---

# Elastic Beanstalk

- AWS Elastic Beanstalk stores your **application files, and _optionally_ your server log files, in Amazon S3.**
- You can configure Elastic Beanstalk to automatically stream logs to the CloudWatch service.
- **Server log files can also be stored in either S3 or CloudWatch Logs, and not only on the EBS volumes of the EC2 instances which are launched by AWS Elastic Beanstalk.**
- Server log files cannot be directly saved to Glacier. You can create a lifecycle policy to the S3 bucket to store the server logs and archive it in Glacier, but there is no direct way of storing the server logs to Glacier using Elastic Beanstalk unless you do it programmatically.

### Elastic Beanstalk Questions

- Where does Elastic Beanstalk store your application and server log files?
- Can you configure Elastic Beanstalk to automatically stream logs to CloudWatch?
- Where can server log files be stored?
- Can you directly save server log files to Glacier? -- what needs to be done instead?

---

# File Storage

### FSx for Lustre

- Amazon FSx for Lustre provides a high-performance file system optimized for fast processing of workloads such as machine learning, high performance computing (HPC), video processing, financial modeling, and electronic design automation (EDA). These workloads commonly require data to be presented via a fast and scalable file system interface, and typically have data sets stored on long-term data stores like Amazon S3.
- **Amazon FSx for Lustre works natively with Amazon S3.**
- When linked to an S3 bucket, an FSx for Lustre file system transparently presents S3 objects as files and allows you to write results back to S3.
- You can also use FSx for Lustre as a standalone high-performance file system to burst your workloads from on-premises to the cloud.

### FSx for Windows File Server

- Although this service is a type of Amazon FSx, it does not work natively with Amazon S3.

### EFS

- Elastic File System (Amazon EFS) provides simple, scalable file storage for use with your Amazon ECS tasks and EC2 instances.
- With Amazon EFS, storage capacity is elastic, growing and shrinking automatically as you add and remove files.
- Amazon EFS file systems can automatically scale from gigabytes to petabytes of data without needing to provision storage.
- **Tens, hundreds, or even thousands of Amazon EC2 instances can access an Amazon EFS file system at the same time,** and Amazon EFS provides consistent performance to each Amazon EC2 instance.
- With **Bursting Throughput mode**, a file system's throughput scales as the amount of data stored in the EFS Standard or One Zone storage class grows. File-based workloads are typically spiky, driving high levels of throughput for short periods of time, and low levels of throughput the rest of the time. To accommodate this, Amazon EFS is designed to burst to high throughput levels for periods of time.
- **Provisioned Throughput mode** is available for applications with high throughput to storage (MiB/s per TiB) ratios, or with requirements greater than those allowed by the Bursting Throughput mode. For example, say you're using Amazon EFS for development tools, web serving, or content management applications where the amount of data in your file system is low relative to throughput demands. Your file system can now get the high levels of throughput your applications require without having to pad your file system.
- Bursting Throughput mode won't be able to sustain the constant demand of the global application. Remember that the application will be frequently accessed by users around the world and there are hundreds of ECS tasks running most of the time.
- Although the EFS service can be used for HPC applications, **it doesn't natively work with Amazon S3.** It doesn't have the capability to easily process your S3 data with a high-performance POSIX interface, unlike Amazon FSx for Lustre.

### File Storage Questions

#### FSx for Lustre Questions

- What sort of file workloads is FSx for Lustre optimal?
- Does FSx for Lustre have native S3 integration?
- How does FSx for Lustre interact with S3?
- Can FSx for Lustre also be used as a standalone high-performance file system to burst workloads from on-prem to the cloud?

#### FSx for Windows Questions

- Does FSx for Windows natively integrate with S3?

#### EFS Questions

- Can EFS file systems scale to from gigabytes to petabytes of data without data storage needing to be provisioned?
- Roughly how many EC2 instances can access an EFS file system at the same time with persistent performance?
- What services does EFS provide simple and scalable file storage for?
- Does EFS storage capacity automatically elastically grow with your needs?
- Describe EFS Bursting Throughput mode.
- Describe EFS Provisioned Throughput mode.
- Would Bursting Throughput mode be able to sustain the constant demand of a global ECS application?
- Does EFS natively work with S3?

---

# Kinesis

- By default, records of a stream in Amazon Kinesis are accessible for up to 24 hours from the time they are added to the stream. You can raise this limit to up to 7 days by enabling extended data retention.
- Amazon Kinesis Data Streams supports resharding, which lets you adjust the number of shards in your stream to adapt to changes in the rate of data flow through the stream. Resharding is considered an advanced operation.
- There are two types of resharding operations: shard split and shard merge. In a shard split, you divide a single shard into two shards. In a shard merge, you combine two shards into a single shard. Resharding is always pairwise in the sense that you cannot split into more than two shards in a single operation, and you cannot merge more than two shards in a single operation.
- Splitting increases the number of shards in your stream and therefore increases the data capacity of the stream. Because you are charged on a per-shard basis, splitting increases the cost of your stream. Similarly, merging reduces the number of shards in your stream and therefore decreases the data capacity—and cost—of the stream.

### Kinesis Data Firehose

- **Firehose only supports Amazon S3, Amazon Redshift, Amazon Elasticsearch, and an HTTP endpoint as the destination.**
- the throughput of Kinesis Firehose is not exceptionally higher than Kinesis Data Streams. In fact, the throughput of an Amazon Kinesis data stream is designed to scale without limits via increasing the number of shards within a data stream.
- There is no Step Scaling feature for Kinesis Data Streams. This is only applicable for EC2.

---

# RDS

- RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ) that is in the same region and not in a different one.
- IAM database authentication is only supported in MySQL and PostgreSQL database engines.
- With IAM database authentication, you don't need to use a password when you connect to a DB instance but instead, you use an authentication token.
- You can force all connections to your DB instance to use SSL, or you can encrypt connections from specific client computers only.

#### RDS in-transit security

- If you want to force SSL, use the `rds.force_ssl` parameter.
- By default, the `rds.force_ssl` parameter is set to false. Set the `rds.force_ssl` parameter to `true` to force connections to use SSL.
- The `rds.force_ssl` parameter is static, so after you change the value, you must reboot your DB instance for the change to take effect.
- To use SSL from a specific client, you must obtain the certs for the client computer, import certs on the client computer, and then encrypt the connections from the client computer.

### Microsoft SQL Server

- You can use Secure Sockets Layer (SSL) to encrypt connections between your client applications and your Amazon RDS DB instances running Microsoft SQL Server.
- When you create an SQL Server DB instance, Amazon RDS creates an SSL certificate for it. The SSL certificate includes the DB instance endpoint as the Common Name (CN) for the SSL certificate to guard against spoofing attacks.
- Transparent Data Encryption (TDE) is primarily used to encrypt stored data on your DB instances running Microsoft SQL Server, and not the data that are in transit.

# OpsWorks

- creating OpsWorks recipes that will automatically launch resources containing the latest version of the code is still incorrect because you don't need to launch new resources containing your new code when you can just update the ones that are already running.

# SWF

- SWF is mainly used for managing workflows and not for monitoring and notifications.

# KMS

- Managed service, providing a centralized control over the lifecycle of your keys, to create and control encryption keys used for encrypting data.
- AWS KMS is also integrated with AWS CloudTrail to provide encryption key usage logs to help meet your auditing, regulatory and compliance needs.
- AWS KMS is integrated with AWS CloudTrail, a service that delivers log files to an Amazon S3 bucket that you designate. By using CloudTrail you can monitor and investigate how and when your master keys have been used and by whom.
- CMK contains the key material used to encrypt, and decrypt, your data.
- If you want a managed service for creating and controlling your encryption keys, but you don't want or need to operate your own HSM, consider using AWS Key Management Service.
- KMS and CloudHSM are two different services. If you want a managed service for creating and controlling your encryption keys, without operating your own HSM, you have to consider using AWS Key Management Service.
- AWS KMS automatically rotate CMK keys every year.
- Resource-based policies _must_ be attached to your customer master keys -- called key policies.
- Three ways to control permissions: using the Key Policy, use IAM policies in combination with the Key Policy, use grants in combination with the key policies.
- KMS shared tenancy of underlying hardware, Automatic key rotation, automatic key generation.
- What is the minimum length of time before you can schedule a KMS key to be deleted? 7 days.

### CMK

- Three ways to generate a CMK: key material is generated for CMK is generated within HSMs managed by AWS KMS. Import material from your own key management infrastructure and associated it with a CMK, have the key material generated and used in a AWS CloudHSM cluster as part of the custom key feature in AWS KMS.

# CloudHSM

- Physical device dedicated entirely to you.
- AWS CloudHSM is a cloud-based **hardware security module (HSM)** that enables you to easily generate and use your own encryption keys on the AWS Cloud.
- You should consider using AWS CloudHSM over AWS KMS if you require your keys stored in dedicated, third-party validated hardware security modules under your exclusive control.
- Dedicated HSM to you, full control of underlying hardware, full control of users, groups, keys, etc. No automatic key rotation.

# AWS Secrets Manager

- Secrets Manager is _not_ free.
- A service that stores, encrypts, and rotates your database credentials and other secrets.
- Encryption in transit, and at rest, using KMS (always encrypted).
- If you enable rotation, Secret Manager _immediately_ rotates the secret once to test the configuration. So make sure your application is configured to use Secrets Manager _before_ enabled credential rotation.
- If you are still using embedded credentials, do not enable rotation because the embedded credentials will no longer work and this will break your application.

# AWS Parameter Store

- Parameter store is free.
- Very similar to Secrets Manager, BUT: there is a limit to number of parameters you can store (10,000) and there is **no key rotation** with parameter store.

# DynamoDB

- Amazon DynamoDB Accelerator (DAX), which is a fully managed, highly available, in-memory cache for Amazon DynamoDB, can deliver microsecond response times.

### DAX

- DAX (DynamoDB Accelerator). In-memory cache. Reduces response time to milliseconds.
- This cache is highly available and lives inside the VPC you specify.
- You have control over DAX, you determine node size, how many in a cluster, TTL for data.

# Redshift

- Amazon Redshift only delivers sub-second response times.

# DDoS

- Layer4 SYN Flood or Amplification Attack.
- Layer7 floods of GET/POST requests.
