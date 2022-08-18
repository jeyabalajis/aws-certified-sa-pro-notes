# Elastic BeanStalk

## Deployment options:

- **All at once**: Deploys the new versions to all intances simultaneously. There would be outage. If the deployment fails, put previous version.
- **Rolling**: Split the environment's EC2 instances into batches and deploys the new version of the application to one batch at a time. _Not ideal for performance sensitive applications_.
- **Rolling with additional batch**: Same as Rolling, but an additional batch is deployed before taking the current batch instances out of service. _Small additional cost but no performance impact._
- **Immutable**: Launch a fresh ASG group, alongside instances running the old version. Avoids partial failure. _Zero downtime, but cost is higher. Longer deployment time._ Rollback is the fastest.
- **Traffic splitting**: Same as Immutable, but weight is configurable. It is canary, so a percentage of traffic is sent to new batch and if healthy, moves all traffic to new.
- **Blue/Green**: Setup a stage environment and deploy updates there. Use **Swap URLs** in Elastic Beanstalk to conduct a Blue/Green deployment.

> Elastic Beanstalk abstracts the finer details and automatically handles all the details such as provisioning an ECS cluster, balancing load, auto-scaling, monitoring, and placing the containers across the cluster.

> ECS provides more control over Elastic BeanStalk

> **You can't deploy an application to your on-premises servers using Elastic Beanstalk**. This is only applicable to your Amazon EC2 instances. However, CodeDeploy can deploy an application to your on-premises servers.

## Using Docker

> By using Docker with Elastic Beanstalk, you have an infrastructure that handles all the details of capacity provisioning, load balancing, scaling, and application health monitoring. You can easily manage your web application in an environment that supports the range of services that are integrated with Elastic Beanstalk.
