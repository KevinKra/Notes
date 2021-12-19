# Disaster and Recovery

### Questions

1. For data storage, what is synchronous replication?
1. For data storage, what is asynchronous replication?
1. For data storage, what is quorum-based replication?
1. What is RTO?
1. What is RPO?
1. Describe Backup and Restore.
1. Describe Pilot Light.
1. Describe Warm Standby.
1. Describe Multi-Site.
1. Does AWS have a suggested disaster recovery tool?

### Answers

1. **Synchronous replication** only acknowledges a transaction after it has been durably stored in both the primary storage and its replicas. It is ideal for protecting the integrity of data from the event of a failure of the primary node.
1. **Asynchronous replication** decouples the primary node from its replicas at the expense of introducing replication lag. This means that changes on the primary node are not immediately reflected on its replicas.
1. **Quorum-based replication** combines synchronous and asynchronous replication by defining a minimum number of nodes that must participate in a successful write operation.
1. RTO, or **Recovery Time Objective**, is _the time it takes_ after a disruption to restore a business process to its service level.
1. RPO, or **Recovery Point Objective**, is _the acceptable amount of data loss_ measured in time.
1. **Backup and Restore** involves taking frequent backups of your most critical systems. Once disaster strikes you simply restore these backups to recover data. B&R usually has the longest RTO and your RPO will depend on how frequently you backup your data.
1. **Pilot Light** (active-passive) provides quicker recovery time than backup and restore because core pieces of the system are already running and are continually kept up to date. Data loss is very minimal in this scenario for the critical parts, but for the other the less critical parts, you have the same RTO and RPO as backup and restore.
1. **Warm standby** (active-passive) provides a scaled-down version of a fully functional environment that is always running. For example, you have a subset of undersized servers and databases that have the same exact configuration as your primary, and are constantly updated also. Once disaster strikes, you only have to make minimal reconfigurations to re-establish the environment back to its primary state. Warm standby is costlier than Pilot Light, but you have better RTO and RPO.
1. **Multi-Site** runs exact replicas of your infrastructure in an active-active configuration. In this scenario, all you should do in case of a disaster is to reroute traffic onto another environment. Multi-site is the most expensive option of all since you are essentially multiplying your expenses with the number of environment replicas. It does give you the best RTO and RPO however.
1. Yes, AWS promotes their disaster recovery tool called **CloudEndure** which they are suggesting to their customers as the preferred solution for disaster recovery workloads.
