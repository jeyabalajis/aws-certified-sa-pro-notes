# DynamoDB

## Global Tables

1. Global tables build on the global Amazon DynamoDB footprint to provide you with a fully managed, multi-Region, and multi-active database that delivers fast, local, read and write performance for massively scaled, global applications. 
2. Global tables replicate your DynamoDB tables automatically across your choice of AWS Regions.
3. Global tables eliminate the difficult work of replicating data between Regions and resolving update conflicts, enabling you to focus on your application's business logic.

### Benefits
1. Read and write locally, access your data globally
2. Performant
3. Easy to set up and operate
4. Availability, durability, and multi-Region fault tolerance
5. Consistency and conflict resolution (Last-Write-Wins)

## Auto-Scaling

- Amazon DynamoDB auto scaling uses the _AWS Application Auto Scaling service_ to dynamically adjust provisioned throughput capacity on your behalf, in response to actual traffic patterns. This enables a table or a global secondary index to increase its provisioned read and write capacity to handle sudden increases in traffic, without throttling.
- When the workload decreases, Application Auto Scaling decreases the throughput so that you don't pay for unused provisioned capacity.

> You can scale DynamoDB tables and global secondary indexes using **target tracking scaling policies** and **scheduled scaling**.

## Adaptive Capacity

> Adaptive capacity is enabled automatically for every DynamoDB table, at no additional cost. **You don't need to explicitly enable or disable it.**

## DAX

DAX is a caching solution for DynamoDB that can be placed in front of the database. This will provide much improved read performance _without any application changes_.

> For read-heavy or bursty workloads, DAX provides increased throughput and potential operational cost savings by reducing the need to overprovision read capacity units. This is especially beneficial for _applications that require repeated reads for individual keys_.

### Use Cases
- Applications that require the fastest possible response time for reads. Some examples include real-time bidding, social gaming, and trading applications. DAX delivers fast, in-memory read performance for these use cases.
- Applications that read a small number of items more frequently than others. For example, consider an ecommerce system that has a one-day sale on a popular product. During the sale, demand for that product (and its data in DynamoDB) would sharply increase, compared to all of the other products. To mitigate the impacts of a "hot" key and a non-uniform traffic distribution, you could offload the read activity to a DAX cache until the one-day sale is over.
- Applications that are read-intensive, but are also cost-sensitive.
- Applications that require repeated reads against a large set of data. Such an application could potentially divert database resources from other applications. For example, a long-running analysis of regional weather data could temporarily consume all the read capacity in a DynamoDB table. 

DAX is not ideal for the following types of applications:

- Applications that require strongly consistent reads (or that cannot tolerate eventually consistent reads).
- Applications that do not require microsecond response times for reads, or that do not need to offload repeated read activity from underlying tables.
- Applications that are write-intensive, or that do not perform much read activity.


