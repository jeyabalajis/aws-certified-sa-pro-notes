# CloudFormation

## DeletionPolicy

- Retain: retain the resource in the event of a stack deletion
- Snapshot: available for resources that support snapshots  
- Delete: Delete the resource on deletion of a stack - default option

> The default policy is Snapshot for AWS::RDS::DBCluster resources and for AWS::RDS::DBInstance resources that don't specify the DBClusterIdentifier property.

## CloudFormation and OpsWorks

CloudFormation also supports OpsWorks. You can model OpsWorks components (stacks, layers, instances, and applications) inside CloudFormation templates, and provision them as CloudFormation stacks. This enables you to document, version control, and share your OpsWorks configuration.
 