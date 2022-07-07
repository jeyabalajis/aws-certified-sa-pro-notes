# Cost Control

## Cost Allocation Tags

- Cost Allocation Tags just appear in the Billing Console
- Takes up to 24 hours for the tags to show up in the report

> A tag is a label that you or AWS assigns to an AWS resource. 
>Each tag consists of a key and a value. A key can have more than one value. 
>You can use tags to organize your resources, and cost allocation tags to track your AWS costs on a detailed level. 
>After you activate cost allocation tags, AWS uses the cost allocation tags to organize your resource costs on your cost allocation report, to make it easier for you to categorize and track your AWS costs.

[Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html?ref=wellarchitected)

## AWS Tag Editor

- Allows you to manage tags of multiple resources at once


## Trusted Advisor

- Can enable weekly email notification from the console
- Full Trusted Advisor – Available for Business & Enterprise support plans
	- Ability to set CloudWatch alarms when reaching limits
	- Programmatic Access using AWS Support API
- Recommendation on Cost Optimization & Recommendations, and Service Limits

> Trsuted Advisor cannot check for S3 objects that are public inside of your bucket! Use CloudWatch Events / S3 Events instead.

### Service Limits

- Limits can only be monitored in Trusted Advisor (cannot be changed)
- Cases have to be created manually in AWS Support Centre to increase limits
- OR use the new AWS Service Quotas service

## AWS Savings Plan

- Commit to a certain type of usage: ex $10 per hour for 1 to 3 years
- Any usage beyond the savings plan is billed at the on-demand price

## S3 Cost Savings

- Requester Pays: If an IAM role is assumed, the owner account of that role pays for the request
- S3 Select & Glacier Select: save in network and CPU cost
- S3 Lifecycle Rules: transition objects between tiers
- Compress objects to save space
- There are no retrieval charges in S3 Intelligent-Tiering

## AWS Budgets

- Create budget and send alarms when costs exceeds the budget
- 4 types of budgets: Usage, Cost, Reservation, Savings Plans
- For Reserved Instances (RI)
- Track utilization
- Supports EC2, ElastiCache, RDS, Redshift
- Up to 5 SNS notifications per budget

## AWS Costs Explorer

- Choose an optimal Savings Plan (to lower prices on your bill)
- Forecast usage up to 12 months based on previous usage

## Cost Management Best Practices
- Only allow specific groups or teams to deploy chosen AWS resources.
- Create policies for each environment.
- Require tags in order to instantiate resources.
- Monitor and send alerts or shut down instances that are improperly tagged.
- Use CloudWatch to send alerts when billing thresholds are met.
- Analyze spend using AWS or partner tools.
