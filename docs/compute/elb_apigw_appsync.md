# ELB, API Gateway and AppSync

## ELB

### ALB:
- ALB vs CLB: ALB supports SNI (Server Name Indication) that allows multiple SSL certificates to be stored.
- ALB: Load balancing to multiple HTTP applications across machines (target groups) - one more level of indirection before the actual targets are identified.
- ALB: Load balancing to multiple applications within a container (through dynamic port mapping)
- ALB: Routing rules for path, header, query string
- ALB: Health checks are at target group level. Targets can be EC2 instances (auto-scaling group), ECS Service, Lambda functions or private IPs.

#### Application Load Balancer (V2)
With Application Load Balancers, the load balancer node that receives the request uses the following process:
- Evaluates the listener rules in priority order to determine which rule to apply.
- Selects a target from the target group for the rule action, using the routing algorithm configured for the target group. The default routing algorithm is round robin. Routing is performed independently for each target group, even when a target is registered with multiple target groups.

> ALB supports Round Robin and least outstanding requests. Consider using least outstanding requests when the requests for your application vary in complexity or your targets vary in processing capability.
> NLB uses a **flow hash** algorithm.

#### Classic Load Balancer
With Classic Load Balancers, the load balancer node that receives the request selects a registered instance as follows:
- Uses the round robin routing algorithm for TCP listeners
- Uses the least outstanding requests routing algorithm for HTTP and HTTPS listeners


### NLB:

- NLB: Layer 4 - forward TCP & UDP traffic to your instances. Handles millions of requests per second. around 100ms latency vs. 400 ms for ALB
- NLB: can redirect to only IP Addresses
- NLB: Target groups: private IPs, EC2 instances or ALB

- NLB: NLB **does not** have a security group. Basically, you will have the ability to either use the security group function already associated with your EC2 Instance’s network card (ENI), a VPC Network Access Control List (NACL), AWS Network Firewall, or some other type of marketplace solution to provide the necessary security controls that you are seeking. 
- Note that NLBs do not have security groups configured and pass connections straight to EC2 instances with the source IP of the client preserved (when registered by instance-id).You must ensure that the security groups for these instances allow traffic on both the listener port and the health check port.

With Network Load Balancers, the load balancer node that receives the connection uses the following process:
- Selects a target from the target group for the default rule using a flow hash algorithm. It bases the algorithm on:
  - The protocol
  - The source IP address and source port
  - The destination IP address and destination port
  - The TCP sequence number
- Routes each individual TCP connection to a single target for the life of the connection. The TCP connections from a client have different source ports and sequence numbers, and can be routed to different targets.

### GLB:

- GLB: New way to run virtual network appliances in the cloud
- GLB: Gateway Load Balancer helps you easily deploy, scale, and manage your third-party virtual appliances. 
- GLB: It gives you one gateway for distributing traffic across multiple virtual appliances while scaling them up or down, based on demand. 
- GLB: Centralize your third-party virtual appliances, Add third-party security appliances to your network, visibility of third-party analytics solutions
- GLB: Operates at Layer 3 (Network Layer) – IP Packet
- GLB: Target groups: EC2 instances and Private IP Addresses

### Cross-Zone Load Balancing:

- NLB: Cross-zone load balancing is disabled by default and need to pay $ for cross-AZ traffic
- GLB: Cross-zone load balancing is disabled by default and need to pay $ for cross-AZ traffic
- NLB: Flow Hash request routing. Each TCP/UDP connection is routed to a single target for the life of the connection
- ALB can be a target group for NLB. You can now easily combine the benefits of NLB, including PrivateLink and zonal static IP addresses, with the advanced routing offered by ALB to load balance traffic to your applications.

> Gateway Load Balancer provides both Layer 3 gateway and Layer 4 load balancing capabilities. 
It is a transparent bump-in-the-wire device that does not change any part of the packet. 
It is architected to handle millions of requests/second, volatile traffic patterns, and introduces extremely low latency.

> All load balancers support Connection draining (deregistration delay)



## API Gateway

### Security & Logging:

- CORS (Cross-origin resource sharing): Browser based security. Control which domains can call your API.
- Authn: Cognito User Pools, Lambda authorizer, AWS_IAM based access (expects AccessKey and SecretKey, which can be obtained through STS API)
- Can send logs directly into Kinesis Data Firehose (as an alternative to CW logs)
- X-Ray: Enable tracing to get extra information about requests in API Gateway. X-Ray API Gateway + AWS Lambda gives you the full picture.

### API Gateway Errors

5xx means Server errors 
- 502: Bad Gateway Exception, usually for an incompatible output returned from a Lambda proxy integration backend and occasionally for out-of-order invocations due to heavy loads.
- 503: Service Unavailable Exception
- 504: Integration Failure – ex Endpoint Request Timed-out Exception. API Gateway requests time out after 29 second maximum

4xx means Client errors 
- 400: Bad Request
- 403: Access Denied, WAF filtered
- 429: Quota exceeded, Throttle

## AWS AppSync

- AppSync is a managed service that uses GraphQL
- Retrieve data in real-time with WebSocket or MQTT on WebSocket
- AppSync is the gateway that sits in between data consumers and data providers (can be databases, Lambda, ElasticSearch or HTTP APIs)
