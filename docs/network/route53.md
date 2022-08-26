# Amazon Route 53

- Route 53 is designed to propagate updates you make to your DNS records to its worldwide network of authoritative DNS servers _within 60 seconds_ under normal conditions.
- Routing policies: Simple, Weighted, Latency based, Failover (Active-Passive), Geo-location, Geo-proximity, 

> Alias: Points a hostname to an AWS Resource. Works for both root domain and non-root domain.
> CNAME: Points a hostname to any other hostname. Only for non-root domain.
> You cannot set an ALIAS record for an EC2 DNS name

> For EC2 instances, always use a Type A Record without an Alias. 
>For ELB, Cloudfront and S3, always use a Type A Record with an Alias 
>For RDS, always use the CNAME Record with no Alias.

## Routing Internet Traffic to AWS Resources
- Route traffic to an Amazon Virtual Private Cloud interface endpoint by using your domain name
- To route domain traffic to an ELB load balancer, use Amazon Route 53 to create an alias record that points to your load balancer. 
     - An alias record is a Route 53 extension to DNS.
- To route domain traffic to an S3 bucket, use Amazon Route 53 to create an alias record that points to your bucket.
- If you want to use your own domain name, use Amazon Route 53 to create an alias record that points to your CloudFront distribution.
- To route domain traffic to an ELB load balancer, use Amazon Route 53 to create an alias record that points to your load balancer.

## Weighted Routing Policy
- Weighted routing policy: Use to route traffic to multiple resources in proportions that you specify. 
- You can use weighted routing to create records in a private hosted zone.
- Weighted routing lets you associate multiple resources with a single domain name (example.com) or subdomain name (acme.example.com) and choose how much traffic is routed to each resource. 
- This can be useful for a variety of purposes, including load balancing and testing new versions of software.

## Geo-location routing
- Geo-location routing lets you choose the resources that serve your traffic based on the geographic location of your users, meaning the location that DNS queries originate from. 
- For example, you might want all queries from Europe to be routed to an ELB load balancer in the Frankfurt region.

> Geo-location routing is based on location of the users. On the other hand, use *Geo-proximity routing* when you want to route traffic _based on the location of your resources_ and, optionally, shift traffic from resources in one location to resources in another.

> If you have bigger resources in a location, _Geoproximity_ allows you to specify more bias or weight to that location. This is not possible with Geo-Location. 
> On the other hand, you can restrict users from a region or location with Geo-location routing policy. 

## Multi-value routing

When a client makes a DNS request with multivalue answer routing, Route 53 responds to DNS queries with up to **eight healthy records selected at random for the particular domain name**. These records can each be attached to a Route 53 health check, which helps prevent clients from receiving a DNS response that is not reachable.

> Multi-value answer routing policy may cause the users to be **randomly sent to other healthy regions** that may be far away from the user's location.
> So, if you want to maintain proximity as much as possible, use failover answer routing policy.

> For simple availability enhancement, setting up Non-alias record with a multi-value answer configuration to target IP addresses of the web servers could be a good solution.

## Active-Active vs. Active-Passive

- You configure active-active failover using any routing policy (or combination of routing policies) other than failover
- You configure active-passive failover using the failover routing policy.

> Use an active-passive failover configuration when you want a primary resource or group of resources to be available the majority of the time and you want a secondary resource or group of resources to be on standby in case all the primary resources become unavailable. 

> You can setup active-passive with weighted routing policy - by tweaking weight of certain targets as zero. However, this may reduce availability since the last healthy resource (with weight 0) may not be able to handle the traffic.


## CNAME vs Alias Record
- Unlike a CNAME record, you can create an alias record at the top node of a DNS namespace, also known as the zone apex.
- You can't create a CNAME record that has the same name as the hosted zone (the zone apex). This is true both for hosted zones for domain names (example.com) and for hosted zones for subdomains (zenith.example.com).
- But alias record can be created at the top node or zone apex.
- Route 53 doesn't charge for alias queries to AWS resources

> When you use an alias record to route traffic to an AWS resource, Route 53 automatically recognizes changes in the resource. 
> For example, suppose an alias record for example.com points to an ELB load balancer at lb1-1234.us-east-2.elb.amazonaws.com. 
> If the IP address of the load balancer changes, Route 53 automatically starts to respond to DNS queries using the new IP address.

## Hosted Zone and Health Checks

> A hosted zone is a container for records, and records contain information about how you want to route traffic for a specific domain, such as example.com, and its subdomains (acme.example.com, zenith.example.com).

