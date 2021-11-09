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
