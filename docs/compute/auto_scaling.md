# Auto-Scaling

## ALB based solution architecture
- Use Route 53 CNAME Weighted record to route traffic across multiple ALBs. Each ALB can connect to an auto-scaling group.


## Auto-scaling and Unhealthy Instances

- CodeDeploy can run pre-install tasks to remove an instance from an ELB Load balancer, suspend health checks before install, and then reinstate.

## Termination policies
- Amazon EC2 Auto Scaling uses termination policies to determine which instances it terminates first during scale-in events.
- Amazon EC2 Auto Scaling also provides instance scale-in protection. When you enable this feature, it prevents instances from being terminated during scale-in events.

> Instance scale-in protection does not guarantee that instances won't be terminated in the event of a human error—for example, if someone manually terminates an instance using the Amazon EC2 console or AWS CLI. 
> To protect your instance from accidental termination, you can use Amazon EC2 termination protection.

> During availability zone rebalancing, Amazon EC2 Auto Scaling **launches new instances before terminating the old ones**, so that rebalancing does not compromise the performance or availability of your application. To ensure that maximum capacity limits are not breached, a 10% margin is applied.

## Termination protection vs. Instance protection

- Termination protection WILL NOT PREVENT an auto scaling group (ASG) from terminating (Instance protection will).

## CodeDeploy and auto scaling

- AWS CodeDeploy is a service that automates application deployments to your fleet of servers. 
- Auto Scaling is a service that lets you dynamically scale your fleet based on load. 
- You can use them together for hands-free deployments! 
- Whenever new Amazon EC2 instances are launched as part of an Auto Scaling group, CodeDeploy can automatically deploy your latest application revision to the new instances.

### Under the Hood

The interaction happens using Auto scaling lifecycle hooks.

- CodeDeploy listens only for notifications about instances that have launched and are about to be put in the _InService_ state. 
- This state occurs after the EC2 instance has finished booting, but before it is put behind any Elastic Load Balancing load balancers you have configured. 
- Auto Scaling waits for a successful response from CodeDeploy before it continues working on the instance.

#### Steps
1. Auto Scaling asks EC2 for a new instance.
2. EC2 spins up a new instance with the configuration provided by Auto Scaling.
3. Auto Scaling sees the new instance, puts it into Pending:Wait status, and sends the notification to Code Deploy.
4. CodeDeploy receives the instance launch notification from Auto Scaling.
5. CodeDeploy validates the configuration of the instance and the deployment group.
6. CodeDeploy creates a new deployment for the instance to deploy the target revision of the deployment group

### Pre-Requisites
- Install the CodeDeploy agent on the Auto Scaling instance. 
- You can either bake the agent as part of the base AMI or use user data to install the agent during launch.
- Make sure the service role used by CodeDeploy to interact with Auto Scaling has the correct permissions. You can use the _AWSCodeDeployRole_ managed policy.  

> CodeDeploy can run pre-install tasks to remove an instance from ELB, suspend health checks before install and then reinstate.
