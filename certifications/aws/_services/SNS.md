# SNS

https://tutorialsdojo.com/amazon-sns/

### Questions

1. What kind of messaging paradigm is SNS?
1. Does SNS _push_ notifications or have them _poll_ SNS for updates?
1. SNS is an **event-driven** computing hub, what is **event-driven computing**?
1. What is **message filtering?**
1. What is **message fanout?**
1. Is SNS message fanout async? What kind of processing does this allow?
1. What is **SNS mobile notifications?**
1. What is **SNS TTL**?
1. What is the relationship between a publisher, topic, and subscribers?
1. What do subscribers do?
1. Do publishers include specific destination addresses in each message?
1. Does SNS log the delivery status of notifications sent to topics to certain SNS endpoints?
1. Message filtering in SNS is written in what language?
1. Does SNS retry delivering a message if it initially fails?
1. Does SNS provide encrypted topics?
1. Does SNS support VPC endpoints?
1. Does SNS support access control policies?

### Answers

1. SNS follows the pub-sub (publish-subscribe) messaging paradigm.
1. SNS _pushes_ notifications to clients.
1. **Event-driven computing** is a model in which subscriber services automatically perform work in response to events triggered by publisher services. It can automate workflows while decoupling the services that collectively and independently work to fulfil these workflows.
1. **Message filtering** allows a subscriber to create a filter policy, so that it only gets notifications it cares about.
1. **Message fanout** occurs when a message is sent to a topic and then replicated and pushed to multiple endpoints.
1. Fanout provides asynchronous event notifications, which in turn allows for parallel processing.
1. **SNS mobile notifications** allows you to fanout mobile push notifications to iOS, Android, Fire OS, Windows and Baidu-based devices. You can also use SNS to fanout text messages (SMS) to 200+ countries and fanout email messages (SMTP).
1. SNS allows you to set a **TTL (Time to Live)** value for each message. When the TTL expires for a given message that was not delivered and read by an end user, the message is deleted.
1. A publisher (some service) produces and sends a message to an SNS topic which then asynchronously sends a message to subscriber(s).
1. Subscribers consume (receive the message/notification) over one of the supported protocols when they are subscribed to a topic.
1. No, a publisher sends a message to a **topic.** SNS matches the topic to a list of subscribers _who have subscribed to that topic_, and delivers the message to subscriber.
1. Yes, these endpoints include Application(?), HTTP, Lambda, SQS, and Kinesis Data Firehose.
1. In SNS, message filtering is written in JSON.
1. Yes. If a message cannot be successfully delivered on the first attempt, SNS implements a 4-phase retry policy: 1) retries with no delay in between attempts, 2) retries with some minimum delay between attempts, 3) retries with some back-off model (linear or exponential), 4) retries with some maximum delay between attempts.
1. SNS provides encrypted topics to protect your messages from unauthorized and anonymous access. The encryption takes place on the server side.
1. SNS supports VPC Endpoints via AWS PrivateLink. You can use VPC Endpoints to privately publish messages to SNS topics, from a VPC, without traversing the public internet.
1. Using access control policies, you have detailed control over which endpoints a topic allows, who is able to publish to a topic, and under what conditions.

- VPN port? 3386 or 22? UDP/TCP?
- Application Load Balancer - Network Load Balancer?
- AWS DataSync?
- ECS target group | Cluster, ALB endpoint
- Aurora rep latency less that 1 sec
- CloudFormation templates
- ec2 enhanced networking reasons
- transport layer 4 (TCP NLB), layer 7 (HTTP/HTTPS ALB)
- Route 53 weighted routing policy
- Elastic Beanstalk app files (s3) and server logs
- distinct department aws isolation
- lambda timeout
