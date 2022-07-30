# X-Ray

## X-Ray Daemon

- The AWS X-Ray daemon is a software application that listens for traffic on UDP port 2000, gathers raw segment data, and relays it to the AWS X-Ray API. 
- The daemon works in conjunction with the AWS X-Ray SDKs and must be running so that data sent by the SDKs can reach the X-Ray service.
- Lambda runs the daemon automatically any time a function is invoked for a sampled request. 
- On Elastic Beanstalk, use the XRayEnabled configuration option to run the daemon on the instances in your environment.
- To run the X-Ray daemon locally, on-premises, or on other AWS services, download it, run it, and then give it permission to upload segment documents to X-Ray.

## X-Ray API

- The X-Ray API provides access to all X-Ray functionality through the AWS SDK, AWS Command Line Interface, or directly over HTTPS. 
- The X-Ray API Reference documents input parameters for each API action, and the fields and data types that they return.
- You can use the AWS SDK to develop programs that use the X-Ray API. 
- The X-Ray console and X-Ray daemon both use the AWS SDK to communicate with X-Ray. 
- The AWS SDK for each language has a reference document for classes and methods that map to X-Ray API actions and types.

### Languages
- Java
- JavaScript
- .NET
- Ruby
- Go
- PHP
- Python
