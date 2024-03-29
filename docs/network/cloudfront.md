# CloudFront

If you want to speed up delivery of your web content, you can use Amazon CloudFront, the AWS content delivery network (CDN). CloudFront can deliver your entire website—including dynamic, static, streaming, and interactive content—by using a global network of edge locations. Requests for your content are automatically routed to the edge location that gives your users the lowest latency.

- CloudFront also offers basic DDoS protections with AWS Shield standard offered for free for use with CloudFront.

## Best Practices

> EC2 Instance or ALB must be public for CloudFront to talk to them. The security group of ALB must allow public IPs of CloudFront.

> CloudFront Signed URL requires an application server that authenticates and then generates a CloudFront Signed URL for the client.

> CloudFront Signed URL: Allow access to a path no matter the origin, leverage caching features. S3 Pre-Signed URL: impersonates the person who pre-signed the URL; limited lifetime. _Use Signed cookies for multiple files._

> Restrict access to custom origins and ALB: Use a custom-header and set ALB rule to accept requests that contain this custom header. Keep the custom-header name & value a secret.

> Whitelist headers to optimize cache hit ratio. Too many headers means less cache hits.

> Maximize cache hit by separating static and dynamic content. For static content, no header caching rules are required.  For dynamic content, cache based on correct headers and cookie.

> If your API clients are geographically dispersed, consider using an edge-optimized API endpoint in API Gateway. 
This type of endpoint acts like a regional endpoint, but has an AWS-managed CloudFront web distribution in front of it to help improve the client connection time.

> To use the global CloudFront content delivery network and maintain more control over the distribution, you can use a regional API with a custom CloudFront web distribution.

> CloudFront functions: Low latency, sub-ms latency and can handle millions of requests per second. Deployed at the Edge location. Cache Key normalization.

> Lambda@Edge, can handle 1000s of requests per second. Deployed at the Regional Edge Cache. Lambda@Edge can be used as a global application that invokes lambda & connects with DynamoDB and cache data.

> We recommend that you configure your HTTP server to add a Content-Length header to prevent CloudFront from caching partial objects.

> For enabling PCI-DSS compliance on sensitive information, leverage **Field Level Encryption** feature of CloudFront. 

> You cannot cache API Gateway results in CloudFront. Enable API Caching at API Gateway level itself.

## CloudFront vs Global Accelerator

**They both use AWS global network and it's edge locations around the world.**

- Global accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP.
- Also good for HTTP use cases that require static IP addresses
- CloudFront uses multiple sets of dynamically changing IP addresses while Global Accelerator will provide you a set of static IP addresses as a fixed entry point to your applications.

> CloudFront uses the edge location to **cache**, while GA uses the edge location to **find the optimal path to the origin**.

Global Accelerator Use Case:
- For example, you have a banking application _that is scattered through multiple AWS regions_ and low latency is a must. 
- Global Accelerator will route the user to the nearest edge location then route it to the nearest regional endpoint where your applications are hosted. 
- AWS Global Accelerator provides improved performance and high availability **when you have copies of your application running in multiple AWS Regions**. 
- GA leverages global aws network and is beneficial if the resources are scattered across regions but still latency CANNOT be sacrificed.

> GA preserves client IP except for NLBs and EIPs endpoints.

## CloudFront Signed URL

- To create signed URLs or signed cookies, you need a signer. 
- A signer is either a trusted key group that you create in CloudFront, or an AWS account that contains a CloudFront key pair. 
- We recommend that you use trusted key groups with signed URLs and signed cookies.

> S3 cannot be used as a signer for CloudFront.

## Field Level Encryption

- Field-level encryption adds an additional layer of security that lets you protect specific data throughout system processing so that only certain applications can see it.
- The sensitive information provided by your users is encrypted at the edge, close to the user, and remains encrypted throughout your entire application stack. This encryption ensures that only applications that need the data—and have the credentials to decrypt it—are able to do so.

