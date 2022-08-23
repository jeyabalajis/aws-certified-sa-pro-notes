# Systems Manager

AWS Systems Manager is a collection of capabilities to help you manage your applications and infrastructure running in the AWS Cloud. 

- Simplifies application and resource management
- Shortens the time to detect and resolve operational problems
- Helps you manage your AWS resources securely at scale.

- Application management
    - Application Manager: Aggregates operations information from multiple AWS services and Systems Manager capabilities to a single application.
    - AppConfig: Create, manage and deploy application configurations and feature flags.
    - Parameter Store: Provides secure, hierarchical storage for configuration data and secrets management. 
- Change management
    - Change Manager: Enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure.
    - Automation: automate common maintenance and deployment tasks
    - Change Calendar: setup date or time ranges when actions you specify can or can't be performed.
    - Maintenance Windows: set up recurring schedules for managed instances to run administrative tasks such as installing patches and updates without interrupting business-critical operations.
- Node management
    - Compliance
    - Fleet Manager
    - Inventory
    - Session Manager
    - Run Command: Remotely and securely manage the configuration of your managed nodes at scale.
    - State Manager: Automate the process of keeping your managed nodes in a defined state.
    - Patch Manager: automate the process of patching your managed nodes with both security related and other types of updates.
    - Distributor: create and deploy packages to managed nodes. With Distributor, you can package your own software—or find AWS-provided agent software packages, such as AmazonCloudWatchAgent—to install on Systems Manager managed nodes.
    - Hybrid Activations: To set up servers and VMs in your hybrid environment as managed instances, create a managed instance activation.
- Operations management
    - Incident Manager
    - Explorer
    - OpsCenter
    - CloudWatch Dashboards

## Automation

AWS Systems Manager Automation simplifies common maintenance and deployment tasks for Amazon Elastic Compute Cloud (Amazon EC2) instances and other AWS resources. Automation enables you build automations to configure and manage instances and AWS resources. You can create custom runbooks or use predefined runbooks maintained by AWS.

- SSM Automation offers one-click automation for simplifying complex tasks such as creating golden Amazon Machines Images (AMIs) and recovering unreachable EC2 instances.
- For example, you can use Use the AWS-UpdateLinuxAmi and AWS-UpdateWindowsAmi runbooks to create golden AMIs from a source AMI.
- You can run custom scripts before and after updates are applied. 
- You can also include or exclude specific packages from being installed. 

## SSM Agent

- AWS Systems Manager Agent (SSM Agent) is Amazon software that runs on Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, and on-premises servers and virtual machines (VMs). 
- SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources.
