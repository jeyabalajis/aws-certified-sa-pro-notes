# Logging

## CloudWatch

- CloudWatch Unified agent: Collect additional system-level metrics such as RAM, processes etc.
- Batch log events for increasing throughput.
- Use Kinesis agent to send logs to Kinesis.
- Neither CloudWatch Logs Agent and CloudWatch Unified Agent can send logs to Kinesis!

> CloudWatch alarms are NOT triggered by actions such as EC2 state changes. CloudWatch Events can be triggered by EC2 state changes!

> CloudWatch unified agent CANNOT collect EC2 state changes!

## X-Ray

- Use X-Ray for distributed tracing
