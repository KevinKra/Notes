# EC2 / Application Auto Scaling Group

https://tutorialsdojo.com/aws-auto-scaling/?src=udemy#features

1. What is an **Auto Scaling Group?**

- An Auto Scaling group contains a collection of Amazon EC2 instances treated as a logical grouping for the purposes of automatic scaling and management. n Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service. The size of an Auto Scaling group depends on the number of instances that you set as the desired capacity. You can adjust its size to meet demand, either manually or by using automatic scaling.

1. What are the two policies for an Auto Scaling Group? Describe their similarities and differences.

- **Step Scaling policies** and **Simple Scaling policies**. Both require CloudWatch alarms for the scaling policies and both require high and low thresholds for the alarms. Additionally, both require you to define whether to add or remove instances, how many, or set the group to an exact size.

- **The main difference between the policy types is the step adjustments that you get with step scaling policies.** When step adjustments are applied, and they increase or decrease the current capacity of your Auto Scaling group, the adjustments vary based on the size of the alarm breach.

#### Target Tracking Scaling

- With a target tracking scaling policy, you can increase or decrease the current capacity of the group **based on a target value for a specific metric.** This policy will help resolve the _over-provisioning_ of your resource. **The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value.** In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern.

#### Simple Scaling

- **Simple scaling requires you to wait for the cooldown period to complete before initiating additional scaling activities.** Target tracking or Step Scaling policies can trigger scaling activity immediately without waiting for the cooldown period to expire.

## EC2 Auto Scaling Group

### Q

1. Conceptually, what is an auto scaling group?
1. What are some benefits of using Auto Scaling Group?
1. What is a EC2 Auto Scaling group **Lifecycle Hook?**
1. What are the four EC2 Auto Scaling Group options?
1. What are the three EC2 Auto Scaling Group Policy Types?
1. What is an EC2 Auto Scaling Group **cooldown period?** What is the only policy type that has a cooldown period?
1. Describe Target Tracking Scaling.
1. Describe Step Scaling.
1. Describe Simple Scaling.
1. The dynamic scaling options, step and simple scaling, have what similarities and differences?
1. What is the primary issue with simple scaling?
1. Do Target Tracking or Step Scaling need to wait for the cooldown period to complete before initiating additional scaling activities?
1. Under what circumstances (3) does EC2 Auto Scaling mark an instance as unhealthy?
1. What is scale-in and scale-out?
1. What do you need to determine when you configure automatic scale in rules?
1. What can you use to prevent specific instances from being terminated during automatic scale ins?
1. What are **launch templates?**
1. What is a **launch configuration?**

### A

1. An auto scaling group is a collection, or group, of EC2 instances that share a common intention and design.
1. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies.
1. A Lifecycle hook allows your auto scaling group to perform custom actions when instances launch or terminate.
1. Scale to maintain current instance levels, manual scaling, scale based on a schedule, and scale based on a demand.
1. Target Tracking Scaling, Step Scaling, and Simple Scaling.
1. A Cooldown period is a _configurable_ setting that helps ensure to not launch or terminate additional instances before previous scaling activities take effect.
1. **Target tracking scaling**—Increase or decrease the current capacity of the group based on a target value for a specific metric.
1. **Step scaling**—Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as **step adjustments**, that vary based on the size of the alarm breach.
1. **Simple scaling**—Increase or decrease the current capacity of the group based on a single scaling adjustment.
1. Both require you setup CloudWatch alarms for scaling policies, specify high and low thresholds for the alarms, whether to add or remove instances, how many, or to set an exact size. The main difference is step adjustments increase the capacity of the group, in a varying manner, based on the size of the alarm breach.
1. The primary issue with simple scaling is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms.
1. No, Target tracking or step scaling policies can trigger a scaling activity immediately without waiting for the cooldown period to expire.
1. If the instance is in a state other than running, the system status is impaired, or ELB reports the instance failed health checks.
1. Scaling in is reducing instances, scaling out is increasing instances.
1. You need to set a **termination policy** to determine which instances to delete first.
1. You can use **instance protection** to prevent instances from being terminated during automatic scale in.
1. Launch templates specify instance configuration information when you launch EC2 instances, and they allow you to have multiple versions of a template.
1. A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances, and you specify information for the instances.

## Application Auto Scaling

### Q

1. What are the three approaches to Application Auto Scaling?
1. Explain Target tracking scaling. How does it differ from the similarly named EC2 Scaling solution of the same name?
1. What does Schedule Scaling use to scale resources?
1. Can you have multiple target tracking policies for a scalable target?

### A

1. Target tracking scaling, Step scaling, and Scheduled scaling.
1. Application Target scaling scales a resource based on a target value for a specific CloudWatch metric. Unlike EC2 Target tracking, Application Target Tracking uses data points derived from CloudWatch to trigger scaling events.
1. Scheduled Scaling uses dates/time to determine scaling.
1. Yes, you can have multiple target tracking policies for a scalable target, provided they each use different metrics.

---

## Monitoring

### Q

1. Does autoscaling provide health checks on instances in standby state?
1. What does CloudWatch metrics enable?

### A

1. Nope. Standby state can be used for performing updates/changes/troubleshooting without health checks being performed or replacement instances being launched.
1. CloudWatch metrics enables you to retrieve statistics about Auto Scaling-published data points as an ordered set of time-series data, known as metrics. You can use these metrics to verify that your system is performing as expected.

# CloudTrail

### Q

1. What is an **event** in CloudTrail?
1. What are the two types of events that can be logged in CloudTrail?
1. Can a trail be applied to one region or all regions? What the best practice regarding this?
1. Where are events logged, where are events logged for global services?
1. CloudTrail, with multi-region trail enabled, only enables the tracking of regional services (EC2, S3, RDS, etc.) and not global services like IAM, CloudFront, WAF, R53, and so on. What do you need ot to to track global services with multi-region CloudTrail?

### A

1. An event in CloudTrail is the record of an activity in an AWS account. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail.
1. Management events and data events. By default, trails log management events, but not data events.
1. A trail can be applied to all regions or a single region. As a best practice, create a trail that applies to all regions in the AWS partition in which you are working. This is the default setting when you create a trail in the CloudTrail console.
1. For most services, events are recorded in the region where the action occurred. For global services such as AWS Identity and Access Management (IAM), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East (N. Virginia) Region.
1. In order to satisfy the requirement, you have to add the `--include-global-service-events` parameter in your AWS CLI command.

# Amazon API Gateway
