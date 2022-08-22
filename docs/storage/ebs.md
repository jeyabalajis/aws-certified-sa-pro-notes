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
- Throughput Optimized HDD (st1) - _Frequently accessed Throughput Intensive Workloads_
- Cold HDD (sc1) - _ lowest cost HDD volume and is designed for in-frequently accessed workloads_

- EBS-optimized instances deliver dedicated throughput between Amazon EC2 and Amazon EBS, with options between 62.5 MB/s and 7,500 MB/s depending on the instance type used. 
- To achieve the limit of 64,000 IOPS and 1,000 MB/s throughput, the volume must be attached to a Nitro System-based EC2 instance.

> Provisioned IOPS volumes have a base I/O size of 16KB. So, if you have provisioned a volume with 40,000 IOPS for an I/O size of 16KB, it will achieve up to 40,000 IOPS at that size. If the I/O size is increased to 32 KB, then you will achieve up to 20,000 IOPS, and so on.

#### General Purpose SSD

> General Purpose SSD storage performance is governed by volume size, which dictates the base performance level of the volume and how quickly it accumulates I/O credits. **Larger volumes have higher base performance levels and accumulate I/O credits faster.**

> gp instances support burst mode. To understand burst mode, you must be aware that **every gp2 volume regardless of size starts with 5.4 million I/O credits at 3000 IOPS**. This means that even for very small volumes, you start with a high-performing volume. This is ideal for “bursty” workloads, such as **daily reporting and recurring extract, transform, and load (ETL) jobs.** It is also good for workloads that don’t require high-sustained IOPS.

> _The burst credit is always being replenished at the rate of 3 IOPS per GiB per second. Consider a daily ETL workload that uses a lot of I/O. For the daily job, gp2 can burst, and during downtime, burst credit can be replenished for the next day’s run._

> An important thing to note is that for any gp2 volume larger than 1 TiB, the baseline performance is greater than the burst performance. For such volumes, burst is irrelevant because the baseline performance is better than the 3,000 IOPS burst performance.

#### Provisioned IOPs (io1, io2)

> Provisioned IOPS SSD (io1 and io2) volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads, that are sensitive to storage performance and consistency. Provisioned IOPS SSD volumes use a consistent IOPS rate, which you specify when you create the volume, and Amazon EBS delivers the provisioned performance 99.9 percent of the time. These volumes are recommended for high-performance database instances.

> With io1, it’s quite simple to predict IOPS because this is the value you provide when the volume is created. The Amazon EBS documentation states that io1 volumes deliver within 10 percent of the Provisioned IOPS performance 99.9 percent of the time over a given year.

> Provisioned IOPS SSD volumes can range in size from 4 GiB to 16 TiB and you can provision from 100 IOPS up to 64,000 IOPS per volume. You can achieve up to 64,000 IOPS only on Instances built on the Nitro System. 

#### io2 Block express
- EBS Block Express is the next generation of Amazon EBS storage server architecture purpose-built to deliver the highest levels of performance with sub-millisecond latency for block storage at cloud scale. 

- Block Express does this by using Scalable Reliable Datagrams (SRD), a high-performance lower-latency network protocol, to communicate with Nitro System-based EC2 instances.

- io2 Block Express volumes are suited for workloads that benefit from a single volume that provides sub-millisecond latency, and supports higher IOPS, higher throughput, and **larger capacity than io2 volumes**.

- These workloads include relational and NoSQL databases such as SAP HANA, Oracle, MS SQL, PostgreSQL, MySQL, MongoDB, Cassandra, and critical business operation workloads such as SAP Business Suite, NetWeaver, Oracle eBusiness, PeopleSoft, Siebel, and ERP workloads such as Infor LN and Infor M3.

### EBS Vs EFS vs S3
- The main difference between EBS and EFS is that EBS is only accessible from a single EC2 instance in your particular AWS region, while EFS allows you to mount the file system across multiple regions and instances. 
- Finally, Amazon S3 is an object store good at storing vast numbers of backups or user files.
- EFS is the most expensive (3x of gp2 pricing) and available across multiple instances.
- S3 - typical for huge, long term storage. Glacier for cold storage.

### Copying and Sharing

- Volume to Snapshot: Encryption state retained, same region
- Snapshot to encrypted snapshot: Can be encrypted and can change regions
- Unencrypted snapshot to encrypted volume: can be encrypted and can change AZ
- Unencrypted snapshot to AMI: cannot be encrypted, can be shared with other accounts, can be shared publicly
- Snapshot to snapshot: can change regions
- Encrypted snapshot to encrypted volume: can be encrypted, can change AZ

### Best Practices
> To get additional IOPS performance on a gp2 EBS Volume, simply increase the storage size.
> POSIX and on-premise is supported by EFS; this works on-premise when you connect through Direct Connect or Site-to-Site VPN. 
> Storage Gateway Volume connects with S3, which is an object store.
> S3 Event vs CloudWatch event. S3 Event is the fastest.
> SSD - IOPs is the metric. HDD - Throughput is the metric. io2 block express - sub-millisecond latency.
