# VPC Guide

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
- In many cases, you can reduce your network costs, increase bandwidth throughput, and provide more consistent network experience than internet-based connections.
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

# Exercises

## Setup Private + Public Subnet

> You need to build a VPC that has a public and private subnet with each subnet having one EC2 instance. The VPC should be able to connect to the internet. However, for security purposes, your private subnet should not **directly** connect to the internet. Your public subnet's EC2 should support inbound SSH and HTTP requests, though the SSH traffic should only be allowed from your current computer's IP address and the private IP block range of your custom VPC. The private subnet's EC2 instance should only allow SSH, HTTP, ICMP, and Postgres protocols. SSH traffic to the private subnet's EC2 instance should only come from your current IP address or the private IP block range of the web-server public subnet. Once both subnets and their resources are set up and configured, you should be able to update the yum packages in each.

#### Resources to use:

- VPC
- EC2 (linux t2.micro free tier)
- Routers
- Internet Gateway
- NAT Gateway
- Security Groups
- NACLs

#### Tips

- Don't configure your VPC's main route table to access the internet gateway, this creates a security vulnerability due to the MRT being the default table new subnets are associated with.
- You can't SSH into private IPv4 addresses if you're outside of the given VPC network.
- When you create a subnet (public in this case), you can configure it to auto-assign public IPv4 addresses when you assign resources to it.
- You'll need to use `chmod 400` your `.pem` key if you want to ssh into either ec2 instance.

## Configure VPC Endpoints

> We want to allow our VPC's private EC2 instance to connect to S3 without going through the internet. Configure the private EC2 instance to have a full-access S3 IAM role and VPC Gateway support.

#### Resources to use:

- AWS VPC Gateway
- IAM

#### Tips

- Use IAM roles to create a role for EC2 instances giving them full-access to S3.
- The Gateway should be associated with the private subnet.

## Initiate VPC Peering

> Connect the current VPC to another VPC using VPC peering. Once the peer is established, you should be able to SSH between resources in the peer network after setting up an appropriate route table and security group configurations to support the new IP blocks.

#### Resources to use:

- AWS VPC Peering
- Security Groups
- Route Tables
