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

## Types of scaling

- Target Tracking
- Scheduled Scaling
- Step Scaling Policies: Target Tracking is preferred for dynamic scaling scenarios.

## Use Cases
### Handling Heavy Workloads

- Use scheduled scaling to add extra capacity to meet a heavy load before it arrives, and then remove the extra capacity when it's no longer required.
- Use a target tracking scaling policy to scale your application based on current resource utilization.
