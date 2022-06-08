# Guard Duty

Amazon GuardDuty is a threat detection service that provides you with an accurate and easy way to continuously monitor and protect AWS accounts and workloads.

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