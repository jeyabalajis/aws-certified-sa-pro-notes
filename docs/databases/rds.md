# RDS

## Salient Features
-  Engines: PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server
-  Transparent Data Encryption (TDE) for Oracle and SQL Server
-  CloudTrail cannot be used to track queries made within RDS

> Multi-AZ is for improving availability. For improving performance, use read replicas.

> Read replicas are available in Amazon RDS for MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server as well as Amazon Aurora.

### Oracle RDS
- DMS works on Oracle RDS
- RDS for Oracle does NOT support RAC
- If Oracle RMAN is used, restore to RDS not supported (only non-RDS supported)

### RDS Proxy
- With RDS Proxy, you no longer need code that handles cleaning up idle connections and managing connection pools
- Multiple Lambda functions can connect to a single RDS Proxy, which handles connection pool

### Aurora
- Cross Region RR: entire database is copied (not select tables)
- Cross Region replication supported in Aurora
- Shared storage volume across all instances (master and reader) - auto expandable upto 128 TB
- Aurora Cross Region Read Replicas: Useful for disaster recovery
- Aurora global database: Upto 5 secondary regions with less than 1 second replication lag.
        - Promoting another region for disaster recovery has an RTO of less than 1 minute.
- Use Aurora multi-master if immediate failover is required for Write node (HA)
        - every node does R/W instead of only one master node.
        

> Before you can create an Aurora MySQL DB cluster that is a cross-Region read replica, 
>you must turn on binary logging on your source Aurora MySQL DB cluster. 

> Cross-region replication for Aurora MySQL uses MySQL binary replication to replay changes on the cross-Region read replica DB cluster.

> Amazon Aurora further extends the benefits of read replicas by employing an SSD-backed virtualized storage layer purpose-built for database workloads. Amazon Aurora replicas share the same underlying storage as the source instance, lowering costs and avoiding the need to copy data to the replica nodes.

#### Aurora vs. RDS

> Amazon RDS MySQL does not have a single reader endpoint for read replicas. You must use Amazon Aurora for MySQL to support this.
        
> Amazon RDS volumes are built using Amazon EBS volumes, except for Amazon Aurora, which uses an SSD-backed virtualized storage layer purpose-built for database workloads.

### Database Migration service
- Supports On-premises to aws, aws to aws and aws to on-premises
- Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)

### Best Practices
> Real-time indexing of DynamoDB objects into Elastic Search: Can be accomplished by dynamodb streams - Kinesis Data Streams - Kinesis Data Firehose - Elastic Search/Open Search.
> Aurora maximum capacity is 128 TB. For unlimited storage, choose DynamoDB.
