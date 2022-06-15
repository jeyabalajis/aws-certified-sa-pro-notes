# RDS

## Salient Features
-  Engines: PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server
-  Transparent Data Encryption (TDE) for Oracle and SQL Server
-  CloudTrail cannot be used to track queries made within RDS

### Oracle RDS
- DMS works on Oracle RD
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
        
        
### Database Migration service
- Supports On-premises to aws, aws to aws and aws to on-premises
- Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)
-        