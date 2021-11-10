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

1. A virtual network dedicated to your AWS account.
1. Amazon VPC is the networking layer for EC2.
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
