# What is Amazon VPC

## Questions:

#### What is Amazon VPC

> https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html

1. What is Amazon VPC?
1. What is Amazon VPC the networking layer for?
1. What is a subnet?
1. What is a route table?
1. What is an internet gateway?
1. What is a VPC endpoint?
1. What is a CIDR block?
1. Is there additional charge for using VPCs?
1. Are there quotas for Amazon VPC?

## Answers:

#### What is Amazon VPC

1. A virtual network dedicated to your AWS account that provides isolated networks for your infrastructure.
1. Amazon VPC is the networking layer for EC2 and other instances.
1. A range of IP addresses in your VPC.
1. A set of rules, called routes, that are used to determine where network traffic is directed.
1. A gateway that you attach to your VPC to enable communication between resources in your VPC and the internet.
1. A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services without requiring an internet gateway, NAT device, VPN, or AWS Direct Connect connection.
1. Classless Inter-Domain Routing. An internet protocol address and route aggregation methodology.
1. No, there is no additional charge for using a VPC. However, there are charges for some VPC components, such as NAT gateways, Reachability Analyzers, and traffic mirroring.
1. Yes, there are quotas for the number of Amazon VPC components that you can provision. You can request an increase for some of these components.

---

# How Amazon VPC works

## Questions

### How Amazon VPC works

1. Are VPCs logically isolated from other Virtual Networks on the cloud?
1. Where do you launch resources into on the VPC?
1. What is the difference between a public and private subnet?
1. What can you use to protect AWS resources in each subnet?
1. Are you required to associate an IPv6 CIDR block to your VPC?

#### Default and non-default VPCs

1. What is a default VPC?
1. What is a non-default VPC?

#### Route Tables

1. What is a Route Table?
1. What happens if you don't explicitly associate a subnet with a particular route table?
1. What does each route in a route table specify?

#### Access to the Internet

1. What is the configuration for the default VPC?
1. What configurations do the default subnets have?
1. What configurations do EC2 instances launched on the default subnet have?
1. What AWS service is leveraged to allow internet gateways to connect to the internet?
1. Do EC2 instances, launched into a **non-default** subnet (non-default VPC), have public and private IPv4 addresses?
1. How can you enable internet access for an EC2 instance launched into a non-default subnet?
1. What can you use to allow EC2 instances in your non-default VPC to initiate outbound connections to the internet (egress) but prevent unsolicited inbound (ingress) connections from the internet?
1. If you associate an IPv6 CIDR block with your VPC and assign IPv6 addresses to your instances, can those instances connect to the internet over IPv6 over internet gateway?

#### Access a corporate or home network

1. What are the key components required to connect the AWS cloud to your home or corporate network?

#### Connect VPCs and networks

1. What is a _peering connection_?
1. What is a _transit gateway_?

#### AWS private global network considerations

1. What is the AWS private global network?

## Answers

#### How Amazon VPC works

1. Yes. VPCs are logically isolated from other virtual networks in the AWS cloud.
1. You launch resources into subnets, which are a given range of IP addresses within a VPC.
1. A public subnet is for resources intended to be connected to the internet, whereas a private subnet is for resources that are _not_ intended to be connected to the internet.
1. You can use multiple layers of security, including Security Groups and Access Control Lists (ACLs).
1. No. IPv6s associations are _optional_.

#### Default and non-default VPCs

1. A default VPC is an VPC that is _automatically_ created for your AWS account and **has a default subnet in each availability zone.**
1. You can create your own VPC (no-default VPC) with custom configurations. Subnets created here are considered _non-default subnets._

#### Route Tables

1. A _route table_ contains a set of rules, called routes, that are used to determine where you network traffic from your VPC is directed.
1. If a route table is not _explicitly_ attached to a subnet, **it _implicitly_ defaults to the main route table.**
1. Each route in a route table specifies the range of IP addresses where you want the traffic to go (destination) and the gateway, networking interface, or connection through which to send the traffic (the target).

#### Access to the Internet

