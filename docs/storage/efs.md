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

### Max I/O:

- Higher latency, Higher throughput, highly parallel (big data, media processing)
- Trade-off Latency for higher throughput.

### Throughput mode:

- Bursting Throughput (1 TB = 50MiB/s + burst of up to 100MiB/s) - _Throughput scales as the file system grows._
- Provisioned Throughput: Set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage is also possible.

> If you need to prioritize _higher throughput without compromising latency_, choose _Provisioned Throughput_ 
