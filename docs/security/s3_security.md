# S3 Security

## SSE-S3

- When you use Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3), each object is encrypted with a unique key. 
- As an additional safeguard, it encrypts the key itself with a root key that it regularly rotates. 
- Amazon S3 server-side encryption uses one of the strongest block ciphers available, 256-bit Advanced Encryption Standard (AES-256) GCM, to encrypt your data.

> To request server-side encryption using the object creation REST APIs, provide the x-amz-server-side-encryption request header. 

## SSE-KMS
- Server-Side Encryption with AWS KMS keys (SSE-KMS) is similar to SSE-S3, but with some additional benefits and charges for using this service. 
- There are separate permissions for the use of a KMS key that provides added protection against unauthorized access of your objects in Amazon S3. 
- SSE-KMS also provides you with an audit trail that shows when your KMS key was used and by whom. 
- Additionally, you can create and manage customer managed keys or use AWS managed keys that are unique to you, your service, and your Region. 

## SSE-C

- With Server-Side Encryption with Customer-Provided Keys (SSE-C), you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects.

> With SSE-KMS, you manage keys with KMS. with SSE-C, you manage the encryption keys outside of aws. With SSE-S3, aws manages the encryptions keys, and internally uses KMS.

## Block Public Access

- BlockPublicAcls: PUT Bucket acl and PUT Object acl calls fail if the specified access control list (ACL) is public. PUT object call fails. Enabling this setting doesn’t affect existing policies or ACLs.
- IgnorePublicAcls: Setting this option to TRUE causes Amazon S3 to ignore all public ACLs on a bucket and any objects that it contains. This setting enables you to _safely block public access granted by ACLs while still allowing PUT Object calls that include a public ACL_ (as opposed to BlockPublicAcls, which rejects PUT Object calls that include a public ACL). Enabling this setting doesn’t affect the persistence of any existing ACLs and doesn’t prevent new public ACLs from being set.
- BlockPublicPolicy: Setting this option to TRUE for a bucket causes Amazon S3 to reject calls to PUT Bucket policy if the specified bucket policy allows public access, and to reject calls to PUT access point policy for all of the bucket's access points if the specified policy allows public access. To use this setting effectively, you should apply it at the account level.
- RestrictPublicBuckets: Setting this option to TRUE restricts access to an access point or bucket with a public policy _to only AWS service principals and authorized users within the bucket owner's account_.

### Access Analyzer

- You can use Access Analyzer for S3 to review buckets with bucket ACLs, bucket policies, or access point policies that grant public access. 
- Access Analyzer for S3 alerts you to buckets that are configured to allow access to anyone on the internet or other AWS accounts, including AWS accounts outside of your organization. 
- For each public or shared bucket, you receive findings that report the source and level of public or shared access.

## Macie

- Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover, monitor, and help you protect your sensitive data in Amazon Simple Storage Service (Amazon S3). 
- Macie automates the discovery of sensitive data, such as personally identifiable information (PII) and intellectual property, to provide you with a better understanding of the data that your organization stores in Amazon S3.
- Amazon Macie automates the discovery of sensitive data at scale and lowers the cost of protecting your data. 
- Macie automatically provides an inventory of Amazon S3 buckets including a list of unencrypted buckets, publicly accessible buckets, and buckets shared with AWS accounts outside those you have defined in AWS Organizations. 
- Then, Macie applies machine learning and pattern matching techniques to the buckets you select to identify and alert you to sensitive data, such as personally identifiable information (PII).

> Integrated with Amazon Event Bridge, which can further integrate with other services such as Step Functions to take automated remediation actions.
> This can help you meet regulations, such as the Health Insurance Portability and Accountability Act (HIPAA) and General Data Privacy Regulation (GDPR).

## S3 Compliance

You can use **AWS Config Auto Remediation feature** to *auto remediate any non-compliant S3 buckets* using the following AWS Config rules:
- s3-bucket-logging-enabled
- s3-bucket-server-side-encryption-enabled
- s3-bucket-public-read-prohibited
- s3-bucket-public-write-prohibited

These AWS Config rules act as controls to prevent any non-compliant S3 activities.
