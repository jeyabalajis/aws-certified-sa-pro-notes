# CloudFormation

## DeletionPolicy

- Retain: retain the resource in the event of a stack deletion
- Snapshot: available for resources that support snapshots  
- Delete: Delete the resource on deletion of a stack - default option

> The default policy is Snapshot for AWS::RDS::DBCluster resources and for AWS::RDS::DBInstance resources that don't specify the DBClusterIdentifier property.

## CloudFormation and OpsWorks

CloudFormation also supports OpsWorks. You can model OpsWorks components (stacks, layers, instances, and applications) inside CloudFormation templates, and provision them as CloudFormation stacks. This enables you to document, version control, and share your OpsWorks configuration.
 
> The default policy is Snapshot for AWS::RDS::DBCluster resources and for AWS::RDS::DBInstance resources that don't specify the DBClusterIdentifier property. 

## UpdatePolicy

Use the UpdatePolicy attribute to specify how AWS CloudFormation handles updates to the following resources:

- AWS::AppStream::Fleet

- AWS::AutoScaling::AutoScalingGroup: 

This policy enables you to specify whether AWS CloudFormation replaces an Auto Scaling group with a new one or replaces only the instances in the Auto Scaling group.

> During a rolling update, suspend the following Auto Scaling processes:
- HealthCheck
- ReplaceUnhealthy
- AZRebalance
- AlarmNotification
- ScheduledActions

- AWS::ElastiCache::ReplicationGroup

- AWS::OpenSearchService::Domain

- AWS::Elasticsearch::Domain

- AWS::Lambda::Alias: For AWS::Lambda::Alias resources, CloudFormation performs an CodeDeploy deployment when the version changes on the alias. 

## CloudFormation vs ElasticBeanStalk

> For Infrastructure resources, CloudFormation is the choice. Elastic Beanstalk is for packaging application code, without the need to setup ALB etc.  