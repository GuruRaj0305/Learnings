## Data & Analytics


### Amazon Athena

- Serverless query service to analyze data stored in Amazon S3
- Uses standard SQL language to query the files (built on Presto)
- Supports CSV, JSON, ORC, Avro, and Parquet
- Pricing: $5.00 per TB of data scanned
- Commonly used with Amazon QuickSight for reporting/dashboards
- **Use cases:** Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc.
- **Exam Tip:** analyze data in S3 using serverless SQL → use Athena

### Amazon Athena – Performance Improvement

- Use columnar data for cost savings (less scan):
  - Apache Parquet or ORC is recommended
  - Huge performance improvement
  - Use Glue to convert your data to Parquet or ORC
- Compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd…)
- Partition datasets in S3 for easy querying on virtual columns:
  ```
  s3://yourBucket/pathToTable/<PARTITION_COLUMN_NAME>=<VALUE>/...
  ```
  - Example: `s3://athena-examples/flight/parquet/year=1991/month=1/day=1/`
- Use larger files (> 128 MB) to minimize overhead

### Amazon Athena – Federated Query

- Allows you to run SQL queries across data stored in relational, non-relational, object, and custom data sources (AWS or on-premises)
- Uses Data Source Connectors that run on AWS Lambda to run Federated Queries (e.g., CloudWatch Logs, DynamoDB, RDS…)
- Supported sources: ElastiCache, DocumentDB, DynamoDB, Aurora, Redshift, RDS, MySQL, SQL Server, HBase in EMR, on-premises databases
- Store the results back in Amazon S3

### Redshift Overview

- Redshift is based on PostgreSQL, but it’s NOT used for OLTP
- It’s **OLAP** – Online Analytical Processing (analytics and data warehousing)
- 10x better performance than other data warehouses; scales to PBs of data
- Columnar storage of data (instead of row-based) & parallel query engine
- Two modes: Provisioned cluster or Serverless cluster
- Has a SQL interface for performing queries
- BI tools such as Amazon QuickSight or Tableau integrate with it
- vs Athena: faster queries / joins / aggregations thanks to indexes

### Redshift Cluster

- **Leader node:** For query planning and results aggregation
- **Compute nodes:** For performing the queries; send results to leader
- **Provisioned mode:** Choose instance types in advance; can reserve instances for cost savings

### Redshift – Snapshots & DR

- Redshift has “Multi-AZ” mode for some clusters
- Snapshots are point-in-time backups of a cluster, stored internally in S3
- Snapshots are incremental (only what has changed is saved)
- You can restore a snapshot into a new cluster
- **Automated:** every 8 hours, every 5 GB, or on a schedule; retention between 1 to 35 days
- **Manual:** snapshot retained until you delete it
- You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region

### Loading Data into Redshift

- Large inserts are **MUCH** better using `COPY` command
- **Loading methods:**
  - Amazon Kinesis Data Firehose → S3 → Redshift (via COPY)
  - S3 → Redshift (via COPY with IAM role)
  - EC2 Instance → JDBC driver → Redshift (better to write data in batches)
- **Enhanced VPC Routing:** routes traffic through VPC (more secure); without it, traffic goes through the internet
- Example COPY command:
  ```sql
  COPY customer FROM 's3://mybucket/mydata'
  IAM_ROLE 'arn:aws:iam::0123456789012:role/MyRedshiftRole';
  ```

### Redshift Spectrum

- Query data that is already in S3 without loading it into Redshift
- Must have a Redshift cluster available to start the query
- The query is then submitted to thousands of Redshift Spectrum nodes

### Amazon OpenSearch Service

- Amazon OpenSearch is the successor to Amazon ElasticSearch
- In DynamoDB, queries only exist by primary key or indexes
- With OpenSearch, you can **search any field, even partial matches**
- It’s common to use OpenSearch as a complement to another database
- Two modes: managed cluster or serverless cluster
- Does not natively support SQL (can be enabled via a plugin)
- Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
- Security through Cognito & IAM, KMS encryption, TLS
- Comes with OpenSearch Dashboards (visualization)

### OpenSearch Patterns

**Pattern 1: DynamoDB + OpenSearch**
- DynamoDB Table (CRUD) → DynamoDB Stream → Lambda → Amazon OpenSearch
- API to retrieve items from DynamoDB; API to search items from OpenSearch

