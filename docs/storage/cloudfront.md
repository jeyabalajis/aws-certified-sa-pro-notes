# CloudFront

## Salient Features
- Can have either S3 bucket as origin (with OAI) or can have ALB + EC2 Auto-scaling as origin (dynamic)
- Always use CloudFront as a cheaper option for cross-region access of both static and dynamic content!
- Cheaper than cross-region replication
- Geo-restriction, Signed URL
- Custom Header solution to restrict access of origins _only from CloudFront_
- Maximize cache hits by separating static and dynamic distributions (can cache based on headers and cookie for dynamic content)

### Edge functions
- Apply filtering or authentication or authorization logic at the edge
- Manipulate HTTP Requests and Responses
- Bot mitigation at the edge
- Edge location: CloudFront functions. Regional Edge Cache: Lambda@Edge Functions
- CloudFront functions: Very low latency (1ms).  Native feature of CloudFront (manage code entirely within CloudFront)
    - No access to request body
    - Only JavaScript   
- Lambda@Edge: NodeJs or Python. Scales to 1000s of requests per second. High Latency (5 seconds)
    - Load content based on user-device. Can re-write URLs based on the user agent.


![Throughput rates - Cache - Storage](../images/throughput_rates_cache_storage.png)
 