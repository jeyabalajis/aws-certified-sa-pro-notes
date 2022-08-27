# RDS

## Salient Features
-  Engines: PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server
-  Transparent Data Encryption (TDE) for Oracle and SQL Server
-  CloudTrail cannot be used to track queries made within RDS

> Multi-AZ is for improving availability. For improving performance, use read replicas.

> Read replicas are available in Amazon RDS for MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server as well as Amazon Aurora.

> Amazon RDS does not support certain features in Oracle such as Multitenant Database, Real Application Clusters (RAC), Unified Auditing, Database Vault, and many more. If any of these features are needed, then Oracle must be installed in a custom EC2 instance.

## Oracle RDS

- DMS works on Oracle RDS
- RDS for Oracle does NOT support RAC
- If Oracle RMAN is used, restore to RDS not supported (only non-RDS supported)

> Use Provisioned IOPS storage to improve the read performance of your database.

### RDS Storage Types

Amazon RDS provides three storage types:

- General Purpose SSD (also known as gp2), Provisioned IOPS SSD (also known as io1), and magnetic (also known as standard)
- You can create MySQL, MariaDB, Oracle, and PostgreSQL RDS DB instances with up to 64 tebibytes (TiB) of storage. 
- You can create SQL Server RDS DB instances with up to 16 TiB of storage. 
- General Purpose SSD: Cost effective storage for a broad range of workloads. Ability to burst to 3000 IOPS for extended period of time
- Provisioed IOPS SSD: Designed to meet the needs of I/O-intensive workloads, that require low I/O latency and consistent I/O throughput.
- Magnetic: Supports magnetic storage for backward compatibility. We recommend that you use General Purpose SSD or Provisioned IOPS for any new storage needs. 

> You can also use Provisioned IOPS SSD storage with read replicas for MySQL, MariaDB or PostgreSQL.

![image](https://user-images.githubusercontent.com/15995686/180932123-a3b54ddd-2642-421e-bb6b-71a2070a9476.png)

### RDS Multi-AZ

- Supports multiple availability zones **but not multiple regions**.

[RDS Multi-AZ](https://aws.amazon.com/rds/features/multi-az/)

### RDS Proxy

- With RDS Proxy, you no longer need code that handles cleaning up idle connections and managing connection pools
- Multiple Lambda functions can connect to a single RDS Proxy, which handles connection pool

### RDS Encryption

- The only way to **unencrypt an encrypted database** is to export the data and import the data into another DB instance. 
- You cannot create unencrypted snapshots of encrypted DB instance.
- You cannot create unencrypted read replicas of an encrypted DB instance.

## Aurora

- Cross Region RR: entire database is copied (not select tables)
- Cross Region replication supported in Aurora
- Shared storage volume across all instances (master and reader) - auto expandable upto 128 TB
- **Aurora Cross Region Read Replicas: Useful for disaster recovery**
- Aurora global database: Upto 5 secondary regions with less than 1 second replication lag.
        - Promoting another region for disaster recovery has an RTO of less than 1 minute.
- Use Aurora multi-master if immediate failover is required for Write node (HA). **_Multi-master is within a region only (i.e. not cross-region). _**
        - every node does R/W instead of only one master node.
        

> Before you can create an Aurora MySQL DB cluster that is a cross-Region read replica, 
>you must turn on binary logging on your source Aurora MySQL DB cluster. 

> Cross-region replication for Aurora MySQL uses MySQL binary replication to replay changes on the cross-Region read replica DB cluster.

> Amazon Aurora further extends the benefits of read replicas by employing an SSD-backed virtualized storage layer purpose-built for database workloads. Amazon Aurora replicas share the same underlying storage as the source instance, lowering costs and avoiding the need to copy data to the replica nodes.

### Aurora Global Database

- Amazon Aurora Global Database is designed for globally distributed applications, allowing a single Amazon Aurora database to span multiple AWS regions. 
- It replicates your data with no impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from region-wide outages.

### Aurora Replication

> Aurora Replicas also help to increase availability, apart from providing read scaling. If the writer instance in a cluster becomes unavailable, Aurora automatically promotes one of the reader instances to take its place as the new writer.
> To increase availability, you can use Aurora Replicas as failover targets. There is a brief interruption during which read and write requests made to the primary instance fail with an exception.

#### Aurora MySQL Replication

- You can create an Aurora read replica of an Aurora MySQL DB cluster in a different AWS Region, by using MySQL binary log (binlog) replication.
- Each cluster can have up to five read replicas created this way, each in a different Region.

#### Aurora PostgreSQL Replication

- Aurora PostgreSQL doesn't support cross-Region Aurora Replicas. However, you can use Aurora global database to scale your Aurora PostgreSQL DB cluster's read capabilities to more than one AWS Region and to meet availability goals. 

### Aurora vs. RDS

> Amazon RDS MySQL does not have a single reader endpoint for read replicas. You must use Amazon Aurora for MySQL to support this.
        
> Amazon RDS volumes are built using Amazon EBS volumes (multiple types), except for Amazon Aurora, which uses an SSD-backed virtualized storage layer purpose-built for database workloads.

### Database Migration service
- Supports On-premises to aws, aws to aws and aws to on-premises
- Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)

### Troubleshooting

- For Amazon Aurora you can monitor the MySQL error log, slow query log, and the general log.
- The MySQL error log is generated by default.
- You can generate the slow query and general logs by setting parameters in your DB parameter group. This data will help troubleshoot any issues with the Aurora database layer.

> AWS X-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization. You can use the AWS SDK to develop programs that use the X-Ray API.
> X-Ray can be used to collect the parameters sent for query and it then can be used for diagnosis of database performance.

### Best Practices

> Real-time indexing of DynamoDB objects into Elastic Search: Can be accomplished by dynamodb streams - Kinesis Data Streams - Kinesis Data Firehose - Elastic Search/Open Search.
> Aurora maximum capacity is 128 TB. For unlimited storage, choose DynamoDB.
