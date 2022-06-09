# EFS

## Salient Features
- Like EBS, EFS also offers high durability. However, the main difference lies in scalability. 
- EFS volumes can scale up quickly and automatically to meet abrupt spikes in workload demand and scale down with a decreased load. 
- This makes EFS more flexible than EBS.
- 11 9s of durability and 4 9s of availability
- Locking in Amazon EFS follows the NFS v4.1 protocol for advisory locking and allows your applications to use both whole file and byte range locks.

### EFS Scale
- 1000s of concurrent NFS clients, 10 GB+ /s throughput
- Grow to Petabyte-scale network file system, automatically
- Performance mode (set at EFS creation time)
    - General purpose (default): latency-sensitive use cases (web server, CMS, etc…)
    - Max I/O – higher latency, throughput, highly parallel (big data, media processing)
- Throughput mode
    - Bursting (1 TB = 50MiB/s + burst of up to 100MiB/s)
    - Provisioned: set your throughput regardless of storage size, ex: 1 GiB/s for 1 TB storage
    
    
### EFS on-premises
- To access Amazon EFS file systems from on premises, you must have an AWS Direct Connect or AWS VPN connection between your on-premises datacenter and your Amazon VPC.
- You mount an Amazon EFS file system on your on-premises Linux server using the standard Linux mount command for mounting a file system using the NFS v4.1 protocol.
- Enables access of Amazon EFS file system concurrently from my on-premises datacenter servers as well as Amazon EC2 instances

Key use-cases:
1. Cloud bursting. move data to EFS, use a fleet of EC2 to process and move back to on-prem
2. DR Copy
3. Backup data to EFS permanently

> AWS Direct Connect provides a high-bandwidth and lower-latency dedicated network connection over which you can mount your EFS file systems. Once mounted, you can use DataSync to copy data into EFS up to 10 times faster than standard Linux copy tools.


