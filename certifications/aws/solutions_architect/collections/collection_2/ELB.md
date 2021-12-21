# Elastic Load Balancer (ELB)

If extreme performance is needed for your application, AWS recommends that you use a Network Load Balancer. Network Load Balancer operates at the connection level (Layer 4), routing connections to targets (Amazon EC2 instances, microservices, and containers) within Amazon VPC, based on IP protocol data. Ideal for load balancing of both TCP and UDP traffic, Network Load Balancer is capable of handling millions of requests per second while maintaining ultra-low latencies. Network Load Balancer is optimized to handle sudden and volatile traffic patterns while using a single static IP address per Availability Zone. It is integrated with other popular AWS services such as Auto Scaling, Amazon EC2 Container Service (ECS), Amazon CloudFormation, and AWS Certificate Manager (ACM).

- Elastic Load Balancer distributes incoming application, or network traffic, across multiple targets such as EC2 instances, containers (ECS), lambda functions, and IP addresses, **in multiple AZs**.
- ELB must be associated with one public AZ from _at least_ two AZs and you can only specify one public subnet per AZ.

### General Features

- Accepts traffic from clients and routes requests to registered targets.
- Monitors the health of its registered targets and routes traffic _only_ to healthy targets.
- While **disabled by default**, you can enable deletion protection to prevent accidental deletion of ELB.
- Deleting ELB won't delete associated instances registered to it.
- Cross Zone Load Balancing, when enabled, each load balancer node distributes traffic across the registered agents in all enabled AZs.

## Application Load Balancer

- Functions at the **Application Layer**, _the seventh layer_ of the OSI (Open Systems Interconnection) model.
- Allows HTTP and HTTPS.
- At Least two subnets need to be specified when using this type of load balancer.

#### Components

- **Load Balancer:** single point of contact for clients.
- **Listener:** checks for connection requests from clients. You must define a default rule for each listener that specifies a target group.
- **Target Group:** routes requests to one or more registered targets. Health checks exist on target groups.

## Network Load Balancer

- Functions at the **fourth layer** of the OSI model.
- Uses TCP/UDP connections.
- At least one subnet must be specified when creating this type of load balancer, but _two_ are recommended.

#### Components

- **Load Balancer:** single point of contact for clients.
- **Listener:** checks for connection requests from clients. You must define a default rule for each listener that specifies a target group.
- **Target Group:** routes requests to one or more registered targets. Health checks exist on target groups.

## Classic Load Balancer

- Distributes incoming application traffic across multiple EC2 instances in multiple AZs.
- **For use with EC2 classic only.**
- AWS recommends using Application or Network Load balancers instead of the Classic Load Balancer.

### Questions

1. What is ELB and what does it do?
1. What are the three types of load balancers in AWS?
1. Describe the Application Load Balancer.
1. Describe the Network Load Balancer.
1. Describe the Classic Load Balancer.
1. What are the core components in a Load Balancer?

### Answers

1. Elastic Load Balancing (ELB) automatically distributes incoming application traffic across multiple targets and virtual appliances in one or more Availability Zones (AZs).
