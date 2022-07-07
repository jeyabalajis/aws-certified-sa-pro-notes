# Logging

## CloudWatch

- CloudWatch Unified agent: Collect additional system-level metrics such as RAM, processes etc.
- Batch log events for increasing throughput.
- Use Kinesis agent to send logs to Kinesis.
- Neither CloudWatch Logs Agent and CloudWatch Unified Agent can send logs to Kinesis!

### Alarms

- A CloudWatch alarm watches a metric and triggers when they exceed a certain threshold or decrease below some point. You can configure it to send a message to an Amazon SNS topic you subscribed to. So, you can get notified when an alarm is triggered. 

### Events

- Most AWS services emit events to Amazon CloudWatch Events when their states change or a specific event occurs in that service.
- For example, if a deployment fails on AWS CodeDeploy, it submits an event to CloudWatch Events. 
- You can define an event rule for an event to take an action when that event occurs.

> Events are triggered when a state changes (or) on a particular schedule. Alarm requires a threshold to be reached!

> Alarms invoke actions only for sustained changes. Alarms watch a single metric and respond to changes in that metric; events can respond to actions (for example, call a lambda function based on EC2 state change).

> **You cannot trigger an AWS Lambda function directly from a CloudWatch Alarm**

> For proactive scale-out ot scale-in actions, CloudWatch alarms come in handy.

> You can use Amazon CloudWatch Events to build event-driven architectures

> CloudWatch alarms are NOT triggered by actions such as EC2 state changes. CloudWatch Events can be triggered by EC2 state changes!

> CloudWatch unified agent CANNOT collect EC2 state changes!

## X-Ray

- Use X-Ray for distributed tracing