> To use field level encryption, you origin must support **chunked encoding**

> you must create an RSA key pair that includes a public key and a private key. The public key enables CloudFront to encrypt data, and the private key enables components at your origin to decrypt the fields that have been encrypted. You can use OpenSSL or another tool to create a key pair. The key size must be 2048 bits.

[CloudFront Field Level Encryption](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html)

## Viewer Protocol Policy

Choose the protocol policy that you want viewers to use to access your content in CloudFront edge locations:

- HTTP and HTTPS: Viewers can use both protocols.
- Redirect HTTP to HTTPS: Viewers can use both protocols, but HTTP requests are automatically redirected to HTTPS requests.
- HTTPS Only: Viewers can only access your content if they're using HTTPS.

## Lambda@Edge

- Lambda@Edge can be configured to inspect the viewer request and look for the user-agent HTTP header.
- This header is a string that can be used to identify the application, operating system, vendor, and/or version of the requesting user agent.
- Based on the operating system of the client, the function can then return different media assets from the CloudFront cache.
- Lambda@Edge can be used in viewer request, origin request, origin response and viewer response.

### Key Use Cases
- Inspect cookies to rewrite URLs to different versions of a site for A/B testing.
- Send different objects to your users based on the User-Agent header, which contains information about the device that submitted the request. For example, you can send images in different resolutions to users based on their devices.
- Inspect headers or authorized tokens, inserting a corresponding header and allowing access control before forwarding a request to the origin.
- Add, delete, and modify headers, and rewrite the URL path to direct users to different objects in the cache.
- Generate new HTTP responses to do things like redirect unauthenticated users to login pages, or **create and deliver static webpages right from the edge**.

## Origin Failover

- You can set up CloudFront with origin failover for scenarios that require high availability. 
- To get started, you create an origin group with two origins: a primary and a secondary. 
- If the primary origin is unavailable, or returns specific HTTP response status codes that indicate a failure, CloudFront automatically switches to the secondary origin.

> **Tip:** You can use Lambda@Edge function in origin failover use cases. 

## Pricing

### Price Class

- If you’re willing to accept potentially higher latency for viewers in some geographic regions in return for lower cost, you can choose a price class that doesn’t include all geographic regions.

### Savings Bundle

- The CloudFront security savings bundle is a simple way to save up to 30% on the CloudFront charges on your AWS bill when you make an upfront commitment.
- When you purchase a savings bundle, you also get credits for AWS WAF, a web application firewall that helps protect your CloudFront distribution against common web exploits.

## Origin Access Identity

- Access to S3 origin is restricted through CloudFront.
- Bucket policy is created that provides bucket access to only OAI as _AWS Principal_.
- With OAI, an identity is created and that is in turn used as AWS principal in S3 Bucket policy to restrict access.

## Behavior

- Redirect requests to specific origin based on path pattern
- Example: If file type is .mp4, go to origin 1, else to origin 2. There could be a default behavior too.

## Geo-Restriction

- Restrict access to specific countries based on white-list or black-list based approach.

## Cache Invalidation

- You need to pay for invalidation requests.

## CloudFront HTTPS

### HTTPS between viewers and CloudFront

- You can use a certificate that was issued by a trusted certificate authority (CA) such as Comodo, DigiCert, Symantec or other third-party providers.
- You can use a certificate provided by AWS Certificate Manager (ACM)

### HTTPS between CloudFront and a custom origin

- If the origin is not an ELB load balancer, such as Amazon EC2, **the certificate must be issued by a trusted CA such as Comodo, DigiCert, Symantec or other third-party providers**.
- If your origin is an ELB load balancer, _you can also use a certificate provided by ACM_.
- If the origin is Amazon EC2 instances, certificate must be issued by a trusted CA such as Comodo.
- If you're using your own domain name, you also need to use an SSL/TLS certificate provided by AWS Certificate Manager (ACM), or import a certificate from a third-party certificate authority into ACM or the IAM certificate store.
