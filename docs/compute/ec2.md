# EC2

## Placement groups
- Control the EC2 Instance placement strategy using placement groups
- Cluster—clusters instances into a low-latency group in a single Availability Zone. Great network bandwidth
- Spread—spreads instances across underlying hardware (max 7 instances per group per AZ) – critical applications. Can span across multiple AZ.
- Partition—spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
- Moving an instance (stop, use CLI, start)

## Launch Types
- Dedicated Instances: no other customers will share your hardware
- Dedicated Hosts: book an entire physical server, control instance placement
    - Great for software licenses that operate at the core, or CPU socket level
    - Can define host affinity so that instance reboots are kept on the same host
    
## EC2 Metrics
- RAM is not included in EC2 Metrics

## Enhanced networking
-  Elastic Network Adapter (ENA) up to 100 Gbps 

### Elastic Fabric Adapter
- Improved ENA for HPC, only works for Linux
- Great for inter-node communications, tightly coupled workloads
- Leverages Message Passing Interface (MPI) standard 
- Bypasses the underlying Linux OS to provide low-latency, reliable transport

## Auto Scaling – Instance Refresh
- Goal: update launch template and then re-creating all EC2 instances

## Spot fleet strategies
- lowestPrice: from the pool with the lowest price (cost optimization, short workload)
- diversified: distributed across all pools (great for availability, long workloads)
- capacityOptimized: pool with the optimal capacity for the number of instances

