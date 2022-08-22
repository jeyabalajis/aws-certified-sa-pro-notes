# Advanced Networking

## ENI

- An elastic network interface is a logical networking component in a VPC that represents a virtual network card. It includes one primary IPV4 and multiple secondary IPV4 addresses.
- You cannot detach a primary network interface from an instance. 
- You can create and attach additional network interfaces. 
- The maximum number of network interfaces that you can use varies by instance type.

## Forward Web Proxy

- A forward proxy server acts as an intermediary for requests from internal users and servers, often caching content to speed up subsequent requests.
- Companies usually implement proxy solutions to provide URL and web content filtering, IDS/IPS, data loss prevention, monitoring, and advanced threat protection. 
- AWS customers often use a VPN or AWS Direct Connect connection to leverage existing corporate proxy server infrastructure, or build a forward proxy farm on AWS using software such as Squid proxy servers with internal Elastic Load Balancing (ELB).

> A NACL or Security Group cannot filter requests based on URLs.

## Centralized Egress

- NAT gateway is a managed network address translation service. 
- Deploying a NAT gateway in every spoke VPC can become cost prohibitive because you pay an hourly charge for every NAT gateway you deploy, so centralizing it could be a viable option. 
- To centralize, you create a separate egress VPC in the network services account and route all egress traﬃc from the spoke VPCs via a NAT gateway sitting in this VPC using Transit Gateway.

![image](https://user-images.githubusercontent.com/15995686/182532291-f10a1c46-b2db-4968-a434-48149bd78f07.png)

## Hybrid Connectivity

### One-to-one connectivity

- In this setup, a VPN connection and/or Direct Connect private VIF is created for every VPC. 
- This is accomplished by using the virtual private gateway (VGW). 
- This option is great for small numbers of VPCs, but as a customer scales their VPCs, managing hybrid connectivity per VPC can become difficult.

### Edge consolidation
- In this setup, customers consolidate hybrid IT connectivity for multiple VPCs at a single endpoint. 
- All the VPCs share these hybrid connections. 
- This is accomplished by using **AWS Transit Gateway and the Direct Connect Gateway**.
