# Route 53

## Routing Internet Traffic to AWS Resources
- Route traffic to an Amazon Virtual Private Cloud interface endpoint by using your domain name
- To route domain traffic to an ELB load balancer, use Amazon Route 53 to create an alias record that points to your load balancer. 
     - An alias record is a Route 53 extension to DNS.
- To route domain traffic to an S3 bucket, use Amazon Route 53 to create an alias record that points to your bucket.
- If you want to use your own domain name, use Amazon Route 53 to create an alias record that points to your CloudFront distribution.