1. The default VPC includes an internet gateway (defaults with internet access).
1. Each default subnet is a public subnet.
1. EC2 instances launched on a public subnet have a private IPv4 and public IPv4 address. These instances communicate to the internet through the internet gateway.
1. Amazon's EC2 network edge allows VPC internet gateways to connect to the internet.
1. No, EC2 instances in non-default VPCs _by default_ only launch into private subnets. **They do not come with a public subnet, unless you specifically assign one at launch**, or otherwise configure it. As such, instances launched into a non-default VPC can only communicate privately amongst one another, but cannot access the internet.
1. You can enable internet access to EC2 instances launched in a non-default subnet by attacking a **internet gateway** to its (non-default) VPC and associating an Elastic IP address with the instance.
1. NAT (Network Address Translation) devices maps multiple private IPv4 addresses to a _single_ public IPv4 address. You can configure the NAT device to an Elastic IP address and connect it to the internet through an internet gateway.
1. Yes. Though, IPv6 traffic is separate from IPv4 traffic; route tables must include separate routes for IPv6 traffic.

#### Access a corporate or home network

1. A Site-to-Site VPN consists of two VPN tunnels between a virtual private gateway, or transit gateway on the AWS side, and a customer gateway device located at your data center.

#### Connect VPCs and networks

1. A peering connecting between to AWS VPCs enables you to route traffic between them **privately**. EC2 instances on either VPC can communicate with each other as if they're on the same network.

1. A transit gateway is used to interconnect your VPCs and on-premise networks. The transit gateway acts a sa regional virtual router for traffic flowing between its attachments, which can include VPCs, VPN connections, AWS Direct Connect gateways, and transit gateway peering connections.

#### AWS private global network considerations

1. The AS private global network is the infrastructure that AWS is built upon. AWS provides a high-performance, and low latency global network that delivers a secure cloud computing environment. AS regions are connected to multiple ISPs as well as to a private global network backbone.

---

# Example VPC configurations

> https://docs.aws.amazon.com/vpc/latest/userguide/example-vpc-share.html

## Questions

#### Sharing public and private subnets

1. What was the structure of the example provided in the (above linked) example that allowed multiple accounts within an organization to create isolated, but connected, private and public subnets within a tailored non-default VPC wrapper?

## Answers

1. In the scenario provided, they created an account (Account A) that was responsible for the VPC infrastructure. Accounts B and C, both want to create private applications that don't connect to the internet. While Account D has public facing applications. Account A (VPC owner) can use the **AWS Resource Access Manager** to create a **Resource Share** for the subnets (the private an public subnets created in the VPC) and then share the subnets. Account A shares the _private subnet_ with Account B and C and the _public subnet_ with Account D. These accounts (B,C, and D) can now only see and create resources on the subnets shared with them. Additionally, each of these accounts can control their resources, including instances and security groups.

---

## Other

# Q

1. Security Groups provide security on what level, how is that different from NACLs?
1. Can you apply Security Groups and NACLs rules on the same resources within a VPC?
1. What does the `Route Table` do?
1. What does a `NAT Gateway` do?
1. How many IP addresses are reserved by AWS for subnets?
1. Do nondefault subnets have the IPv4 public addressing attribute set to false?
1. How many Internet Gateways can you have attached to a VPC?
1. If you create a subnet and don't associate it to a Route Table within a VPC what does it get assigned to?
1. Why is it a bad idea to have your Main Route Table internet accessible?

# A

1. Security Groups are applied on a per-instance level, when they're created they are attached to instance(s). NACLs apply access restrictions to the entire subnet that instances may live in.
1. Yes, they can both work together to add layers of security to your instances.
1. The `Route Table` controls routing of outgoing Network Requests.
1. A `NAT Gateway` translates internal IPs into public ones (NAT Gateway has a public IP) and forwards it to the Internet Gateway. It allows private subnets to remain private and use the NAT Gateway as a proxy to reach the internet.
1. AWS reserves 5 IP addresses on subnets.
1. Yes, nondefault subnets (unless generated automatically by another service, like EC2) default to private IPv4 settings.
1. 1 Internet Gateway per VPC.
1. The subnets will be assigned, by default, to the Main Route Table.
1. If your Main Route Table is internet accessible, that means your subnets if not set, will default to the MRT and become internet accessible. Not ideal.

