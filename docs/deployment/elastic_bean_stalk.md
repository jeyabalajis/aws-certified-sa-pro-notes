# Elastic BeanStalk

- Elastic Beanstalk also supports deployment versioning. 
- It maintains a copy of older deployments so that it is easy for the developer to rollback any changes made on the application.
- Elastic Beanstalk is best suited for running web applications that are developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker.

## CloudFormation vs BeanStalk

> CloudFormation deals more with the AWS infrastructure rather than applications.

> CloudFormation supports Elastic Beanstalk application environments. This allows you, for example, to create and manage an AWS Elastic Beanstalk–hosted application along with an RDS database to store the application data.

## Blue-Green Deployment

Use **Swap Environment URLs feature**

> Because AWS Elastic Beanstalk performs an in-place update when you update your application versions, your application might become unavailable to users for a short period of time. To avoid this, perform a blue/green deployment. To do this, deploy the new version to a separate environment, and then swap the CNAMEs of the two environments to redirect traffic to the new version instantly.

> DNS Switch can be managed gradually by using the Amazon Route 53 weighted routing policy.

## Things to consider

- In some scenarios, sharing a data store isn’t desired or feasible. Schema changes are too complex to decouple. 
- Data locality introduces too much performance degradation tothe application, as when the blue and green environments are in geographically disparate regions. 
- All of these situations require a solution where the data store is inside of the deployment environment boundary and tightly coupled to the blue and green applications respectively.

> You should consider using feature flags in your application to make it deployment aware.
 
