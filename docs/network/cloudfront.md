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