---

# Core Networking Components

## Network Access Control List (nACL)

A Network Access Control List (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

### Network ACL basics:

- Your default VPC automatically comes with a modifiable Network ACL. This default nACL allows _all_ outbound and inbound traffic.
- You can create custom nACLs. By default, each _custom_ nACL denies **all** inbound and outbound traffic until you add rules.
- Each subnet on your VPC _must be_ associated with a nACL. If you don't explicitly associate a subnet with a nACL, then the subnet is automatically associated with the default network ACL.
- Use nACLs, _not security groups,_ to block IP addresses.
- A nACL can be associated with multiple subnets. However, a subnet can _only_ be associated with 1 network ACL at a time.
- Network ACLs contain a numbered list of rules that are evaluated in order, starting with the **lowest** numbered rule.
- nACLs have **separate** inbound and outbound rules, and each rule can either allow or deny traffic.
- nACLs are **stateless**, responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).
- There are quotas (limits) for the number of network ACLs per VPC, and the number of rules per network ACL.

### Questions:

- What is a nACL? What does it act like?
- Is a nACL a required layer of security?
- Does your default VPC automatically come with nACL? What are its characteristics?
- Do all subnets need to be associated with an nACL?
- If you don't explicitly associate a subnet with a nACL, what happens?
- What would you use to block specific IP addresses?
- A nACL can be associated with how many subnets?
- a subnet can be associated with how many nACLs?
- In which order are nACL rules evaluated?
- Do nACLs have joined or separate inbound / outbound rules?
- Are nACLs stateless? What does this mean?
- Are there limits for the amount of ACLs on a VPC?
- Are there limits for the number of rules per network ACL?

## NAT Devices

You can use a NAT device to allow instances in **private subnets** to connect to the internet, other VPCs, or on-premise networks.

The NAT device **replaces** the source IPv4 address of the instances with the address of the NAT device. When sending response traffic to the instances, the NAT device translates the addresses back to the original source IPv4 addresses (your instances).

- NAT stands for Network Address Translation service.
- Allows outbound communication for instances within a private subnet, but external services cannot initiate a connection with those private instances.
- There are two connectivity types for NAT gateways, **Public** (default) and **Private**.

#### Public Connectivity

instances in private subnets can connect to the internet through a public NAT gateway, but cannot receive unsolicited inbound connections. Replaces the instance's source IP address with the **elastic IP address** of the NAT gateway.

#### Private Connectivity

instances in private subnets can connect to other VPCs or on-premise networks through a private NAT gateway. Replaces the instance's source IP address with the **private IP address of the NAT gateway.**

### NAT Gateway

A managed NAT device offered by AWS.

- The alternate, "NAT instance", is a personally create NAT device, AWS recommends using the NAT Gateway.
- NAT devices are not supported for IPv6 traffic.
- Redundant inside of the AZ.
- Starts at 5 GB/s scales up to 45 GB/s.
- No need to patch.
- Not Associated with Security Groups.
- Automatically assigned public IP address.
- **High Availability with NAT Gateways**: if you have resources in multiple AZs and they share a NAT gateway, in the event that the NAT Gateways AZ goes don, resources in other AZs (using that NAT Gateway) will lose internet access. To create AZ-independent architecture, create A NAT gateway in _each_ AZ and configure routing to ensure resources in each AZ use their own NAT gateway.

### NAT Gateway basics:

