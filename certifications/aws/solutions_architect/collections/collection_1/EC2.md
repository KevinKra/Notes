## EC2 Auto Scaling Group

### Questions

1. Conceptually, what is an auto-scaling group?
1. What are some benefits of using Auto Scaling Group?
1. What is an EC2 Auto Scaling group **Lifecycle Hook?**
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
1. What can you use to prevent specific instances from being terminated during automatic scale-ins?
1. What are **launch templates?**
1. What is a **launch configuration?**

### Answers

1. An auto-scaling group is a collection, or group, of EC2 instances that share a common intention and design.
1. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies.
1. A Lifecycle hook allows your auto-scaling group to perform custom actions when instances launch or terminate.
1. Scale to maintain current instance levels, manual scaling, scale based on a schedule, and scale based on a demand.
1. Target Tracking Scaling, Step Scaling, and Simple Scaling.
1. A Cooldown period is a _configurable_ setting that helps ensure to not launch or terminate additional instances before previous scaling activities take effect.
1. **Target tracking scaling**—Increase or decrease the current capacity of the group based on a target value for a specific metric.
1. **Step scaling**—Increase or decrease the current capacity of the group based on a set of scaling adjustments, known as **step adjustments**, that vary based on the size of the alarm breach.
1. **Simple scaling**—Increase or decrease the current capacity of the group based on a single scaling adjustment.
1. Both require you set up CloudWatch alarms for scaling policies, specify high and low thresholds for the alarms, whether to add or remove instances, how many, or to set an exact size. The main difference is step adjustments increase the capacity of the group, in a varying manner, based on the size of the alarm breach.
1. The primary issue with simple scaling is that after a scaling activity is started, the policy must wait for the scaling activity or health check replacement to complete and the cooldown period to expire before responding to additional alarms.
1. No, Target tracking or step scaling policies can trigger a scaling activity immediately without waiting for the cooldown period to expire.
1. If the instance is in a state other than running, the system status is impaired, or ELB reports the instance failed health checks.
1. Scaling in is reducing instances, scaling out is increasing instances.
1. You need to set a **termination policy** to determine which instances to delete first.
1. You can use **instance protection** to prevent instances from being terminated during automatic scale-in.
1. Launch templates specify instance configuration information when you launch EC2 instances, and they allow you to have multiple versions of a template.
1. A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances, and you specify information for the instances.
