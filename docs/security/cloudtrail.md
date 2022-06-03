# Cloud Trail

## CloudTrail Events
- Management events (write or read) *are enabled by default*
- Data events (S3 objects or Lambda Invocations) *are not enabled by default*
- Default 90 days retention. Beyond that, push to S3 and use Athena for analysis.

## CloudTrail Insights
- Enable CloudTrail Insights to detect unusual activity in your account

## Scenarios

### Multi-region or Multi-account logging
- Push multiple CloudTrail events into a common S3 (cross-account access through bucket policy)
- Trails can be pushed into management account for all member accounts

### Alerts for API Calls
- Metric filters of CloudWatchLogs can be used to detect a high level of API happening. 
- Example: Count occurrences of EC2 TerminateInstances API

### How to react to events faster?
- Cloudtrail delivery to Cloudwatch events (Event Bridge) is the fastest. 
- Cloudtrail delivery to Cloudwatch logs. Can use metric filters to analyze occurrences and anomalies.
- S3 storage - can be used for long term storage, cross-account delivery, logs integrity.




