# Step Functions

Step Functions is used to coordinate multiple serverless functions in a workflow.

## Step Functions vs. SQS

For asynchronous or event driven processing, go with SQS. For coordinating **multiple serverless functions in a workflow**, use Step Functions. 

> An SQS queue cannot be used as a direct input for an AWS Step Function workflow. An SQS queue can be read by a Lambda, which in turn can invoke a step function.
