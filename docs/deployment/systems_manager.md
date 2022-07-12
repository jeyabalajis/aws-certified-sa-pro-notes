# Systems Manager

AWS Systems Manager is a collection of capabilities for configuring and managing your Amazon EC2 instances, on-premises servers and virtual machines, and other AWS resources at scale.

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
