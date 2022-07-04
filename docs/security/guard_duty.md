# Guard Duty

Amazon GuardDuty is a threat detection service that provides you with an accurate and easy way to continuously monitor and protect AWS accounts and workloads.

> GuardDuty analyzes continuous metadata streams generated from your account and network activity found in AWS CloudTrail Events, Amazon Virtual Private Cloud (VPC) Flow Logs, and domain name system (DNS) Logs. 
>GuardDuty also uses integrated threat intelligence such as known malicious IP addresses, anomaly detection, and machine learning (ML) to more accurately identify threats.

## Reconnaissance: 
Activity suggesting reconnaissance by an attacker, such as unusual API activity, intra-VPC port scanning, unusual failed login request patterns, or unblocked port probing from a known bad IP.

## Instance compromise: 
Activity indicating an instance compromise, such as cryptocurrency mining, backdoor command and control (C&C) activity, malware using domain generation algorithms (DGA), outbound denial of service activity, unusually high network traffic volume, unusual network protocols, outbound instance communication with a known malicious IP, temporary Amazon EC2 credentials used by an external IP address, and data exfiltration using DNS.

## Account compromise: 
Common patterns indicative of account compromise include API calls from an unusual geolocation or anonymizing proxy, attempts to disable AWS CloudTrail logging, changes that weaken the account password policy, unusual instance or infrastructure launches, infrastructure deployments in an unusual region, and API calls from known malicious IP addresses.

## Bucket compromise: 
Activity indicating a bucket compromise, such as suspicious data access patterns indicating credential misuse, unusual Amazon S3 API activity from a remote host, unauthorized S3 access from known malicious IP addresses, and API calls to retrieve data in S3 buckets from a user with no prior history of accessing the bucket or invoked from an unusual location. Amazon GuardDuty continuously monitors and analyzes AWS CloudTrail S3 data events (e.g. GetObject, ListObjects, DeleteObject) to detect suspicious activity across all of your Amazon S3 buckets.

### Typical use cases
- GuardDuty can detect signs of account compromise, such as AWS resource access from an unusual geo-location at an atypical time of day.

> AWS CloudTrail Management Event analysis: GuardDuty continuously analyzes CloudTrail management events, monitoring all access and behavior of your AWS accounts and infrastructure. CloudTrail management event analysis is charged per 1,000,000 events per month and pro-rated.

> AWS CloudTrail S3 Data Event analysis: GuardDuty continuously analyzes CloudTrail S3 data events, monitoring access and activity of all your Amazon S3 buckets. CloudTrail S3 data event analysis is charged per 1,000,000 events per month and is pro-rated.

> VPC Flow Log and DNS Log analysis: GuardDuty continuously analyzes VPC Flow Logs and DNS requests and responses to identify malicious, unauthorized, or unexpected behavior in your AWS accounts and workloads. Flow log and DNS log analysis is charged per Gigabyte (GB) per month. Flow log and DNS log analysis is offered with tiered volume discounts.