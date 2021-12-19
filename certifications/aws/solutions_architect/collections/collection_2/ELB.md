# Elastic Load Balancer (ELB)

If extreme performance is needed for your application, AWS recommends that you use a Network Load Balancer. Network Load Balancer operates at the connection level (Layer 4), routing connections to targets (Amazon EC2 instances, microservices, and containers) within Amazon VPC, based on IP protocol data. Ideal for load balancing of both TCP and UDP traffic, Network Load Balancer is capable of handling millions of requests per second while maintaining ultra-low latencies. Network Load Balancer is optimized to handle sudden and volatile traffic patterns while using a single static IP address per Availability Zone. It is integrated with other popular AWS services such as Auto Scaling, Amazon EC2 Container Service (ECS), Amazon CloudFormation, and AWS Certificate Manager (ACM).

### Questions

1. What is ELB and what does it do?
1. What are the three types of load balancers in AWS?
1. Describe the Network Load Balancer.
1. Describe the Classic Load Balancer.
1. Describe the Application Load Balancer.

### Answers

1. Elastic Load Balancing (ELB) automatically distributes incoming application traffic across multiple targets and virtual appliances in one or more Availability Zones (AZs).
