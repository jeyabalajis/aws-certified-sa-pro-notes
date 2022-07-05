# Elastic BeanStalk

## Deployment options:

- All at once: 
- Rolling: Split the environment's EC2 instances into batches and deploys the new version of the application to one batch at a time.
- Rolling with additional batch: Same as Rolling, but an additional batch is deployed before taking the current batch instances out of service.
- Immutable: Launch a fresh ASG group, alongside instances running the old version. avoids partial failure. 
- Traffic splitting: Same as Immutable, but weight is configurable. It is canary, so a percentage of traffic is sent to new batch and if healthy, moves all traffic to new.

> Elastic Beanstalk abstracts the finer details and automatically handles all the details such as provisioning an ECS cluster, balancing load, auto-scaling, monitoring, and placing the containers across the cluster.

> ECS provides more control over Elastic BeanStalk
