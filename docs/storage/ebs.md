# Elastic Block Storage
- encryption across snapshots
- general purpose (bursting) vs io1, io2 (multi-attach)
- HDD (sc1 - cheapest, st1  - throughput) - NO ROOT VOLUME
- Instance store is NOT a networking device, physically attached, Ephemeral
- IOPS, you can change volume type when it is running
- performance of scratch EBS is better than snapshot (downloaded from S3)
- delete on terminate for root, configurable.
- RAID0 - performance RAID1 - reliable
- Another AZ requires snapshot
- Volumes need not be unmounted before snapshot but recommended.

### Why EBS?
- Block store, cheaper than EFS and can be attached to EC2 instances. 
- Faster throughput
- Local instance store persists data only as long as that instance is alive

### EBS Block types
- Provisioned IOPS SSD (io2 Block Express, io2, and io1)
- General Purpose SSD (gp3 and gp2). gp3 - no bursting, gp2 - bursting
- Throughput Optimized HDD (st1)
- Cold HDD (sc1).

- EBS-optimized instances deliver dedicated throughput between Amazon EC2 and Amazon EBS, with options between 62.5 MB/s and 7,500 MB/s depending on the instance type used. 
- To achieve the limit of 64,000 IOPS and 1,000 MB/s throughput, the volume must be attached to a Nitro System-based EC2 instance.

> Provisioned IOPS volumes have a base I/O size of 16KB. So, if you have provisioned a volume with 40,000 IOPS for an I/O size of 16KB, it will achieve up to 40,000 IOPS at that size. If the I/O size is increased to 32 KB, then you will achieve up to 20,000 IOPS, and so on.


### io2 Block express
- EBS Block Express is the next generation of Amazon EBS storage server architecture purpose-built to deliver the highest levels of performance with sub-millisecond latency for block storage at cloud scale. 
- Block Express does this by using Scalable Reliable Datagrams (SRD), a high-performance lower-latency network protocol, to communicate with Nitro System-based EC2 instances.
- io2 Block Express is suited for performance and capacity intensive workloads that benefit from lower latency, higher IOPS, higher throughput, or larger capacity in a single volume. 
- These workloads include relational and NoSQL databases such as SAP HANA, Oracle, MS SQL, PostgreSQL, MySQL, MongoDB, Cassandra, and critical business operation workloads such as SAP Business Suite, NetWeaver, Oracle eBusiness, PeopleSoft, Siebel, and ERP workloads such as Infor LN and Infor M3.

### EBS Vs EFS vs S3
- The main difference between EBS and EFS is that EBS is only accessible from a single EC2 instance in your particular AWS region, while EFS allows you to mount the file system across multiple regions and instances. 
- Finally, Amazon S3 is an object store good at storing vast numbers of backups or user files.
- EFS is the most expensive (3x of gp2 pricing) and available across multiple instances.
- S3 - typical for huge, long term storage. Glacier for cold storage.

### Best Practices
> To get additional IOPS performance on a gp2 EBS Volume, simply increase the storage size.
> POSIX and on-premise is supported by EFS; this works on-premise when you connect through Direct Connect or Site-to-Site VPN. 
> Storage Gateway Volume connects with S3, which is an object store.
> S3 Event vs CloudWatch event. S3 Event is the fastest.
> SSD - IOPs is the metric. HDD - Throughput is the metric. io2 block express - sub-millisecond latency.
