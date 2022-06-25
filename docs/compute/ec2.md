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
 
> After you have allocated a Dedicated Host, you can launch instances onto it.
> Dedicated Hosts are also integrated with AWS License Manager. 
> With License Manager, you can create a host resource group, which is a collection of Dedicated Hosts that are managed as a single entity. When creating a host resource group, you specify the host management preferences, such as auto-allocate and auto-release, for the Dedicated Hosts.

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

## Best Practices
> Use CloudWatch Alarms for restarting EC2 instances on failed state. StatusCheckFailed_System
> aws vpc networking mode gives each of your ECS Task a different ENI.
> Use case: Multiple ECS Services in a single instance: Keep minimum permissions as EC2 instance role. Provide specific IAM Role per each ECS Task in Task Definition.