**Pattern 2: CloudWatch Logs + OpenSearch**
- CloudWatch Logs → Subscription Filter → Lambda (managed by AWS) → OpenSearch (real-time)
- CloudWatch Logs → Subscription Filter → Kinesis Data Firehose → OpenSearch (near real-time)

**Pattern 3: Kinesis Data Streams + OpenSearch**
- Kinesis Data Streams → Lambda (real-time) → Amazon OpenSearch
- Kinesis Data Streams → Kinesis Data Firehose + Lambda (data transformation, near real-time) → Amazon OpenSearch

### Amazon EMR

- EMR stands for “Elastic MapReduce”
- EMR helps creating Hadoop clusters (Big Data) to analyze and process vast amounts of data
- The clusters can be made of hundreds of EC2 instances
- EMR comes bundled with Apache Spark, HBase, Presto, Flink…
- EMR takes care of all the provisioning and configuration
- Auto-scaling and integrated with Spot instances
- **Use cases:** data processing, machine learning, web indexing, big data…

### Amazon EMR – Node Types & Purchasing

| Node Type | Purpose | Run Time |
| --- | --- | --- |
| Master Node | Manage the cluster, coordinate, manage health | Long running |
| Core Node | Run tasks and store data | Long running |
| Task Node (optional) | Just to run tasks – usually Spot | Usually Spot |

**Purchasing options:**
- **On-demand:** Reliable, predictable, won’t be terminated
- **Reserved (min 1 year):** Cost savings (EMR uses automatically if available)
- **Spot Instances:** Cheaper, can be terminated, less reliable
- Can have long-running cluster or transient (temporary) cluster

### Amazon QuickSight

- Serverless machine learning-powered business intelligence service to create interactive dashboards
- Fast, automatically scalable, embeddable, with per-session pricing
- **Use cases:** Business analytics, building visualizations, ad-hoc analysis, business insights
- Integrated with RDS, Aurora, Athena, Redshift, S3…
- In-memory computation using SPICE engine if data is imported into QuickSight
- Enterprise edition: Possibility to set up Column-Level Security (CLS)
- Reference: https://aws.amazon.com/quicksight/

### QuickSight Integrations

- **AWS Services:** RDS, Aurora, Redshift, Athena, S3, OpenSearch, Timestream
- **SaaS:** On-premises databases (JDBC)
- **Imports:** ELF & CLF (Log Format)

### QuickSight – Dashboard & Analysis

- Define Users (standard version) and Groups (enterprise version)
- These users & groups only exist within QuickSight, not IAM!
- A **dashboard:**
  - Is a read-only snapshot of an analysis that you can share
  - Preserves the configuration of the analysis (filtering, parameters, controls, sort)
- You can share the analysis or the dashboard with Users or Groups
- To share a dashboard, you must first publish it
- Users who see the dashboard can also see the underlying data

### AWS Glue

- Managed extract, transform, and load (ETL) service
- Useful to prepare and transform data for analytics
- Fully serverless service
- **Flow:** S3 Bucket / Amazon RDS (Extract) → Glue ETL (Transform) → Redshift Data Warehouse (Load)

### AWS Glue – Convert Data into Parquet Format

- S3 Bucket (CSV input) → Glue ETL Job → S3 Bucket (Parquet output) → Amazon Athena (Analyze)
- Trigger: Lambda Function (on S3 PUT event notification), or EventBridge (as an alternative)

### Glue Data Catalog

- AWS Glue Data Crawler: discovers data in Amazon S3, Amazon RDS, Amazon DynamoDB, JDBC sources
- Writes metadata to AWS Glue Data Catalog (database + tables with metadata)
- **Catalog is used by:** Amazon Athena, Amazon Redshift Spectrum, Amazon EMR, Glue Jobs (ETL)

### Glue – Things to Know

- **Glue Job Bookmarks:** prevent re-processing old data
- **Glue DataBrew:** clean and normalize data using pre-built transformations
- **Glue Studio:** new GUI to create, run, and monitor ETL jobs in Glue
- **Glue Streaming ETL** (built on Apache Spark Structured Streaming): compatible with Kinesis Data Streaming, Kafka, MSK (managed Kafka)

