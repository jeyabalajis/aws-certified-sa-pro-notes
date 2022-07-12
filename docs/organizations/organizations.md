# Organizations

## Consolidated Billing

- For billing purposes, the consolidated billing feature of AWS Organizations treats all the accounts in the organization as one account. 
- This means that all accounts in the organization can receive the hourly cost-benefit of Reserved Instances that are purchased by any other account. 
- In the payer account, you can turn off Reserved Instance discount sharing on the Preferences page on the Billing and Cost Management console.

> By default, member account DOES NOT have the capability to turn-off RI sharing on their account.

## AWS Config and Organizations

1. Multi-account, multi-region data aggregation in AWS Config enables you to aggregate AWS Config data from multiple accounts and AWS Regions into a single account.

### Enable trusted access to AWS Config through service-linked role

```
aws organizations enable-aws-service-access \ 
    --service-principal config.amazonaws.com
```
