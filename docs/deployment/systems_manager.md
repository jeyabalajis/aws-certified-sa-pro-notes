# Systems Manager

AWS Systems Manager is a collection of capabilities for configuring and managing your Amazon EC2 instances, on-premises servers and virtual machines, and other AWS resources at scale.

- With Systems Manager, you can select a resource group and view its recent API activity, resource configuration changes, related notifications, operational alerts, software inventory, and patch compliance status. 
- Systems Manager provides a central place to view and manage your AWS resources, so you can have complete visibility and control over your operations.
- Systems Manager enables you to manage AWS IoT Greengrass devices alongside Amazon Elastic Compute Cloud (EC2) instances and on-premises servers. 

## Patch Manager

- Patch Manager, a capability of AWS Systems Manager, automates the process of patching managed nodes with both security related and other types of updates. You can use Patch Manager to apply patches for both operating systems and applications.
- You can patch fleets of Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, or your on-premises servers and virtual machines (VMs) by operating system type. 

> AWS doesn't test patches before making them available in Patch Manager. Also, Patch Manager doesn't support upgrading major versions of operating systems, such as Windows Server 2016 to Windows Server 2019, or SUSE Linux Enterprise Server (SLES) 12.0 to SLES 15.0.

> Patch Manager integrates with AWS Identity and Access Management (IAM), AWS CloudTrail, and Amazon EventBridge to provide a secure patching experience that includes event notifications and the ability to audit usage.

### Patch Baselines

- Patch Manager uses patch baselines, which include rules for auto-approving patches within days of their release, in addition to a list of approved and rejected patches. 
- You can install patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task.

#### Patch Group

> A patch group is an optional means of organizing instances for patching. For example, you can create patch groups for different operating systems (Linux or Windows), different environments (Development, Test, and Production), or different server functions (web servers, file servers, databases). 

## Fleet Manager

Fleet Manager is operating system (OS) agnostic. You can use Fleet Manager to perform common OS operations on Windows, Linux, and Mac-based servers. 

- File system and log exploration: Use the Systems Manager console to browse through disks, folders, and files, including file-based logs, on servers. 
- Performance counter monitoring: Monitor common server performance metrics, such as CPU utilization, network traffic, disk usage, and memory utilization.
- Windows Event management: View and troubleshoot Windows Events logs without the need to install additional agents. 

## Compliance

> Using an integration with AWS Config, you can monitor an instance's compliance with a desired configuration through AWS Config rules. This capability allows security experts and compliance auditors to have a complete audit trail of instance configuration changes, as well as receive proactive notifications in the event of non-compliance.

## OpsCenter

- OpsCenter enables users to reduce their MTTR for operational issues, in some cases by over 50%.
- OpsCenter enables standardization and aggregation of operational issues (OpsItems) across various resources in a single place. 
- Additionally, it brings together contextual information and operational tooling required to investigate and remediate issues.

## AppConfig

- AWS AppConfig is a feature of AWS Systems Manager that allows you to quickly validate and roll out configurations across an application of any size, whether hosted on Amazon EC2 instances, containers, AWS Lambda functions, mobile apps, or IoT devices, in a controlled and monitored way.
- AWS AppConfig enables you to validate configuration data to make sure it is syntactically and semantically correct according to your definitions before deploying it to your application.

### AppConfig vs CodeDeploy

> We recommend that you use AWS AppConfig to apply safety mechanisms when deploying new configurations and AWS CodeDeploy when deploying new code.

### AppConfig vs AWS Config

- AWS Config enables you to assess, audit, and evaluate the configurations of your AWS resources while AWS AppConfig lets you manage application configuration.
- You should use AWS Config to get a detailed view of the configuration of AWS resources in your account and identify how the resources were configured in the past and how the configurations change over time.
- AWS AppConfig is meant for your applications running on AWS resources or on-premises servers.

## Parameter Store

> You can reference Systems Manager parameters to build generic configuration and automation scripts for use across AWS services such as Amazon ECS and AWS CloudFormation.

> For automated rotation of passwords and integration with database secrets, use Secrets Manager.

## Maintenance Window

AWS Systems Manager lets you schedule windows of time to run administrative and maintenance tasks across your instances. This ensures that you can select a convenient and safe time to install patches and updates or make other configuration changes, improving the availability and reliability of your services and applications.

> You can create and schedule any AWS Systems Manager run command execution, AWS Systems Manager automation document execution, AWS Step Functions, or AWS Lambda functions as tasks.

## Configuration Compliance

AWS Systems Manager lets you scan your managed instances for patch compliance and configuration inconsistencies. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant. 

## Inventory

AWS Systems Manager collects information about your instances and the software installed on them, helping you to understand your system configurations and installed applications. You can collect data about applications, files, network configurations, Windows services, registries, server roles, updates, and any other system properties.

> Both EC2 instances and on-premise instances are supported.
