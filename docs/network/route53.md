# Route 53

- Routing policies: Simple, Weighted, Latency based, Failover (Active-Passive), Geo-location, Geo-proximity, 

> Alias: Points a hostname to an AWS Resource. Works for both root domain and non-root domain.
> CNAME: Points a hostname to any other hostname. Only for non-root domain.
> You cannot set an ALIAS record for an EC2 DNS name



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

## Hosted Zones
> Health check of private hosted zones can be done through Cloud Watch Metric and Cloud Watch Alarms.
> Health check for databases: They need to either be connected to CloudWatch Alarms or talk through an HTTP enabled application as a proxy to checking the health of your RDS database
> If private hosted zone has to be shared with another account, VPC Peering must be in place, and perform associate-vpc-with-hosted-zone.


## CNAME vs Alias Record
- Unlike a CNAME record, you can create an alias record at the top node of a DNS namespace, also known as the zone apex.
- You can't create a CNAME record that has the same name as the hosted zone (the zone apex). This is true both for hosted zones for domain names (example.com) and for hosted zones for subdomains (zenith.example.com).
- But alias record can be created at the top node or zone apex.
- Route 53 doesn't charge for alias queries to AWS resources

> When you use an alias record to route traffic to an AWS resource, Route 53 automatically recognizes changes in the resource. 
> For example, suppose an alias record for example.com points to an ELB load balancer at lb1-1234.us-east-2.elb.amazonaws.com. 
> If the IP address of the load balancer changes, Route 53 automatically starts to respond to DNS queries using the new IP address.