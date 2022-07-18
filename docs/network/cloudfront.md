# CloudFront

If you want to speed up delivery of your web content, you can use Amazon CloudFront, the AWS content delivery network (CDN). CloudFront can deliver your entire website—including dynamic, static, streaming, and interactive content—by using a global network of edge locations. Requests for your content are automatically routed to the edge location that gives your users the lowest latency.

## Best Practices

> EC2 Instance or ALB must be public for CloudFront to talk to them. The security group of ALB must allow public IPs of CloudFront.

> CloudFront Signed URL requires an application server that authenticates and then generates a CloudFront Signed URL for the client.

> CloudFront Signed URL: Allow access to a path no matter the origin, leverage caching features. S3 Pre-Signed URL: impersonates the person who pre-signed the URL; limited lifetime.

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

## CloudFront vs Global Accelerator
- Global accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP.
- Also good for HTTP use cases that require static IP addresses
- CloudFront uses multiple sets of dynamically changing IP addresses while Global Accelerator will provide you a set of static IP addresses as a fixed entry point to your applications.

> CloudFront uses the edge location to cache, while GA uses the edge location to find the optimal path to the origin.

Global Accelerator Use Case:
- For example, you have a banking application that is scattered through multiple AWS regions and low latency is a must. 
- Global Accelerator will route the user to the nearest edge location then route it to the nearest regional endpoint where your applications are hosted. 

## CloudFront Signed URL

- To create signed URLs or signed cookies, you need a signer. 
- A signer is either a trusted key group that you create in CloudFront, or an AWS account that contains a CloudFront key pair. 
- We recommend that you use trusted key groups with signed URLs and signed cookies.

> S3 cannot be used as a signer for CloudFront.

## Field Level Encryption

- Field-level encryption adds an additional layer of security that lets you protect specific data throughout system processing so that only certain applications can see it.
- The sensitive information provided by your users is encrypted at the edge, close to the user, and remains encrypted throughout your entire application stack. This encryption ensures that only applications that need the data—and have the credentials to decrypt it—are able to do so.

> To use field level encryption, you origin must support **chunked encoding**

    
