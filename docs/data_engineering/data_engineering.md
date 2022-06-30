# Data Engineering

## Kinesis Overview

- Kinesis Streams: low latency streaming ingest at scale
- Kinesis Analytics: perform real-time analytics on streams using SQL
- Kinesis Firehose: load streams into S3, Redshift, ElasticSearch & Splunk
- Data Origin side: Streams Sink side: Firehose Processing: Analytics

### Kinesis Streams

- Default retention of 24 hours, can go upto 365 days
- Real-time processing with scale of throughput. Data Immutability
- ~200 ms for Classic and ~70 ms for enhanced fan-out
- Kinesis supports only **at-least once delivery** semantics
- One message can be as big as 1 MB
- Data is replicated asynchronously to three AZ
- Allows data storage, but customers must manage scaling (no auto-scaling)

#### Kinesis Data throughput Limits
##### Writes
- Writes: A single shard can ingest up to 1 MB of data per second (including partition keys) or 1,000 records per second (i.e. ingestion rate). 
- Similarly, if you scale your stream to 5,000 shards, the stream can ingest up to 5 GB per second or 5 million records per second. 
- If you need more ingest capacity, you can easily scale up the number of shards in the stream using the AWS Management Console or the UpdateShardCount API.
- ** Manual action is required to increase Shard Count**
- The maximum size of the data payload of a record before base64-encoding is up to 1 MB.

##### Reads
- Reads: GetRecords can retrieve up to 10 MB of data per call from a single shard, and up to 10,000 records per call. Each call to GetRecords is counted as one read transaction.
- Each shard can support up to five read transactions per second. Each read transaction can provide up to 10,000 records with an upper quota of 10 MB per transaction.
- Each shard can support up to a maximum total data read rate of 2 MB per second via GetRecords. If a call to GetRecords returns 10 MB, subsequent calls made within the next 5 seconds throw an exception.
- With on-demand capacity mode, Kinesis automatically manages the shards in order to provide the necessary throughput. It starts at 4 MB/s write and 8 MB/s read. It can go upto a maximum of 200 MB/s of write and 400 MB/s of read.
- By default, new data streams created with the on-demand capacity mode have 4 MB/s of 'write' and 8 MB/s of 'read' throughput. As the traffic increases, data streams with the on-demand capacity mode scale up to 200 MB/s of 'write' and 400 MB/s 'read' throughput. If you require additional throughput, contact AWS support.

##### Provisioned capacity mode:
- AWS customers must manage the number of shards based on the throughput required.
- Shards are charged at an hourly rate.
- Shared vs Enhanced Fan-out:
- By default, one shard supports a maximum of 2 MB/s. This is immaterial of the number of consumers, so they get into contention. Uses Pull over HTTP.
- With Enhanced Fan-out, each consumer gets an allotment of 2 MB/s (without any contention). This allows for parallel processing. Uses Push over HTTP/2.
- Partition Key: Partition key is used to group data into a shard. I.e. data records with the same partition key go into the same shard.

#### Amazon Kafka vs Kinesis Streams
- Both Apache Kafka and AWS Kinesis Data Streams are good choices for real-time data streaming platforms. 
- If you need to keep messages for more than 7 days with no limitation on message size per blob, Apache Kafka should be your choice. 
- However, Apache Kafka requires extra effort to set up, manage, and support.
- If your organization lacks Apache Kafka experts and/or human support, then choosing a fully-managed AWS Kinesis service will let you focus on the development. 
- Message retention: Kinesis allows only until a year. With Kafka, you can store as long as you like - if the use case warrants the same.
- Messaging semantics: Kinesis allows only at-least-once. Kafka allows both at-least-once and exactly-once message delivery semantics.
- Message Size: In Kinesis, a single message can be as big as 1 MB. In Kafka, max size is configurable.
- Registry:  Kafka has Kafka Schema Registry but Kinesis does not.


### Kinesis Data Firehose:

- The maximum size of a record sent to Kinesis Data Firehose, before base64-encoding, is 1,000 KiB. The PutRecordBatch operation can take up to 500 records per call or 4 MiB per call, whichever is smaller. This quota cannot be changed. 
- It can also batch, compress, and encrypt the data before loading it, minimizing the amount of storage used at the destination and increasing security.
- Near-real time (60 seconds latency for non batch records)
- Destinations: Splunk, MongoDB, Datadog, S3, Redshift (COPY through S3), Amazon ElasticSearch, HTTP Custom endpoint.
- Supports Serverless data transformations through Lambda
- **Automated scaling, but no data storage**

> Kinesis Data Firehose supports JSON to Parquet conversion, making it an ideal streaming option for S3. 
Note that it cannot convert CSV to Parquet automatically, so be careful with the question.

> If real-time flush from Kinesis Data Streams to S3 is needed, use Lambda

### Glue vs Firehose:

