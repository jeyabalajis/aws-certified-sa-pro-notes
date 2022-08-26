# AWS SAM

- Use AWS Serverless Application Model (SAM) and set up AWS Code Build, AWS Code Deploy, and AWS Code Pipeline to build a CI/CD Pipeline.

## Deploying Serverless Applications Gradually

- If you use AWS SAM to create your serverless application, it comes built-in with CodeDeploy to provide gradual Lambda deployments. 

With just a few lines of configuration, AWS SAM does the following for you:

- Deploys new versions of your Lambda function, and automatically creates aliases that point to the new version.
- Gradually shifts customer traffic to the new version until you're satisfied that it's working as expected, or you roll back the update.
- Defines pre-traffic and post-traffic test functions to verify that the newly deployed code is configured correctly and your application operates as expected.
- Rolls back the deployment if CloudWatch alarms are triggered.

### Auto Publish Alias

- Detects when new code is being deployed, based on changes to the Lambda function's Amazon S3 URI.
- Creates and publishes an updated version of that function with the latest code.
- Creates an alias with a name that you provide (unless an alias already exists), and points to the updated version of the Lambda function.

### Deployment Preference Type

- Canary: Two increments. Shift a percent of traffic, wait for a prescribed amount of time & shift remaining.
- Linear: Traffic is shifted in equal increments with an equal number of minutes between each increment.
- All At Once: All traffic is shifted from the original Lambda function to the updated Lambda function version at once.

