### Q

#### Five Pillars of a Well Architected Framework.

1. What are the five pillars of a "Well-Architected Framework"?
1. Describe the pillar Operation Excellence, what are it's design principles?
1. Describe the pillar Security, what are it's design principles?
1. Describe the pillar Reliability, what are it's design principles?
1. Describe the pillar Performance Efficiency, what are it's design principles?
1. Describe the pillar Cost Optimization, what are it's design principles?

#### Best Practices when Architecting in the Cloud

1. What is a **Data Lake?**
1. For data storage, what is the difference between Synchronous, Asynchronous, and Quorum-based replication?

#### Disaster Recovery in AWS

1. What is RTO?
1. What is RPO?
1. Describe Backup and Restore.
1. Describe Pilot Light.
1. Describe Warm Standby.
1. Describe Multi-Site.
1. Does AWS have a suggested disaster recovery tool?

---

### A

#### Five Pillars of a Well Architected Framework.

1. Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization.
1. **Operational Excellence** is the ability to support development and run workloads effectively, gain insight into their operations, and to continuously improve supporting processes and procedures to deliver business value. _Design principles: Perform operations as code, make frequent and small reversible changes, refine operation procedures frequently, learn from operational failures._
1. **Security** is the ability to protect data, systems, and assets to take advantage of cloud technologies to improve your security. _Design principles: implement a strong identity federation, enable traceability, apply security at all layers, automates security best practices, protect data in transit and at rest, keep people away from data, prepare for security events._
1. **Reliability** is the ability of a workload to perform its intended function correctly and consistently when it’s expected to. This includes the ability to operate and test the workload through its total lifecycle. _Design principles: automate recover from failure, test recovery procedures, scale horizontally to increase aggregate workload availability, stop guessing capacity, manage change in automation._
1. **Cost Optimization** the ability to run systems to deliver business value at the lowest price point. _Design principles: implement Cloud Financial Management, adopt a consumption model, measure overall efficiency, stop spending money on undifferentiated heavy lifting, analyze and attribute expenditure._

#### Best Practices when Architecting in the Cloud

1. A **Data Lake** is an architectural approach that allows you to store massive amounts of data in a central location so that it's readily available to be categorized, processed, analyzed, and consumed by diverse groups within your organization.
1. **Synchronous replication** only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node. **Asynchronous replication** decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas. **Quorum-based replication** combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.

#### Disaster Recovery in AWS

1. RTO, or **Recovery Time Objective**, is _the time it takes_ after a disruption to restore a business process to its service level.
1. RPO, or **Recovery Point Objective**, is _the acceptable amount of data loss_ measured in time.
1. **Backup and Restore** involves taking frequent backups of your most critical systems. Once disaster strikes you simply restore these backups to recover data and quickly. B&R usually has the longest RTO and your RPO will depend on how frequently you backup your data.
1. **Pilot Light** provides quicker recovery time than backup and restore because core pieces of the system are already running and are continually kept up to date. Data loss is very minimal in this scenario for the critical parts, but for the other the less critical parts, you have the same RTO and RPO as backup and restore.
1. **Warm standby** provides a scaled-down version of a fully functional environment that is always running. For example, you have a subset of undersized servers and databases that have the same exact configuration as your primary, and are constantly updated also. Once disaster strikes, you only have to make minimal reconfigurations to re-establish the environment back to its primary state. Warm standby is costlier than Pilot Light, but you have better RTO and RPO.
1. **Multi-Site** runs exact replicas of your infrastructure in an active-active configuration. In this scenario, all you should do in case of a disaster is to reroute traffic onto another environment. Multi-site is the most expensive option of all since you are essentially multiplying your expenses with the number of environment replicas. It does give you the best RTO and RPO however.
1. Yes, AWS promotes their disaster recovery tool called **CloudEndure** which they are suggesting to their customers as the preferred solution for disaster recovery workloads.
