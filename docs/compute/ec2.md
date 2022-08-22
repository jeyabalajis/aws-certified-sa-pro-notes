# EC2

## Placement groups
- Control the EC2 Instance placement strategy using placement groups
- Cluster: Clusters instances into a low-latency group in a single Availability Zone. Great network bandwidth. Typical for HPC applications
- Spread: Spreads instances across underlying hardware (max 7 instances per group per AZ) – critical applications. Can span across multiple AZ. Reduces correlated failures. Each instance is located on a separate rack.
- Partition: spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka). Each partition is located on a separate rack. Partitions can be in multiple AZ. Upto 7 per AZ.
- Moving an instance (stop, use CLI, start)

## Launch Types
- Dedicated Instances: no other customers will share your hardware. Per Instance billing. If the use case requires dedicated hardware.
- Dedicated Hosts: book an entire physical server, control instance placement
    - Great for software licenses that operate at the core, or CPU socket level
    - Can define host affinity so that instance reboots are kept on the same host.
    - Per host billing is possible
    - Use case: Database with per-socket licensing

> After you have allocated a Dedicated Host, you can launch instances onto it.
> Dedicated Hosts are also integrated with AWS License Manager. 
> With License Manager, you can create a host resource group, which is a collection of Dedicated Hosts that are managed as a single entity. When creating a host resource group, you specify the host management preferences, such as auto-allocate and auto-release, for the Dedicated Hosts.

## EC2 Metrics
- RAM is not included in EC2 Metrics

## EC2 Instance Role

- Temporary security credentials are retrieved from instance metadat and not user data.

## Enhanced networking

### EC2 Enhanced Networking (SR-IOV)
- Higher bandwidth, higher Packets Per Second (PPS), lower latency
- Option I: Elastic Network Adapter (ENA) up to 100 Gbps
- Option II: Intel 82599 VF up to 10 Gbps
 
### Elastic Fabric Adapter
- Improved ENA for HPC, only works for Linux
- Great for inter-node communications, tightly coupled workloads (distributed computing)
- Leverages Message Passing Interface (MPI) standard
- Bypasses the underlying Linux OS to provide low-latency, reliable transport

## Auto Scaling – Instance Refresh
- Goal: update launch template and then re-create all EC2 instances

## Spot fleet strategies
- lowestPrice: from the pool with the lowest price (cost optimization, short workload)
- diversified: distributed across all pools (great for availability, long workloads)
- capacityOptimized: pool with the optimal capacity for the number of instances

## Best Practices
> Use CloudWatch Alarms for restarting EC2 instances on failed state. StatusCheckFailed_System
> aws vpc networking mode gives each of your ECS Task a different ENI.
> Use case: Multiple ECS Services in a single instance: _Keep minimum permissions as EC2 instance role_. _Provide specific IAM Role per each ECS Task in Task Definition_.

> If you receive a capacity error when launching an instance in a placement group that already has running instances, stop and start all of the instances in the placement group, and try the launch again. 
>Starting the instances may migrate them to hardware that has capacity for all of the requested instances.

## Spot Instances

- 2 minute warning is provided if AWS need to reclaim capacity –available via instance metadata and CloudWatch Events
- For uninterrupted block of duration in a spot instance (1 to 6 hours), can request **Spot Block**. Pricing is 30-45% lesser than on-demand. 

### Capacity Rebalancing

The goal of Capacity Rebalancing is to keep processing your workload without interruption. When Spot Instances are at an elevated risk of interruption, the Amazon EC2 Spot service notifies Amazon EC2 Auto Scaling with an EC2 instance rebalance recommendation.

> Capacity Rebalancing helps you maintain workload availability by proactively augmenting your fleet with a new Spot Instance before a running instance is interrupted by Amazon EC2. You can switch on _Capacity Rebalancing_ in your Auto-Scaling-Group (ASG) configuration.

## Elastic IPs

- Elastic IPs (EIPS) can be remapped across AZs
- Can be moved between instances and Elastic Network Adapters
