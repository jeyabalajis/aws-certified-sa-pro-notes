# Route 53

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