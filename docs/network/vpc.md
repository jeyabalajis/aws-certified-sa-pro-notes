# VPC

## Salient points
- NAT Gateway is resilient to failure within a single AZ. Require multiple NAT Gateways for resilience across multi-AZ.
- NACL is stateless. Incoming does not mean outgoing is allowed. Both allow and deny rules. Applied at subnet level.
- Security Group is stateful. Only allow rules, deny rules not possible. Instance level
- Can reference other security groups in the same region.
- Egress Only Internet Gateway for IPV6 traffic
- Transitive connections are not allowed with VPC Peering, establish one on one. CIDR cannot overlap.
- VPC Peering can work inter-region and cross-account.
- Edge to edge routing not allowed with VPC Peering. 
    - For example: VPC A and VPC B are paired. 
    - If VPC B is connected to a Site-site VPN (or) Direct connect, VPC A cannot use them
    - Edge to edge routing through a VPC gateway endpoint not allowed
    - Edge to edge routing through an internet gateway not allowed  
- For edge-to-edge routing and transitive connections, _use Transit Gateway_, which supports more complex routing rules, overlapping CIDR ranges, network-level packet filtering.     

###  Bastion Hosts
1. SSH into private EC2 instances through a public EC2 instance (bastion host)
2. You must manage these instances yourself (failover, recovery)
3. SSM Session Manager is a more secure way to remote control without SSH

### AWS PrivateLink
1. You do not need to use an internet gateway, NAT device, public IP address, AWS Direct Connect connection, or AWS Site-to-Site VPN connection to allow communication with the service from your private subnets.
2. Use Cases:
    - Connect to other aws services (hosted in a separate VPC)
    - Connect to other AWS Account service, through VPC endpoint service (requires NLB)
    - Connect to an AWS Marketplace partner service
3. Endpoint services that are hosted by service providers.
4. Service consumers create interface VPC endpoints to connect to the endpoint services (created in service provider account)
5. You can configure Amazon Route 53 to route domain traffic to a VPC endpoint. 

### Transit Gateway
- Works with Direct Connect Gateway, VPN connections
- Supports IP Multicast (not supported by any other AWS service)
- Instances in a VPC can access a NAT Gateway, NLB, PrivateLink, and EFS in others VPCs attached to the AWS Transit Gateway (This is not possible with Transit Gateway)

> Without AWS Transit Gateway, you have to combine an internet gateway with NAT gateways or NAT instances for each VPC needing outbound internet access. 
> However, if you have more significant numbers of VPCs, the management of multiple internet gateways and NAT gateways and instances adds labor and costs. 
> In that case, you can save overhead by centralizing outbound traffic with AWS Transit Gateway.
> With Transit Gateway, communication between VPCs through NAT Gateway can be denied 
>& outbound communication to internet from VPCs can be routed through Transit Gateway, which in turn routes all traffic to Internet Gateway.  

### VPC Endpoint policies
- Does not replace IAM Policies or resource based policies.
- use aws:SourceVpc in S3 bucket policies to allow access only from endpoint (more secure) 