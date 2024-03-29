# VPC

## Salient points
- NAT Gateway is resilient to failure within a single AZ. Require multiple NAT Gateways for resilience across multi-AZ.
- NAT Gateway is created in public subnet (i.e. a subnet that has internet gateway). 0.0.0.0/0 is routed to NAT Gateway from private subnet.
- NAT gateway can be assigned an _Elastic IP Address_ which can then be used as a single white-listed IP for external APIs that demand the same.
- Private NAT Gateway: Instances in private subnets can connect to other VPCs or your on-premises network through a private NAT gateway.
    - You can route traffic from the NAT gateway through a transit gateway or a virtual private gateway.
    - If you route traffic from the private NAT gateway to the internet gateway, the internet gateway drops the traffic.
- NAT instance uses a special AMI with the string “amzn-ami-vpc-nat” in the name. 
    - Must disable source/destination checks. 
    - Need to assign security group.
    - Can use as bastion host
    - Can implement port forwarding through customization (not possible with NAT Gateway)
- NACL is stateless. Incoming does not mean outgoing is allowed. Both allow and deny rules. Applied at subnet level.
- Security Group is stateful. Only allow rules, deny rules not possible. Instance level.
- Subnet routes. _Longest prefix wins._
- Can reference other security groups in the same region.
- Egress Only Internet Gateway for IPV6 traffic
- An Egress-only Internet Gateway allows IPv6 traffic outbound but not inbound
- Transitive connections are not allowed with VPC Peering, establish one on one. CIDR cannot overlap.
- VPC Peering can work inter-region and cross-account.
- Edge to edge routing not allowed with VPC Peering. 
    - For example: VPC A and VPC B are paired. 
    - If VPC B is connected to a Site-site VPN (or) Direct connect, VPC A cannot use them
    - Edge to edge routing through a VPC gateway endpoint not allowed
    - Edge to edge routing through an internet gateway not allowed  
- For edge-to-edge routing and transitive connections, _use Transit Gateway_, which supports more complex routing rules, overlapping CIDR ranges, network-level packet filtering.     

> When configuring NACL in a private subnet to reach ALB node (which is in a public subnet), an outbound rule for ports 1024 through 65535 to destination CIDR is required. This is because, **source port of ALB Node is a dynamically defined high number port between 1024 and 65535.** 

##  Bastion Hosts
1. SSH into private EC2 instances through a public EC2 instance (bastion host)
2. You must manage these instances yourself (failover, recovery)
3. SSM Session Manager is a more secure way to remote control without SSH

## AWS PrivateLink (VPC Endpoint Service)

AWS PrivateLink is a highly available, scalable technology that enables you to privately connect your VPC to services as if they were in your VPC.

1. You do not need to use an internet gateway, NAT device, public IP address, AWS Direct Connect connection, or AWS Site-to-Site VPN connection to allow communication with the service from your private subnets.
2. Use Cases:
    - Connect to other aws services (hosted in a separate VPC)
    - Connect to other AWS Account service, through VPC endpoint service (requires NLB)
    - Connect to an AWS Marketplace partner service
3. Endpoint services that are hosted by service providers.
4. Service consumers create interface VPC endpoints to connect to the endpoint services (created in service provider account)
5. You can configure Amazon Route 53 to route domain traffic to a VPC endpoint.
6. If the NLB is in multiple AZ, and the ENI in multiple AZ, the solution is fault tolerant!
7. **NLB on the service provider side and VPC endpoint on the service consumer side. **

### Interface endpoint

1. An elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported AWS service, endpoint service, or AWS Marketplace service.
2. Leverage security groups for security
3. Uses Private DNS entries to redirect traffic.
4. Interface can be accessed from Direct Connect and Site-to-Site VPN, through intra-region VPC peering connections from Nitro instances, and through inter-region VPC peering connections from any type of instance.

### Under the Hood

- You must ensure that the security group that's associated with the endpoint network interface allows communication between the endpoint network interface and the resources in your VPC that communicate with the service.
- When you create an interface endpoint, endpoint-specific DNS hostnames are generated. For AWS services and AWS Marketplace Partner services, the private DNS option (enabled by default) associates a private hosted zone with your VPC.
- The hosted zone contains a record set for the default DNS name for the service (for example, ec2.us-east-1.amazonaws.com) that resolves to the private IP addresses of the endpoint network interfaces in your VPC. **This enables you to make requests to the service using its default DNS hostname instead of the endpoint-specific DNS hostnames.**
- With Private DNS enabled on the endpoint, instances can send requests AWS services through the interface endpoint using either the default DNS hostname or the endpoint-specific DNS hostname.

### VPC Endpoint policies (Gateway endpoint)
- Does not replace IAM Policies or resource based policies.
- use aws:SourceVpc in S3 bucket policies to allow access only from endpoint (more secure) 

> To provide aws-only access from EC2 instances to S3 bucket, create S3 Gateway endpoint and set endpoint policy with _SourceVpce_ as a condition.

