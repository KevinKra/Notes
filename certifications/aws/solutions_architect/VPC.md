# VPC Overview

> A VPC is a virtual data center in the cloud.

## VPC Introduction

- A logically isolated part of the AWS Cloud where you can define your own network.
- Gives you complete control of the Virtual Networking, including your own IP address range, subnets, route tables, and network gateways.

## Networking

### Fully Customizable Network Example

You can leverage multiple layers of security, including security groups and network ACLs, to help control access to Amazon EC2 instances in each subnet.

#### Web

- Public-facing subnet.

#### Application

- Private subnet. Can only speak to web tier and database tier.

#### Database

- Private subnet. Can only speak to application tier.

## VPNs

## Network Structure

- Region holds a VPC(s).
- VPC holds subnet(s).
- Subnets can be public or private.
- A Subnet consists of the Instance and Security Group.
- Within a VPC, the **Router** handles incoming requests from the **Internet Gateway** or **Virtual Private Gateway** (instances from corporate data center for example). It then directs traffic to **Route Tables** that then connect to a **Network ACL** which then connects to the appropriate **Subnet**.

## VPC features

- Launch EC2 instances into a subnet of your choosing.
- Assign custom IP address ranges in each subnet.
- Configure Route tables between subnets.
- Create internet gateways and attach it to our VPC.
- Much better security control over your AWS resources.
- Create subnet Network Access Control Lists (NACLs).
- You can use NACLs to block specific IP addresses.

## Comparing VPCs

#### Default VPC

- Every account in AWS comes with Default VPCs in every region. They're user friendly but come pre-configured. All subnets in a default VPC have a route out to the internet (all public). Each EC2 instance within a default VPC has both a public and private IP address.

#### Custom VPC

- Fully customizable.
- Takes time to setup.

## Exam Tips

- A VPC is a logical data center in AWS.
- Consists of internet gateways (or virtual private gateways), route tables, network access control lists, subnets, and security groups.
- 1 subnet is _always_ in 1 AZ. They cannot span multiple AZs.

## Questions:

- What is a VPC?
- What does a custom VPC give you complete control over?
- What characteristics define a Default VPC?
- What are the key differences between a Custom and Default VPC?
- Do you use Security Groups or NACLs to block specific IP addresses?
- Does customizing your VPC potentially provide much better security to your infrastructure?
- Do AWS accounts come with default VPCs?
- What is the general structure of VPCs?
- How many subnets are in AZs?
- Can subnets span multiple AZs?
