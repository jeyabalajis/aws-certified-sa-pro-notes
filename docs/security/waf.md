# Web Application Firewall (WAF)

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to an _Amazon CloudFront distribution_, an _Amazon API Gateway REST API_, an _Application Load Balancer_, or an _AWS AppSync GraphQL API_.

WAF provides additional protection against web attacks using the criteria you specify.

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