### Gateway Endpoint
1. Works only for S3 and DynamoDB
2. Gateway is defined at VPC Level and must update route table entries with prefix list.
3. A Prefix List is a collection of CIDR blocks that can be used to configure VPC security groups, VPC route tables, and AWS Transit Gateway route tables and can be shared with other AWS accounts using Resource Access Manager (RAM).
4. Gateway endpoint **cannot** be extended out of a VPC (VPN, DX, TGW, peering)
5. Gateway endpoint employs routing and requires prefix lists in the route table to redirect traffic.

## Transit Gateway
- Works with Direct Connect Gateway, VPN connections
- Supports IP Multicast (not supported by any other AWS service)
- Instances in a VPC can access a NAT Gateway, NLB, PrivateLink, and EFS in others VPCs attached to the AWS Transit Gateway (This is not possible with Transit Gateway)

> Without AWS Transit Gateway, you have to combine an internet gateway with NAT gateways or NAT instances for each VPC needing outbound internet access. 
> However, if you have more significant numbers of VPCs, the management of multiple internet gateways and NAT gateways and instances adds labor and costs. 
> In that case, you can save overhead by centralizing outbound traffic with AWS Transit Gateway.
> With Transit Gateway, communication between VPCs through NAT Gateway can be denied 
>& outbound communication to internet from VPCs can be routed through Transit Gateway, which in turn routes all traffic to Internet Gateway.  

## Site-to-Site VPN

1. Setup a software or hardware VPN appliance to your on-premises network.
2. On-premises VPN must be accessible through public IP.
3. Customer Gateway on the data center side, Virtual Gateway on aws side
4. Can use Global Accelerator for worldwide networks.  
5. An accelerated Site-to-Site VPN connection (accelerated VPN connection) uses AWS Global Accelerator to route traffic from your on-premises network to an AWS edge location that is closest to your customer gateway device.
6. Static routing to allow bi-directional traffic. Use BGP (Border Gateway Protocol) for dynamic routing.
7. The cost of a VPN is very less when compared with AWS Direct Connect. Also, there is an option of VPN per connection hour pricing which is not available with Direct Connect.
8. AWS VPN CloudHub can connect upto 10 Customer Gateway for a single virtual gateway. The traffic goes through public network only.
9. Can be a failover connection between your on-premises locations
10. VPN to multiple VPC: Direct Connect is preferred since it has direct connect gateway.

## Direct Connect
- Provides a dedicated private connection from a remote network to your VPC
- Dedicated connection must be setup between your DC and AWS Direct Connect locations
- More expensive than running a VPN solution
- Private access to AWS services through VIF
- Bypass ISP, reduce network cost, increase bandwidth and stability
- Not redundant by default (must setup a failover DX or VPN)
- Dedicated connections: Lead times are often longer than 1 month to establish a new connection, but offers upto 100 Gbps
- Hosted connections: upto 10 Gbps from partners
- **Data in transit is not encrypted but is private**
- AWS Direct Connect + VPN provides an IPsec-encrypted private connection (complicated to setup)
- LAG: Get increased speed and failover by summing up existing DX connections into a logical one (up to 4 can be aggregated)
- If you want to setup a Direct Connect to one or more VPC in many different regions (same/cross account), you must use a Direct Connect Gateway

## NAT Gateway

> If you have resources in multiple Availability Zones and they share one NAT gateway, in the event that the NAT gateway’s Availability Zone is down, resources in the other Availability Zones lose internet access.

## VPC Sharing

VPC sharing allows customers to share subnets with other AWS accounts within the same AWS Organization. This is a very powerful concept that allows for a number of benefits:

1. Separation of duties: centrally controlled VPC structure, routing, IP address allocation.
2. Application owners continue to own resources, accounts, and security groups.
3. VPC sharing participants can reference security group IDs of each other.
4. Efficiencies: higher density in subnets, efficient use of VPNs and AWS Direct Connect.
5. Hard limits can be avoided, for example, 50 VIFs per AWS Direct Connect connection through simplified network architecture.
5. Costs can be optimized through reuse of NAT gateways, VPC interface endpoints, and intra-Availability Zone traffic.

> Essentially, we can now decouple accounts and networks. I expect customers to continue to have multiple VPCs even with VPC sharing. But they can now have fewer, larger, centrally managed VPCs. Highly interconnected apps automatically benefit from this approach.

### Best Practices

- Isolate subnets across child accounts to reduce blast radius. On non-production accounts, share subnets
- Have dedicated subnets for shared AWS infrastructure components such as VPC interface endpoints, firewall endpoints, and NAT Gateways.
- Isolate environments (dev, staging, production) and isolate VPCs per each environment
- Segregate security zones inside a VPC, such as External zone, Customer Zone and Internal Zone, with different permissions
- For bigger scale, have a separate VPC per each unique security zone.
- VPC Sharing uses RAM for controlling which subnets are shared with which AWS accounts.
- Enable fow logs at each VPC that is shared
- Amazon GuardDuty logs findings on the owner account  
