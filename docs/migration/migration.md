# Migration

## 6R Migration Strategies

- Rehosting: Lift and Shift (app, db etc.). Migrate as-is. Cloud can save 30% cost
- Replatform: Migrate on-prem database to RDS (or) Java app to Elastic Beanstalk
- Repurchase: Drop and shop. Move to a different product in SaaS. HR to Workday
- Refactoring/Rearchitecting: Reimagine using Cloud-Native. Scale, Performance etc.
- Retire: Turn off things you don't need. Save cost.
- Retain: Too complicated, Cost reason.

## AWS Storage Gateway

- Use cases: Disaster recovery, backup & restore, tiered storage
- Types: File, Volume, Tape and Amazon FSx File Gateway

### File Gateway

- Virtual Machine to bridge between NFS and S3
- S3 buckets are accessible using the NFS and SMB protocol
- Most recent data is cached in file gateway
- EC2 can use NFS/SMB and File Gateway appliance to access S3 data (coming from on-prem)
- Enable S3 Object Versioning to maintain multiple versions
- Lifecycle policies, Object Lock can also be applied (thanks to S3)

### Volume Gateway
- Block storage using iSCSI protocol backed by S3
- Cached volumes: low latency access to most recent data, full data on S3
- Stored volumes: entire dataset is on premise, scheduled backups to S3
- Can create EBS snapshots from the volumes and restore as EBS!
- Up to 32 volumes per gateway
- Each volume up to 32TB in cached mode (1PB per Gateway)
- Each volume up to 16 TB in stored mode (512TB per Gateway)

### Tape Gateway
- Some companies have backup processes using physical tapes (!)
- With Tape Gateway, companies use the same processes but in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors 
- You can’t access single file within tapes. You need to restore the tape entirely

### Amazon FSx Gateway
- Native access to Amazon FSx for Windows File Server
- Local cache for frequently accessed data
- Windows native compatibility (SMB, NTFS, Active Directory...) 
- Useful for group file shares and home directories

## Snow family
- Snow Mobile (Exabytes) > Snowball Edge (80 TB) > Snowcone (8 TB)
- Snowcone can be used in space constrained environments (like drone)
- Each snowmobile has 100 PB of data
- Snowcone: 8 TB Snowball edge storage optimized: 80 TB Snowmobile: < 100 PB
- Use AWS OpsHub to manage Snow family device

### Recommendations:
- Perform multiple write operations at one time - from multiple terminals
- Transfer small files in batches – zip up small files until at least 1MB
- Don't perform other operations on files during transfer
- Reduce local network use
- Eliminate unnecessary hops – directly connect to the compute

> The data transfer rate using the file interface is typically between 25 
MB/s and 40 MB/s. If you need to transfer data faster than this, use the 
Amazon S3 Adapter for Snowball, which has a data transfer rate 
typically between 250 MB/s and 400 MB/s

## Database Migration Service
- You must create an EC2 instance to perform the replication tasks
- Works over VPC Peering, VPN (site to site, software), Direct Connect
- Supports Full Load, Full Load + CDC, or CDC only

> One useful pattern: Edge (snow device) to S3 to DMS to Target database.

## Disaster Recovery (DR)
- RPO: Recovery Point Objective - deals with data loss
- RTO: Recovery Time Objective - deals with time to recover

### Strategies
- Backup and Restore (RTO is not a concern. High RPO)
- Pilot Light (Similar to Backup and Restore, but faster)
- Warm Standby (full system as a standby but reduced size, on failover scale up)
- Hot Site / Multi Site Approach (Fully minimized RTO, typically seconds, expensive)

### DR Tips

#### Backup 
- EBS Snapshots, RDS automated backups / Snapshots, etc…
- Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication
- From on-premises: Snowball or Storage Gateway

#### High Availability
- Use Route53 to migrate DNS over from Region to Region
- RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
- Site to Site VPN as a recovery from Direct Connect

#### Replication 
- RDS Replication (Cross Region), AWS Aurora + Global Database
- Database replication from on-premises to RDS
- Storage Gateway

#### Automation
- CloudFormation / Elastic Beanstalk to re-create a whole new environment
- Recover / Reboot EC2 instances with CloudWatch if alarms fail
- AWS Lambda functions for customized automations

#### Chaos engineering
- AWS Fault Injection Simulator (FIS)
- A fully managed service for running fault injection experiments on AWS workloads
- Supports the following AWS services: EC2, ECS, EKS, RDS


