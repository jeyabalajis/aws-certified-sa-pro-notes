# Web Application Firewall (WAF)

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to an _Amazon CloudFront distribution_, an _Amazon API Gateway REST API_, an _Application Load Balancer_, or an _AWS AppSync GraphQL API_.

WAF provides additional protection against web attacks using the criteria you specify.

> If you have a requirement to filter all inbound requests _for common vulnerability attacks_ in your web app (layer 7), WAF is a good choice for the same.

## Criteria

- IP addresses that requests originate from.
- Country that requests originate from.
- Values in request headers.
- Strings that appear in requests, either specific strings or strings that match regular expression (regex) patterns.
- Length of requests.
- Presence of SQL code that is likely to be malicious (known as SQL injection).
- Presence of a script that is likely to be malicious (known as cross-site scripting).

## WAF vs Shield

AWS Shield Advanced provides expanded DDoS attack protection for your Amazon EC2 instances, Elastic Load Balancing load balancers, CloudFront distributions, Route 53 hosted zones, and AWS Global Accelerator standard accelerators. AWS Shield Advanced incurs additional charges.

## WAF vs Firewall Manager

AWS Firewall Manager simplifies your administration and maintenance tasks across multiple accounts and resources for a variety of protections, including AWS WAF, AWS Shield Advanced, Amazon VPC security groups, AWS Network Firewall, and Amazon Route 53 Resolver DNS Firewall.

## Which one should I choose

- Start with WAF
- You can automate and then simplify AWS WAF management using AWS Firewall Manager. 
- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.

> If you want granular control over the protection that is added to your resources, AWS WAF alone is the right choice. If you want to use AWS WAF across accounts, accelerate your AWS WAF configuration, or automate protection of new resources, use Firewall Manager with AWS WAF.
> Finally, if you own high visibility websites or are otherwise prone to frequent DDoS attacks, you should consider purchasing the additional features that Shield Advanced provides.

## WAF Logging 
- You can enable logging to get detailed information about traffic that is analyzed by your web ACL.
- Logged information includes the time that AWS WAF received a web request from your AWS resource, detailed information about the request, and details about the rules that the request matched. 
- You can send your logs to an Amazon CloudWatch Logs log group, an Amazon Simple Storage Service (Amazon S3) bucket, or an Amazon Kinesis Data Firehose.