- A NAT gateway supports the following protocols: TCP, UDP, ICMP.
- If you have resources in multiple AZs that share one NAT gateway if the NAT gateway's AZ is down, resources bound to it in other AZs will lose internet access.
- In order to create AZ independent architecture, create a NAT gateway in each AZ and configure your routing to ensure resources in AZs use the NAT gateway in the same AZs.
- NAT gateways are **not** supported for IPv6 traffic -- use outbound-only (egress-only) internet gateway instead.
- A NAT gateway supports 5 Gbps of bandwidth and _automatically_ scales to 45 Gbps. If you require more bandwidth, you can split your resources into multiple subnets and create a NAT gateway in each subnet.
- A NAT gateway scales automatically from 1-million packets per second up to 4-million packets per second. Beyond this limit, a NAT gateway will begin to drop packets. To prevent packet loss, split resources into multiple subnets with separate NAT gateways per subnet.
- You can associate **one** Elastic IP address with a public NAT gateway. You _cannot_ disassociate an Elastic IP address from a NAT gateway after it's created.
- In order to use a different Elastic IP address for your public NAT gateway, you must create a new NAT gateway with the required address, update your route tables, and delete the existing NAT gateway.
- A private NAT gateway receives an available private IP address from the subnet in which it is configured. You cannot detach this address, or add additional addresses.
- You cannot associate a security group with a NAT gateway.
- A NAT gateway can support up to 55,000 simultaneous connections to each unique destination.
- You _cannot_ route traffic to a NAT gateway through a VPC peering connection, a Site-to-Site VPN connection, or AWS Direct Connect. A NAT gateway cannot be used by resources on the other side of these connections.

### Questions:

- What does a NAT gateway replace?
- What does NAT stand for?
- What purpose do NAT gateways serve?
- Can external services initiate a connection with private instances behind a NAT device?
- What are the two types of connectivity for NAT devices?
- What are public connectivity NAT devices designed for?
- What do public connectivity NAT replace the instance source IP address with?
- What are private connectivity NAT devices designed for?
- What do private connectivity NAT replace the instance source IP address with?
- What three internet protocols do NAT gateways support?
- If multiple resources share the same NAT gateway and it's AZ goes down, what will happen to the instances using the NAT gateway?
- How can you configure your NAT gateways to ensure they're resilient to AZ outages?
- Do NAT gateways support IPv6?
- What is the Gbps bandwidth range that NAT devices support? Where does it start, what does it scale to?
- What is the packets-per-second range that NAT gateways support?
- How can you optimize your NAT gateways architecture you get more range/performance for Gbps and bandwidths?
- How many Elastic IP addresses can you configure to a public NAT gateway?
- What are the differences between public and private NAT gateways?
- Can you associate a security group with a NAT gateway?
- Can you route traffic to a NAT gateway from a VPC peering connection?

## Security Groups

- Security Groups are stateful. If you send a request from your instance, the _response traffic_ for that request **is allowed to flow in regardless of inbound security group rules.** In other words, if you send something out from a resource, Security Group rules will _not_ block response inbound traffic.
- Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.

## Elastic IPs

## Route Tables

A route table contains a set of rules, called _routes_, that are used to determine where network traffic from your subnet or gateway is directed.

### Route Table Concepts

- **Main Route Table:** the route table the automatically comes with your VPC. It controls the routing for all the subnets that are not **explicitly associated with any other route table.**

## Direct Connect

- Connects directly to your data center to AWS.
- Useful for high-throughput workloads (lots of traffic).
- Helpful for when you need a stable and secure connection.

## VPC Endpoints

- When you want to connect AWS services _without_ leaving the Amazon internal network.
- Two types of VPC Endpoints: interface endpoints and gateway endpoints.
- Gateway Endpoints: Support S3 and DynamoDB.

## VPC Peering

- Allows you to connect 1 VPC to another via a direct network route using private IP addresses.
- Instances behave as if they were on the same private network.
- You can peer VPCs with other AWS accounts as well as with other VPCs on the same account.
- Peering is in a star configuration, with 1 central VPC peering with 4 others.
- No transitive peering!
- You can peer VPCs between regions.

## AWS PrivateLink

- PrivateLink is used to peer VPCs to tens, hundreds, or thousands of customer VPCs.
- Doesnt require VPC peering; no route tables, NAT gateways, internet gateways, etc.
- Requires a Network Load Balancer on the service VPC and an ENI on the customer VPC.

## AWS Transit Gateway

- You can use route tables to limit how VPCs talk to one another.
- Works with Direct Connect as well as VPN connections.
- Supports IP multicast.
- Simplifies network typology.

## VPN Hub

-