### AWS Lake Formation

- **Data lake** = central place to have all your data for analytics purposes
- Fully managed service that makes it easy to set up a data lake in days
- Discover, cleanse, transform, and ingest data into your Data Lake
- Automates many complex manual steps (collecting, cleansing, moving, cataloging data…) and de-duplicates using ML Transforms
- Combine structured and unstructured data in the data lake
- Out-of-the-box source blueprints: S3, RDS, Relational & NoSQL DB…
- **Fine-grained Access Control** for applications (row and column-level)
- Built on top of AWS Glue

**Architecture:**
- Data Sources (S3, RDS, Aurora, on-premises SQL & NoSQL) → Source Crawlers + ETL and Data Prep → Data Catalog + Security Settings → Data Lake (stored in S3)
- Consumers: Athena, Redshift, users

**Centralized Permissions Example:**
- Column-level security enforced centrally by Lake Formation
- Users access data through QuickSight or Athena; access controlled by Lake Formation Access Control (not directly on S3)

### Amazon Managed Service for Apache Flink

- Previously named: Kinesis Data Analytics for Apache Flink
- Flink (Java, Scala, or SQL) is a framework for processing data streams
- Sources: Kinesis Data Streams, Amazon MSK (Apache Kafka)
- Run any Apache Flink application on a managed cluster on AWS
- Provisioned compute resources, parallel computation, automatic scaling
- Application backups (implemented as checkpoints and snapshots)
- Use any Apache Flink programming features to transform data
- **Important:** Flink does **not** read from Amazon Data Firehose

### Amazon Managed Streaming for Apache Kafka (Amazon MSK)

- Alternative to Amazon Kinesis
- Fully managed Apache Kafka on AWS
- Allow you to create, update, delete clusters
- MSK creates & manages Kafka broker nodes & Zookeeper nodes for you
- Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
- Automatic recovery from common Apache Kafka failures
- Data is stored on EBS volumes for as long as you want
- **MSK Serverless:** Run Apache Kafka on MSK without managing capacity; MSK automatically provisions resources and scales compute & storage

### Kinesis Data Streams vs. Amazon MSK

| Feature | Kinesis Data Streams | Amazon MSK |
| --- | --- | --- |
| Message size | 1 MB limit | 1 MB default; configurable (e.g., 10 MB) |
| Partitioning | Shards | Kafka Topics with Partitions |
| Scaling | Shard Splitting & Merging | Can only add partitions to a topic |
| Encryption in-flight | TLS | PLAINTEXT or TLS |
| Encryption at-rest | KMS | KMS |

### Amazon MSK Consumers

- Kinesis Data Analytics for Apache Flink
- AWS Glue Streaming ETL Jobs (powered by Apache Spark Streaming)
- Lambda
- Amazon EC2, ECS, EKS applications

### Big Data Ingestion Pipeline

- Fully serverless ingestion pipeline
- **Requirements:**
  - Collect data in real time
  - Transform the data
  - Query the transformed data using SQL
  - Reports stored in S3
  - Load into a warehouse and create dashboards

**Pipeline Architecture:**
1. IoT Devices → Amazon Kinesis Data Streams (real-time ingestion)
2. Kinesis Data Streams → Amazon Kinesis Data Firehose (delivery, every 1 minute)
3. Kinesis Data Firehose → Lambda (data transformation, optional) → Amazon S3 (ingestion bucket)
4. Amazon S3 (ingestion bucket) → SQS (trigger, optional) → Lambda → Amazon Athena (SQL query)
5. Amazon Athena → Amazon S3 (reporting bucket)
6. Amazon S3 (reporting bucket) → Amazon QuickSight, Amazon Redshift (dashboards)

**Discussion:**
- IoT Core allows you to harvest data from IoT devices
- Kinesis is great for real-time data collection
- Firehose helps with data delivery to S3 in near real-time (1 minute)
- Lambda can help Firehose with data transformations
- Amazon S3 can trigger notifications to SQS
- Lambda can subscribe to SQS (or S3 can connect directly to Lambda)
- Athena is a serverless SQL service and results are stored in S3
- The reporting bucket can be used by reporting tools such as QuickSight, Redshift, etc.
