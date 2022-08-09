# Application Auto Scaling

Application Auto Scaling is a web service for developers and system administrators who need a solution for automatically scaling their scalable resources for individual AWS services beyond Amazon EC2.

## Services

- Comprehend
- DynamoDB
- ECS: supports both Target Tracking (ECSServiceAverageCPUUtilization) and Step scaling - target tracking is preferred over step scaling. 
- ElastiCache
- EMR
- Lambda
- SageMaker
- Spot Fleet
- Aurora

> **NOTE:** RDS is not part of the above list. Autoscaling is not applicable for RDS databases.

## Types of scaling

### Target Tracking

Target Tracking is preferred for dynamic scaling scenarios.

### Scheduled Scaling

For example, let's say that every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday. You can configure a schedule for Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday.

> You can temporarily turn off scheduled scaling for a scalable target. 

#### Limitations

- The names of scheduled actions must be unique per scalable target.
- Application Auto Scaling doesn't provide second-level precision in schedule expressions. The finest resolution using a cron expression is 1 minute.
- The scalable target can't be an Amazon MSK cluster. Scheduled scaling is not supported for Amazon MSK.

### Step Scaling: 

> You cannot create step scaling policies for certain services. Step scaling policies are not supported for DynamoDB, Amazon Comprehend, Lambda, Amazon Keyspaces, Amazon MSK, ElastiCache, or Neptune.

## Use Cases
### Handling Heavy Workloads

- Use scheduled scaling to add extra capacity to meet a heavy load before it arrives, and then remove the extra capacity when it's no longer required.
- Use a target tracking scaling policy to scale your application based on current resource utilization.
