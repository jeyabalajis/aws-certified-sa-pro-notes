# ECS & Lambda

## ECS

- awsvpc — The task is allocated its own elastic network interface (ENI) and a primary private IPv4 address. This gives the task the same networking properties as Amazon EC2 instances.
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage.
- You can use these and other CloudWatch metrics to scale out your service (add more tasks) to deal with high demand at peak times, and to scale in your service (run fewer tasks) to reduce costs during periods of low utilization.
- Target Tracking Policies are recommended.
- ECS / Fargate is preferred for running arbitrary Docker images.

### Spot Instance Draining

- If Amazon ECS Spot Instance draining is enabled on the instance, ECS receives the Spot Instance interruption notice and places the instance in DRAINING status.
- New tasks are not scheduled on these instances
- Spot Instance draining is disabled by default and must be manually enabled (through user data that is executed during instance launch). 

### Deployment Types

#### Rolling update: 
Replace current tasks with new tasks. Respect MinimumHealthyPercent

#### Blue/Green deployment with CodeDeploy: 
Can be Canary, Linear or All-at-Once

#### External deployment: 
The external deployment type enables you to use any third-party deployment controller for full control over the deployment process for an Amazon ECS service. 

## Lambda

- By default, lambdas have internet access allowing them to communicate with any public resources. 
- This works well for many use cases but there are times 
- When you will need your lambda to access resources inside your VPC. An example of this is would be a requirement to access a VPN tunnel inside a VPC.
- For VPC-enabled Lambda functions, you should make sure that your subnet has enough elastic network interfaces (ENIs) and IP addresses to meet the demand.
- By default, there is a soft limit of 350 ENIs per region. If this limit is reached then invocations of VPC-enabled functions will be throttled. 
- Which is why you should always ask for a limit raise for ENIs whenever you ask for a concurrency raise for Lambda. 
- it’s recommended that you create dedicated subnets with large IP ranges for your Lambda functions.
- In Lambda, more RAM means better vCPU. **There is no specific config for CPU**

> “Don’t put your Lambda function in a VPC unless you have to”!

- For short-lived operations, such as DynamoDB queries, the latency overhead of setting up a TCP connection might be greater than the operation itself. 
- To ensure connection reuse for short-lived/infrequently invoked functions, 
- we recommend that you use TCP keep-alive for connections that were created during your function initialization, to avoid creating new connections for subsequent invokes.

### Too many Requests Exception

Your functions' concurrency is the number of instances that serve requests at a given time. For an initial burst of traffic, your functions' cumulative concurrency in a Region can reach an initial level of between 500 and 3000, which varies per Region.

**When seeing the TooManyRequestsException in AWS Lambda, it is possible that the throttles that you're seeing aren't on your Lambda function.**

An ideal solution in this case may be to **group data sets and reduce the number of calls to the Lambda function**, if the Lambda function is processing data.
If the Lambda is being called through SQS, ensure that the **SQS employs batching** to reduce the number of calls to the Lambda function.

[Lambda Throttling](https://aws.amazon.com/premiumsupport/knowledge-center/lambda-troubleshoot-throttling/) 


> Note: To work around burst concurrency limits, you can configure provisioned concurrency.
> Note that the burst concurrency quota is not per-function; it applies to all your functions in the Region.

### Concurrency

> Reserved concurrency – Reserved concurrency guarantees the maximum number of concurrent instances for the function. When a function has reserved concurrency, no other function can use that concurrency. There is no charge for configuring reserved concurrency for a function.

> Reserved concurrency also limits your function from using concurrency from the unreserved pool, which caps its maximum concurrency. You can reserve concurrency to prevent your function from using all the available concurrency in the Region, or from overloading downstream resources.

> Provisioned concurrency – Provisioned concurrency initializes a requested number of execution environments so that they are prepared to respond immediately to your function's invocations. Note that configuring provisioned concurrency incurs charges to your AWS account.

Can set a “reserved concurrency” at the function level (=limit)
• Each invocation over the concurrency limit will trigger a “Throttle”


### Asynchronous processing
> Lambda – Asynchronous Invocation: S3, SNS, CloudWatch Events
> Configure a dead letter queue (DLQ) on AWS Lambda to give you more control over message handling for all asynchronous invocations, including those delivered via AWS events (S3, SNS, IoT, etc).
> To invoke a function asynchronously, set the invocation type parameter to Event.
> Lambda allows configuration of destinations for asynchronous invocation.

### Lambda Event Source Mapping:

An event source mapping is a Lambda resource that reads from an event source and invokes a Lambda function. 
You can use event source mappings to process items from a stream or queue in services that don't invoke Lambda functions directly. 

Sources:
- Kinesis Data Streams, SQS, SQS FIFO
- DynamoDB Streams, Amazon MQ, Apache Kafka

> To fine-tune batching behavior, you can configure a batching window (MaximumBatchingWindowInSeconds) and a batch size (BatchSize). 
A batching window is the maximum amount of time to gather records into a single payload. A batch size is the maximum number of records in a single batch.

### AWS Lambda versions:
- Version = code + configuration (nothing can be changed - immutable)
- Each version of the lambda function can be accessed 

### AWS Lambda Aliases
- Aliases are ”pointers” to Lambda function versions

You can’t incrementally deploy your software across a fleet of servers when there are no servers!* In fact, even the term “deployment” takes on a different meaning with functions as a service (FaaS).

> With the introduction of alias traffic shifting, it is now possible to trivially implement canary deployments of Lambda functions. By updating additional version weights on an alias, invocation traffic is routed to the new function versions based on the weight specified. 

### Using AWS CLI for Canary

```
# Update $LATEST version of function
aws lambda update-function-code --function-name myfunction ….

# Publish new version of function
aws lambda publish-version --function-name myfunction

# Point alias to new version, weighted at 5% (original version at 95% of traffic)
aws lambda update-alias --function-name myfunction --name myalias --routing-config '{"AdditionalVersionWeights" : {"2" : 0.05} }'

# Verify that the new version is healthy
…
# Set the primary version on the alias to the new version and reset the additional versions (100% weighted)
aws lambda update-alias --function-name myfunction --name myalias --function-version 2 --routing-config '{}'
```

The above can be automated with AWS Step Functions.
