# Disaster Recovery

- Recovery Point Objective (RPO): How much data can you afford to recreate or lose?
- Recovery Time Objective (RTO): How quickly must you recover? What is the cost of downtime? 
- RTO is the maximum acceptable delay between the interruption of service and restoration of service  

## Availability

- MTBF: Mean Time Between Failures
- MTTR: Mean Time to Recover
- Availability = MTBF / MTBF + MTTR
- Availability = Successful Responses / Valid Requests

> Your disaster recovery strategy requires different approaches than
those for availability, focusing on deploying discrete systems to multiple locations, so that you can fail
over the entire workload if necessary.

> In addition to data, you must also back up the configuration and infrastructure necessary to redeploy
your workload and meet your Recovery Time Objective (RTO). AWS CloudFormation provides
Infrastructure as Code (IaC), and enables you to define all of the AWS resources in your workload so
you can reliably deploy and redeploy to multiple AWS accounts and AWS Regions.

## Disaster Recovery Strategies

![DR Strategies](../images/disaster_recovery_strategies.png)

## RTO


![RTO](../images/rto_tradeoff.png)

## RPO

![RPO](../images/rpo_tradeoff.png)

## Pilot Light

- Data is available in another region always
- Infrastructure in another region is _switched off_.

> Example: Use Amazon Aurora global database across regions. ELB, Front-end and application servers are switched off in another region. 
In the event of disaster, switch them on and configure Route 53 to the new ELB.

> When failing over to run your read/write workload from the disaster recovery Region, you must promote
an RDS read replica to become the primary instance. For DB instances other than Aurora, the process
takes a few minutes to complete and rebooting is part of the process.

> Route 53 *failover routing* is a recommended way to failover to the DR region in an event of a disaster.

> Another option is to use AWS Global Accelerator. Using AnyCast IP, you can associate multiple endpoints
in one or more AWS Regions with the same static public IP address or addresses. AWS Global Accelerator
then routes traffic to the appropriate endpoint associated with that address. Global Accelerator health checks monitor endpoints.
**Global Accelerator also avoids caching issues that can occur with DNS systems (like Route 53)**

> CloudFront offers origin failover, but this is done per each request.

## AWS Backup

> AWS Backup supports copying backups across Regions, such as to a disaster recovery Region.

> **Your backup strategy must include testing your backups.**

## AWS DRS

- Set up AWS Elastic Disaster Recovery on your source servers to initiate secure data replication.
- Your data is replicated to a staging area subnet in your AWS account, in the AWS Region you select. 
- The staging area design reduces costs by using affordable storage and minimal compute resources to maintain ongoing replication. You can perform non-disruptive tests to confirm that implementation is complete.

- On-premises to AWS: Quickly recover operations after unexpected events such as software issues or datacenter hardware failures. AWS DRS enables RPOs of seconds and RTOs of minutes.
- Cloud to AWS: Help increase resilience and meet compliance requirements using AWS as your recovery site. AWS DRS converts your cloud-based applications to run natively on AWS.
- AWS Region to AWS Region: Increase application resilience and help meet availability goals for your AWS-based applications, using AWS DRS to recover applications in a different AWS Region.

## Pilot Light vs Warm Standby

> The distinction is that pilot light cannot process requests without additional action taken
first, whereas warm standby can handle traffic (at reduced capacity levels) immediately. The pilot light
approach requires you to “turn on” servers, possibly deploy additional (non-core) infrastructure, and
scale up, whereas warm standby only requires you to scale up (everything is already deployed and
running). Use your RTO and RPO needs to help you choose between these approaches.

> If you have RTO of <= 5 minutes, Pilot Light would not work.
> If you have RTO of <= 30 minutes for application tier, a Warm standby is required. Pilot light may work as well. AMI snapshots backup and restore **would not work.**

## Pilot Light vs Backup & Restore

> For Backup & Restore, RTO/RPO is in hours. If the RTO is <= 30 minutes, Backup and Restore will not work. 

## Multi-site Active/Active

- A write global strategy routes all writes to a single Region. In case of failure of that Region, another
Region would be promoted to accept writes. **Aurora global database** is a good fit for write global,
as it supports synchronization with read-replicas across Regions, and **you can promote one of the
secondary Regions to take read/write responsibilities in less than one minute**. Aurora also supports
write forwarding, which lets secondary clusters in an Aurora global database forward SQL statements
that perform write operations to the primary cluster.
- A write local strategy routes writes to the closest Region (just like reads). Amazon DynamoDB global
tables enables such a strategy, allowing read and writes from every region your global table is
deployed to. Amazon DynamoDB global tables use a last writer wins reconciliation between concurrent
updates.
- A write partitioned strategy assigns writes to a specific Region based on a partition key (like user ID)
to avoid write conflicts. Amazon S3 replication configured bi-directionally can be used for this case,
and currently supports replication between two Regions.

> AWS CloudFormation is a powerful tool to enforce consistently deployed infrastructure among AWS
accounts in multiple AWS Regions. AWS CloudFormation StackSets extends this functionality by enabling
you to create, update, or delete CloudFormation stacks across multiple accounts and Regions with
a single operation

> Amazon RDS for MySQL instance with a cross-Region read replica in the failover Region can work for DR, **but will fail RTO of less than five minutes**

## Testing Diaster Recovery

> Our experience has shown that the only error recovery that works is the path you test frequently. This is
the reason why having a small number of recovery paths is best.

> You can utilize AWS Config to continuously monitor and record your AWS resource configurations. AWS
Config can detect drift and trigger AWS Systems Manager Automation to fix drift and raise alarms.