### Public Hosted Zones
- Contains records that specify how to route traffic on the internet (public domain names)

> DNS Security Extensions (DNSSEC) works only with _Public Hosted Zones_ 

### Private Hosted Zones
- Contains records that specify how to route traffic within one or more VPCs (private domain names).

> Must enable _enableDnsHostNames_ and _enableDnsSupport_ for private hosted zones.

### Health checks:

- Health Checks provide _Automated DNS Failover_.

- Health checks that monitor an endpoint
- Health checks that monitor other health checks (calculated health checks). Perform maintenance on your website without causing all healthchecks to fail.
- Health checks that monitor CloudWatch alarms

> Route 53 health checkers can’t access private endpoints.

Use case:  RDS multi-region failover (Promote Read Replica)
- Use health endpoint (or) CloudWatch Alarms
- Create an CW event out of health checks (or) push to SNS Topic
- Lambda function to update DNS Records in Route 53

DNS Firewall is a feature of Route53 Resolver.
DNS exfiltration can happen when a bad actor compromises an application instance in your VPC and then uses DNS lookup to send data out of the VPC to a domain that they control.
With DNS Firewall, you can monitor and control the domains that your applications can query.
DNS Firewall is a feature of Route 53 Resolver and doesn't require any additional Resolver setup to use.

> **DNSSEC validation only applies to public signed names in Amazon Route 53, and not to forwarded zones.**

> Health check of private hosted zones can be done through Cloud Watch Metric and Cloud Watch Alarms.
> Create a CW Metric and associate a CW alarm - then create a Route 53 Health check that checks the alarm itself.
 
> Health check for databases: They need to either be connected to CloudWatch Alarms or talk through an HTTP enabled application as a proxy to checking the health of your RDS database, which sits inside a private endpoint.

### Associate Route53 private hosted zone with a VPC in a different account

> If private hosted zone has to be shared with another account, VPC Peering must be in place, and perform associate-vpc-with-hosted-zone.

1. Authorize the association between the private hosted zone in Account A and the VPC in Account B. **Perform this in Account A**

```commandline
aws route53 create-vpc-association-authorization --hosted-zone-id <hosted-zone-id> --vpc VPCRegion=<region>,VPCId=<vpc-id> --region us-east-1
```

2. Create the association between the private hosted zone in Account A and the VPC in Account B. **Perform this in Account B**

```commandline
aws route53 associate-vpc-with-hosted-zone --hosted-zone-id <hosted-zone-id> --vpc VPCRegion=<region>,VPCId=<vpc-id> --region us-east-1
``` 

3. Delete the authorization in account A

```commandline
aws route53 delete-vpc-association-authorization --hosted-zone-id <hosted-zone-id>  --vpc VPCRegion=<region>,VPCId=<vpc-id> --region us-east-1
``` 

**Amazon EC2 instances in the VPC from Account B can now resolve records in the private hosted zone in Account A.**


[Route53 private hosted zone](https://aws.amazon.com/premiumsupport/knowledge-center/route53-private-hosted-zone/) 

## DNS Resolvers / Hybrid DNS

### Inbound Resolver

- _Inbound Resolver_: This allows your DNS resolvers to easily resolve domain names for AWS resources such as EC2 instances or records in a Route 53 private hosted zone.

> Inbound here is from the perspective of Route 53. Any DNS resolver in your on-premises network can reach Route 53 inbound resolver to reach AWS resources (such as EC2 instances and Route 53 Private Hosted Zones).

**To accomplish inbound, link up domain names of your AWS resources to Route 53 inbound endpoint _in your on-premise resolver_.**

### Outbound Resolver

- _Outbound Resolver_: Resolver conditionally forwards queries to resolvers on your network (_on-premises_) via this endpoint
- Outbound resolve requires resolver rules.

> Outbound is from the perspective of Route 53. Route 53 can _conditionally_ forward queries to on-premises resolvers. 
>A classic example of this is to resolve On-prem Active Directory servers through Route 53 resolvers. 

> **Conditional forwarding rules** – You create conditional forwarding rules (also known as forwarding rules) when you want to forward DNS queries for specified domain names to DNS resolvers on your network.

#### Outbound Resolver rules
- Conditional Forwarding Rules
- System Rules
- Auto-defined System Rules
- If multiple rules are matched, Route 53 chooses the mose specific match.

> Route 53 resolver is _within a VPC_, can be deployed across multiple AZ. 10,000 queries per second per IP.
