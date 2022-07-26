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

- Amazon DynamoDB auto scaling uses the AWS Application Auto Scaling service to dynamically adjust provisioned throughput capacity on your behalf, in response to actual traffic patterns. This enables a table or a global secondary index to increase its provisioned read and write capacity to handle sudden increases in traffic, without throttling.
- When the workload decreases, Application Auto Scaling decreases the throughput so that you don't pay for unused provisioned capacity.

## Adaptive Capacity

> Adaptive capacity is enabled automatically for every DynamoDB table, at no additional cost. **You don't need to explicitly enable or disable it.**

## DAX

DAX is a caching solution for DynamoDB that can be placed in front of the database. This will provide much improved read performance _without any application changes_.