- Glue ETL provides an easy option to automatically generate ETL scripts and run the script as a scheduled job.  
- Glue ETL provisions required Spark infrastructure to run the job and automatically terminates the environment after the job is completed.
- Large scale ETL - go for Glue. For streaming solutions to sink into S3, use Data Firehose.

> A solution involving Kinesis Firehose requires an additional component to read data from S3 and add it firehose stream. 
For large files, you would also need to chunk into many messages when adding to the firehose.


### Kinesis Analytics
- Machine Learning on Kinesis Data Analytics is possible.
- SQL Function for Anomaly detection (Random Cut Forest)
- Hotspots: information about relatively dense regions in your data (collection of overheated servers)

#### Use Cases
- Streaming ETL: select columns, make simple transformations, on streaming data
- Continuous metric generation: live leaderboard for a mobile game
- Responsive analytics: look for certain criteria and build alerting (filtering)

### Data Pipeline vs DMS:
- Data Pipeline is more suitable for running scheduled tasks
- Data pipeline only supports JDBC Database, Amazon RDS and Amazon Redshift
- Data Pipeline does not support external SQL databases - DMS does
- To replicate a database and load on-going changes (i.e. CDC), use AWS DMS.

### AWS Batch
- Run batch jobs as Docker images
- No need to manage clusters, fully serverless
- Example: batch process of images, running thousands of concurrent jobs
- Schedule Batch Jobs using CloudWatch Events
- Orchestrate Batch Jobs using AWS Step Functions
- AWS Batch offers managed compute environment

#### Multi-Node mode

- Multi Node: large scale, good for HPC (high performance computing)
- Cannot use spot instances
- EC2 placement group Cluster recommended. Good for tightly coupled workloads.

> Batch does not have a time out & underlying can be EC2 or Fargate. Also, underlying storage is EBS while Lambda has temp storage limitations.


### EMR

- Supports Apache Spark, HBase, Presto, Flink
- Managed Hadoop Cluster
- Use cases: data processing, machine learning, web indexing, big data
- Compute Trade-Offs: One big cluster vs many smaller ones? Long running vs transient?
- Instance Configuration: Uniform Instance Groups (has auto-scaling) vs. Instance Fleet (no auto-scaling)

### Glue

- MapReduce and Apache Spark provide a protocol of data processing and node task distribution and management. 
- Amazon EMR: Managed Framework, but not Serverless
- AWS Glue: Serverless, Data Partitioning, compression
- Glue Learning Transforms: De-duplication of Data: FindMatches is a simple way to dedupe data. identify duplicate or matching records in your dataset, even when the records do not have a common unique identifier and no fields match exactly.
- Glue data catalog: Glue Crawler can crawl metadata from S3, RDS or DynamoDB and write metadata into Glue Data Catalog. It can then be discovered from Athena, Redshift spectrum or EMR
 
### Redshift

- It’s OLAP – online analytical processing (analytics and data warehousing
- **Columnar** storage of data
- Massively Parallel Query Execution (MPP), has SQL Interface
- BI tools such as AWS Quicksight or Tableau integrate with it
- Data is loaded from S3, Kinesis Firehose, DynamoDB, DMS
- Can provision multiple nodes, but it’s not Multi-AZ
- Backup & Restore, Security VPC / IAM / KMS, Monitoring
- Redshift Enhanced VPC Routing: COPY / UNLOAD goes through VPC
- Spectrum: Query data that is already in S3 without loading it
- Workload Management (WLM): Enables you to flexibly manage queries’ priorities within workload
- Redshift concurrency scaling uses WLM to offer automated additional cluster for scaling of queries

> Redshift is provisioned, so it’s worth it when you have a sustained usage
(use Athena if the queries are sporadic instead)

> You can configure Amazon Redshift to automatically copy snapshots 
(automated or manual) of a cluster to another AWS Region

### DocumentDB

- AWS implementation of MongoDB - **compliant with MongoDB API**

### QuickSight

- QuickSight allows you to directly connect to and import data from a wide variety of cloud and on-premises data sources. 
- These include SaaS applications such as Salesforce, Square, ServiceNow, Twitter, Github, and JIRA; 
- 3rd party databases such as Teradata, MySQL, Postgres, and SQL Server; 
- native AWS services such as Redshift, Athena, S3, RDS, and Aurora; and private VPC subnets. 
- You can also upload a variety of file types including Excel, CSV, JSON, and Presto.
- Role-based access control, active directory integration, CloudTrail auditing, single sign-on (IAM, 3rd party), private VPC subnets, and data backup. 
- QuickSight is also FedRamp, HIPAA, PCI PSS, ISO, and SOC compliant to help you meet any industry-specific or regulatory requirements.
- Pay per use pricing: Available in 30 minute increments or per reader per month charge for unlimited usage.

### Athena

- Serverless SQL queries on top of your data in S3, pay per query, output to S3
- Supports CSV, JSON, Parquet, ORC, etc…
- Queries are logged in CloudTrail (which can be chained with CloudWatch logs & used with metrics filters)




