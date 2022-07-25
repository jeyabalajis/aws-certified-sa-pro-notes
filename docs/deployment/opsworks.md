# OpsWorks

## OpsWorks Stacks

> AWS OpsWorks Stacks is an application and server management service. With OpsWorks Stacks, 
>you can model your application as a stack containing different layers, such as load balancing, database, and application server. 
>Within each layer, you can provision Amazon EC2 instances, enable automatic scaling, and configure your instances 
>with Chef recipes using Chef Solo. This allows you to automate tasks such as installing packages and programming 
>languages or frameworks, configuring software, and more.

## Under the Hood

- In OpsWorks, you will be provisioning a stack and layers. 
- The stack is the top-level AWS OpsWorks Stacks entity. It represents a set of instances that you want to manage collectively, typically because they have a common purpose such as serving PHP applications. 
- In addition to serving as a container, a stack handles tasks that apply to the group of instances as a whole, such as managing applications and cookbooks.
- Every stack contains one or more layers, each of which represents a stack component, such as a load balancer or a set of application servers. As you work with AWS OpsWorks Stacks layers, keep the following in mind:
- Each layer in a stack must have at least one instance and can optionally have multiple instances.
- Each instance in a stack must be a member of at least one layer, except for registered instances. You cannot configure an instance directly, except for some basic settings such as the SSH key and hostname. You must create and configure an appropriate layer, and add the instance to the layer.

## OpsWorks vs. CloudFormation

Compared to CloudFormation, OpsWorks focuses more on orchestration and software configuration, and less on what and how AWS resources are procured.

## Best Practices

> For preparation work (like VPC, NAT etc.), it is okay to use CloudFormation. For dynamic workloads (EC2), OpsWorks is preferred.

## Updating Patches

By default, AWS OpsWorks Stacks automatically installs the latest updates during setup, after an instance finishes booting. 

> **AWS OpsWorks Stacks does not automatically install updates after an instance is online, to avoid interruptions such as restarting application servers. Instead, you manage updates to your online instances yourself, so you can minimize any disruptions.**

AWS recommends that you use one of the following to update your online instances:

- Create and start new instances to replace your current online instances. Then delete the current instances. The new instances will have the latest set of security patches installed during setup.
- On Linux-based instances in Chef 11.10 or older stacks, run the Update Dependencies stack command, which installs the current set of security patches and other updates on the specified instances.
