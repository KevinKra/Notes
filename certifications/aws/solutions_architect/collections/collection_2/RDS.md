Failover is automatically handled by Amazon RDS so that you can resume database operations as quickly as possible without administrative intervention. When failing over, Amazon RDS simply flips the canonical name record (CNAME) for your DB instance to point at the standby, which is in turn promoted to become the new primary. We encourage you to follow best practices and implement database connection retry at the application layer. Failovers, as defined by the interval between the detection of the failure on the primary and the resumption of transactions on the standby, typically complete within one to two minutes. Failover time can also be affected by whether large uncommitted transactions must be recovered; the use of adequately large instance types is recommended with Multi-AZ for best results. AWS also recommends the use of Provisioned IOPS with Multi-AZ instances for fast, predictable, and consistent throughput performance.

### Questions

1. In AWS, do RDS failovers require administrative intervention?
1. In the event of a failover, what does AWS update to set a standby instance as the primary?
1. What does CNAME stand for?
1. Failovers, defined by the interval between the detection of the failure on the primary and the resumption of transactions on the standby RDS instance typically take how long?

### Answers

1. No, AWS automatically handles RDS failovers.
1. Amazon RDS updates the CNAME of your DB instance to point to the standby, thus promoting it to become the primary.
1. CNAME stands for Canonical Name Record.
1. The process of failing over to the standby RDS instance usually takes about one to two minutes.
