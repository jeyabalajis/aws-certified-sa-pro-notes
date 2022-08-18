# ECS & Lambda

## ECS

- awsvpc — The task is allocated its own elastic network interface (ENI) and a primary private IPv4 address. This gives the task the same networking properties as Amazon EC2 instances.
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Amazon ECS publishes CloudWatch metrics with your service’s average CPU and memory usage.
- You can use these and other CloudWatch metrics to scale out your service (add more tasks) to deal with high demand at peak times, and to scale in your service (run fewer tasks) to reduce costs during periods of low utilization.
- Target Tracking Policies are recommended.
- ECS / Fargate is preferred for running arbitrary Docker images.
- Fargate Launch mode _DOES NOT_ support EBS & EFS integration, but EC2 launch mode does.
- Fargate automates underlying infrastructure, so limited control vis-a-vis EC2 launch type, which offers more granular control, but with more responsibility.

### ECS and IAM Roles

- With EC2 launch type, there is IAM Instance Role and IAM Task Role. Container inherits permissions provided through IAM Instance Role.
- With Fargate launch type, there is only IAM Task Role. 

### Spot Instance Draining

- If Amazon ECS Spot Instance draining is enabled on the instance, ECS receives the Spot Instance interruption notice and places the instance in DRAINING status.
- **New tasks are not scheduled on these instances**
- Spot Instance draining is disabled by default and must be manually enabled (through user data that is executed during instance launch). 

### Scaling

- Service auto-scaling and Cluster auto-scaling
- Service auto-scaling: Specify _Desired Task Count_. This is automatically adjusted based on _Application Auto-Scaling_
- Service auto-scaling supports step, scheduled, and target tracking
- Cluster auto-scaling is applicable only for EC2 Launch type and uses EC2 Auto scaling. Cluster auto-scaling uses Capacity Provider.
- Cluster auto-scaling supports _Managed Instance Termination Protection_ when scale-in happens.  Amazon ECS prevents the Amazon EC2 instances in an Auto Scaling group that contain tasks from being terminated during a scale-in action.

### ECS and ELB

> The Classic Load Balancer doesn't allow you to run multiple copies of a task on the same instance. Instead, when using the Classic Load Balancer, you must statically map port numbers on a container instance. **However, an Application Load Balancer uses dynamic port mapping, so you can run multiple tasks from a single service on the same container instance**.

### Deployment Types

#### Rolling update: 
Replace current tasks with new tasks. Respect MinimumHealthyPercent

#### Blue/Green deployment with CodeDeploy: 
Can be Canary, Linear or All-at-Once

- Canary: Traffic is shifted in two increments. It can be 10 percent in 5 minutes (or) 10 Percent in 15 minutes (the remaining traffic is shifted after the interval)
- Linear: Traffic is shifted in equal increments. It can be 10 percent every 1 minute (or) 10 percent every 3 minutes.
- All-at-once: Moved all at once to new tasks

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

#### Handling
- To work around burst concurrency limits, you can configure provisioned concurrency.
- Note that the burst concurrency quota is not per-function; it applies to all your functions in the Region.
- Use exponential backoff in your application code

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
- A version includes: the function code & dependencies, the Lambda runtime, function settings and a Unique ARN
- $LATEST is mutable.
- Publish a new version (each version is immutable). Each version has it's own ARN
- You can use each version to be used in a separate environment.  

### AWS Lambda Aliases

> Aliases are ”pointers” to Lambda function versions. An alias points to one or more versions.

- You can’t incrementally deploy your software across a fleet of servers when there are no servers!
- In fact, even the term “deployment” takes on a different meaning with functions as a service (FaaS).

> **With the introduction of alias traffic shifting, it is now possible to trivially implement canary deployments of Lambda functions. By updating additional version weights on an alias, invocation traffic is routed to the new function versions based on the weight specified. **
> For example, an alias can send 80% of traffic to Version 1 and 20% of traffic to Version 2.

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

**The above can be automated with AWS Step Functions.**
