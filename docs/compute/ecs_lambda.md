# ECS & Lambda

## ECS

- awsvpc — The task is allocated its own elastic network interface (ENI) and a primary private IPv4 address. This gives the task the same networking properties as Amazon EC2 instances.
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage.
- You can use these and other CloudWatch metrics to scale out your service (add more tasks) to deal with high demand at peak times, and to scale in your service (run fewer tasks) to reduce costs during periods of low utilization.
- Target Tracking Policies are recommended.
- ECS / Fargate is preferred for running arbitrary Docker images.


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

Can set a “reserved concurrency” at the function level (=limit)
• Each invocation over the concurrency limit will trigger a “Throttle”

There are two types of concurrency controls available:

> Reserved concurrency – Reserved concurrency guarantees the maximum number of concurrent instances for the function. 
When a function has reserved concurrency, no other function can use that concurrency. There is no charge for configuring reserved concurrency for a function.

> Provisioned concurrency – Provisioned concurrency initializes a requested number of execution environments so that they are prepared to respond immediately to your function's invocations.
Note that configuring provisioned concurrency incurs charges to your AWS account.
