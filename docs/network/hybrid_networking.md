# Hybrid Networking in AWS

## AWS Direct Connect
- Direct Connect allows for private communication over a dedicated line, and can provide additional security, performance, and privacy.
- It can also bridge the network architecture gap between on-premises networks and AWS.
- Direct Connect locations are public datacenters that have AWS operated private backbone connectivity to AWS regions.
- A virtual interface is like a subinterface on a router in a virtual routing and forwarding (VRF) table, with a route in the VRF toward a specific location.
- Virtual interfaces can be private or public. 
- Private virtual interfaces are used to route a Direct Connect connection to a specific VPC.
- Public virtual interfaces allow for direct routing into AWS’s public services.
- Key requirement: Your device must support Border Gateway Protocol (BGP) and BGP MD5 authentication.

> To connect to services such as EC2 using just Direct Connect you need to create a private virtual interface. However, if you want to encrypt the traffic flowing through Direct Connect, you will need to use the public virtual interface of DX to create a VPN connection that will allow access to AWS services such as S3, EC2, and other services. Direct Connect in itself does not provide encryption in-transit.

![image](https://user-images.githubusercontent.com/15995686/173286279-cc5571dc-fdeb-4c36-9836-d8006d5cf648.png)


## Direct Connect Gateways

- A Direct Connect gateway is a globally available resource. You can create the Direct Connect gateway in any Region and access it from all other Regions.
- A Direct connect Gateway is associated with either of the following:
    - A transit gateway when you have multiple VPCs in the same Region
    - A virtual private gateway

> **AWS Managed VPN can be combined with Direct Connect Gateway to provide an IPSEC-encrypted private connection**

## Transit Gateway
- AWS Transit Gateway connects your Amazon Virtual Private Clouds (VPCs) and on-premises networks through a central hub. Your data is automatically encrypted and never travels over the public internet.
- A route table includes dynamic and static routes that decide the next hop based on the destination IP address of the packet.
- Attachments: A Transit Gateway can be attached to one of the following:
    - One or more VPCs
    - A Connect SD-WAN/third-party network appliance
    - An AWS Direct Connect gateway
    - A peering connection with another transit gateway
    - A VPN connection to a transit gateway
- Example configurations
    - Centralized Router
    - Isolated VPCs
    - Isolated VPCs with shared services
    - Peering
    - Centralized outbound routing
    - Appliance VPC
 
 ![image](https://user-images.githubusercontent.com/15995686/173280790-8b98c725-63a5-4e7f-8de2-2a48f862627e.png)
 
## S3endpoints
- With S3 endpoints, you can create a private route in your VPC that allows you to route traffic directly to and from S3 in your VPC. 
- S3 was the first AWS service with endpoints, and AWS is constantly evaluating endpoints for other services.

## AWS PrivateLink
- Service consumers create interface VPC endpoints to connect to endpoint services that are hosted by service providers.
- Interface endpoint vs Gateway endpoint. Gateway endpoint is for just S3 and DynamoDB using Private IP Addresses and uses Route Table. On the other hand, Interface endpoint uses NLB for routing.

![image](https://user-images.githubusercontent.com/15995686/173285636-b9ac14f8-e406-4f0a-8780-eb882a319bfa.png)

## Global Accelerator
- Two single IP that can in turn be connected to Load Balancers and put origin servers behind this
- Leverage jitter and congestion free aws network to origin servers
- **Use cases**:
    - Scale for increased application utilization
    - Acceleration for latency-sensitive applications
    - Disaster recovery and multi-Region resiliency (if compute resources in one region fails, it can be rerouted to another region)
    - Global Accelerator decreases the risk of attack by masking your origin behind two static entry points. These entry points are protected by default from Distributed Denial of Service (DDoS) attacks with AWS Shield.
    - Using a custom routing accelerator, you can leverage the performance benefits of Global Accelerator for your VoIP or gaming applications.


## Site to Site VPN

A virtual private gateway is the VPN concentrator on the Amazon side of the Site-to-Site VPN connection. You create a virtual private gateway and attach it to the VPC from which you want to create the Site-to-Site VPN connection.

> **Only one virtual private gateway (VGW) can be attached to a VPC at a time.**



## Network to Amazon VPC Connectivity Options

### AWS Direct Connect + VPN

> This solution combines the benefits of the end-to-end secure IPSec connection with low latency 
>and increased bandwidth of the AWS Direct Connect to provide a more consistent network experience 
>than internet-based VPN connections.

### AWS Direct Connect + Transit Gateway

> This solution simplifies management of connections between an Amazon VPC and your networks over a private connection 
>that can reduce network costs, increase bandwidth throughput, and provide a more consistent network experience 
>than internet-based connections.

### AWS Direct Connect + TGW + VPN

> Consider taking this approach when you want to simplify management and minimize the cost of IPSec VPN connections to multiple Amazon VPCs in the same region, 
>with the low latency and consistent network experience benefits of a private dedicated connection over an internet-based VPN. 

### AWS VPN CloudHub

>  Use this approach if you have multiple branch offices and existing internet connections and would like 
>to implement a convenient, potentially low-cost hub-and-spoke model for primary or backup connectivity 
>between these remote offices.

### AWS TGW + VPN

> Consider using this approach when you want to take advantage of an AWS-managed VPN endpoint for connecting to multiple VPCs 
>in the same region without the additional cost and management of multiple IPSec VPN connections to multiple Amazon VPCs.

## VPC to VPC Connectivity options

### AWS PrivateLink

> We recommend this approach if you want to use services offered by another VPC securely within the AWS network, 
>with all network traffic staying on the global AWS backbone and never traversing the public internet.

### Transit Gateway

> AWS Transit Gateway is a highly available and scalable service to consolidate the AWS VPC routing configuration for a region with a hub-and-spoke architecture.

> Each VPC need not have a NAT Gateway. The egress traffic can be routed into Transit Gateway & onto other VPCs.

### Software Site-to-Site VPN

> This option is recommended when you want to manage both ends of the VPN connection using your preferred VPN software provider. This option uses an internet gateway attached to each VPC to facilitate communication between the software VPN appliances.

 
