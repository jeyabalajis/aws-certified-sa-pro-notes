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

