- Smallest AWS IP netmask is 28 (16 address)
- Largest AWS IP netmask is a 16 (65,536 addresses)
- "Block sizes must be between a 16 and 28 netmask."
- Every region comes with a `Default VPC` with `Default Subnets` contained within.
- to connect instances in a private subnet to a corporate data center a `Virtual Private Gateway` could be connected to the VPC to establish a VPN connection.
- You can block **Specific IP addresses** with `NACLs`.
- all subnets in a default VPC have a route out to the internet.
- each EC2 instance, in a default subnet, has a public and private IP address.
- VPCs consist of internet gateways (or virtual private gateways), route tables, NACLs, subnets, and security groups.
- 1 subnet is always in 1 availability zone.
- If you go to a subnet's page, you can set it to auto-assign IP addresses.
- You can only have one internet gateway on a VPC.
- By default, every time you create a subnet in a VPC it will be associated with the `Main Route Table` of the VPC.
- Since your `Main Route Table` is the default table for new subnets, it's a good idea to _not_ have your default route table be internet facing.
- NAT gateways are redundant inside the AZ.
- NAT gateways have a range of 5-45 Gbps.
- NAT gateways are managed, no need to patch.
- NAT gateways are not associated with Security Groups.
- NAT gateways are automatically assigned a public IP address.

- Computers communicate through using protocols, and protocols are associated to ports. ie. port 22 is reserved for the SSH protocol.
- SGs are essentially virtual firewalls for an EC2 instance. By default everything is blocked.
- SGs are stateful, if you send a request from your instance, the response traffic is allowed to flow back in _regardless_ of inbound security group rules.

- NACLs first line of defense, acts as a firewall for one or more instances.
- NACLs can be used to block specific IP addresses.
- Subnets _can_ only be associated with one NACL at a time, yet NACLs can be associated with multiple subnets.
- NACLs have a **numbered list of rules** that are evaluated in order, starting at the lowest.
- NACLs have separate inbound and outbound rules. Each rule can either allow or deny traffic.
- NACLs are **stateless**; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).
- new NACLs deny everything (inbound/outbound) by default.
- the default NACL allows everything.
- NACL rules are established in chronological order, if a rule sets an allow/deny for a specific port and source, it will supersede all following rules to the same port.

- VPC endpoints enabled you to privately connect your VPC to support AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, nat device, vpn connection, or aws direct connect connection.
- VPC endpoints are virtual devices that are horizontally scaled, redundant, and highly available. They allow communication between instances in your VPC and services **without imposing availability risks or bandwidth constraints on your network traffic.**
- NAT gateways have a maximum amount of bandwidth, if you have EC2 instances that are communicating with S3 for example, using a VPC endpoint may actually be better to due bandwidth limitations.
- Interface Endpoints: an interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic headed to a supported service. They support a large number of AWS services.
- Gateway Endpoints: similar to NAT gateways, a gateway endpoint is a **virtual device you provision**. It supports connection to (currently) S3 and DynamoDB.

- you can open your vpc up to other vpcs by either opening it up to the internet (IGW) or using VPC peering.
- opening a vpc up to the internet comes with some security considerations. There is a lot more to manage, IGW, Route tables, etc.
- using VPC peering, you will have to create and manage many different peering relationships, including scaling.
- the whole network is accessible, this isnt good if you have multiple applications in your VPC.
- the best way to expose a service VPC to tens, hundreds, or thousands of customer VPCs is to use **PrivateLink**.
- PrivateLink doesn't require route tables, NAT gateways, Internet gateways, etc.
- PrivateLink requires a Network Load Balancer _on the service VPC_ and an ENI (Elastic Network Interface) _on the customer VPC_.

- VPC peering allows you to use multiple VPCs for different environments and connect them together. Example: Production Web VPC, Content VPC, Intranent (backend) sales.
- VPC peering allows you to connect one VPC to another via direct network route using private IP addresses.
- Instances behave as if they were on the same private network.
- You can peer VPCs with other AWS accounts as well with other VPCs in the same account.
- Peering is in a star configuration, e.g., one central VPC peers with 4 others. **No transitive peering!**
- You can peer between regions.
- You cannot communicate through the center star to another VPC, if you need to communicate with a sibling on another end of the star center, you need to create a direct peering connection between the two VPCs in question. No transitive peering means you cant talk through the middle connector VPC. **Must be hub-and-spoke model.**
- You cannot have overlapping CIDR address ranges between peering VPCs.

- VPN CloudHub, if you have multiple sites, each with its own VPN connection, you can use AWS VPN CloudHub to connect those sites together.
- CloudHub uses the hub-and-spoke model.
- Low cost and easy to manage.
- VPN CloudHub operates over the public internet, but all traffic between the customer gateway and the AWS VPN CloudHub is encrypted.

- Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS.
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

- Transit Gateway, dramatically simplifies network topologies.
- Transit Gateway connects your on-premise networks through a central hub. This simplifies your network and puts an end to complex peering relationships. It acts as a cloud router -- each new connection is only made once.
- Allows you to have transitive peering between thousands of VPCs and on-premise data-centers.
- Works with the hub-and-spoke model.
- Works on a regional basis, but you can have it across multiple regions.
- You can use it across multiple AWS accounts using RAM (Resource Access Manager.)
- You can use route tables to limit how VPCs talk to one another.
- Transit Gateway works with Direct Connect as wel as VPN connections.
- Supports IP multicast (not supported by any other AWS service).

ET

- If you have resources in multiple AZs and they share a NAT gateway, in the event that the NAT gateway's AZ goes down, resources in other AZs will lose internet access.
- To create an AZ-independent architecture, create a NAT gateway in each AZ and configure your routing to ensure resources use the NAT gateway in the same AZ.
