# Certificate Manager

- It's a best practice to use IAM as a certificate manager when you must support HTTPS connections in a Region that isn't supported by ACM. 
- For more information, see Managing server certificates in IAM.

> ACM certificates can be used only with services integrated with ACM.

## Services integrated with ACM

- Elastic Load Balancing
- Amazon CloudFront
- Amazon Cognito
- AWS Elastic BeanStalk
- AWS App Runner
- Amazon API Gateway
- AWS Nitro Enclaves
- AWS CloudFormation
- AWS Amplify
- Amazon OpenSearch Service

## Public vs. Private CA

- Public SSL/TLS certificates provisioned through AWS Certificate Manager are free. You pay only for the AWS resources you create to run your application.
- ACM Private Certificate Authority (CA) is priced along two dimensions. 
  - You pay a monthly fee for the operation of each private CA until you delete it and 
  - You pay for the private certificates you issue each month.

### Private Certificate Authority Operation
- $400.00 per month for each ACM private CA until you delete the CA.
- ACM Private CA operation is pro-rated for partial months based on when you create and delete the CA. 
- You are not charged for a private CA after you delete it. 
- However, if you restore a deleted CA, you are charged for the time between deleting it and restoring it.

> From a cost perspective, it is cheaper to provision a public certificate and attach on ALB, than provision a private certificate and attach on EC2 instances.
