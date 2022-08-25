# EFS

## Salient Features
- Like EBS, EFS also offers high durability. However, the main difference lies in scalability. 
- EFS volumes can scale up quickly and automatically to meet abrupt spikes in workload demand and scale down with a decreased load. 
- This makes EFS more flexible than EBS.
- 11 9s of durability and 4 9s of availability
- Locking in Amazon EFS follows the NFS v4.1 protocol for advisory locking and allows your applications to use both whole file and byte range locks.

> If you are looking at providing _higher throughput_ and _lower latency_ across a fleet of multiple EC2 instances, **EFS is an optimal choice**.

## EFS Scale
- 1000s of concurrent NFS clients, 10 GB+ /s throughput
- Grow to Petabyte-scale network file system, automatically
 
 > FSx for Lustre file systems can scale to hundreds of GB/s of throughput and millions of IOPS. FSx for Lustre also supports concurrent access to the same file or directory from thousands of compute instances. You use Lustre for workloads where speed matters, such as machine learning, high-performance computing (HPC), video processing, and financial modeling.
 
 > For some HPC use cases where there is a need for improving performance, FSx for Lustre may perform better than EFS. 
    
## EFS on-premises
- To access Amazon EFS file systems from on premises, you must have an AWS Direct Connect or AWS VPN connection between your on-premises datacenter and your Amazon VPC.
- You mount an Amazon EFS file system on your on-premises Linux server using the standard Linux mount command for mounting a file system using the NFS v4.1 protocol.
- Enables access of Amazon EFS file system concurrently from my on-premises datacenter servers as well as Amazon EC2 instances

Key use-cases:
1. Cloud bursting. move data to EFS, use a fleet of EC2 to process and move back to on-prem
2. DR Copy
3. Backup data to EFS permanently

> AWS Direct Connect provides a high-bandwidth and lower-latency dedicated network connection over which you can mount your EFS file systems. Once mounted, you can use DataSync to copy data into EFS up to 10 times faster than standard Linux copy tools.

## Performance Mode

- set at EFS creation time

### General purpose (default): 

- Latency-sensitive use cases (web server, CMS, etc…) and also general file serving.
- **upto 35,000 IOPs**

### Max I/O:

- Higher latency, Higher throughput, highly parallel (big data, media processing)
- **upto 500,000+ IOPs**, _but trades off latency for higher throughput._

## Throughput mode:

- You can switch an existing file system's throughput mode, with the restriction that you can make only one restricted change in a 24-hour period
- 
### Bursting Throughput: 

- (1 TB = 50MiB/s + burst of up to 100MiB/s) - _Throughput scales as the file system grows._
- Uses the concept of burst credits.
- When throughput is low, Bursting Throughput mode uses _burst buckets to save burst credits_. When throughput is higher, _it uses burst credits_.
- When burst credits are available, a file system can drive up to 100 MBps per terabyte (TB) of storage, with a minimum of 100 MBps. 
- If no burst credits are available, a file system can drive up to 50 MBps per TB of storage with a minimum of 1 MBps.

#### Baseline performance:

- 1 TB and above file system can drive 150 MiBps read only per TiB of storage and 50 MiBps write only per TiB of storage.
- 100 GB~ file system can drive 15 MiBps read only per TiB of storage and 5 MiBps write only per TiB of storage.

#### Burst performance:

- 1 TB and above file system can drive 300 MiBps read only per TiB of storage and 100 MiBps write only per TiB of storage - for 12 hours per day.
- 100 GB~ file system can drive 300 MiBps read only per TiB of storage and 100 MiBps write only per TiB of storage - for 72 minutes per day.

### Provisioned Throughput:

- Set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage is also possible.

> If you need to prioritize _higher throughput without compromising latency_, choose _Provisioned Throughput_ 
