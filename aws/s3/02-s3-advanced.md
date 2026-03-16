## Amazon S3 – Advanced

### Amazon S3 – Moving Between Storage Classes

- You can transition objects between storage classes
- For infrequently accessed objects, move them to Standard-IA
- For archive objects that don’t need fast access, move them to Glacier or Glacier Deep Archive
- Moving objects can be automated using **Lifecycle Rules**

**Transition flow (can only move to lower/cheaper tiers):**
Standard → Standard-IA → Intelligent-Tiering → One-Zone-IA → Glacier Instant Retrieval → Glacier Flexible Retrieval → Glacier Deep Archive

### Amazon S3 – Lifecycle Rules

- **Transition Actions** – configure objects to transition to another storage class
  - Example: Move to Standard-IA 60 days after creation
  - Example: Move to Glacier for archiving after 6 months
- **Expiration Actions** – configure objects to expire (delete) after some time
  - Access log files can be set to delete after 365 days
  - Can delete old versions of files (if versioning is enabled)
  - Can delete incomplete Multi-Part uploads
- Rules can be created for a certain prefix (e.g., `s3://mybucket/mp3/*`)
- Rules can be created for certain object Tags (e.g., `Department: Finance`)

**Lifecycle Rules – Scenario 1:**
> Your application on EC2 creates image thumbnails after profile photos are uploaded to S3. These thumbnails can be easily recreated and only need to be kept for 60 days. The source images should be immediately retrievable for 60 days, then users can wait up to 6 hours.

- S3 source images on Standard, with lifecycle configuration to transition to Glacier after 60 days
- S3 thumbnails on One-Zone-IA, with lifecycle configuration to expire (delete) after 60 days

**Lifecycle Rules – Scenario 2:**
> Your company requires the ability to recover deleted S3 objects immediately for 30 days. After 30 days, and for up to 365 days, deleted objects should be recoverable within 48 hours.

- Enable S3 Versioning so "deleted objects" are hidden by a "delete marker" and can be recovered
- Transition "noncurrent versions" to Standard-IA, then to Glacier Deep Archive

### Amazon S3 Analytics – Storage Class Analysis

- Helps you decide when to transition objects to the right storage class
- Recommendations for Standard and Standard-IA only (does NOT work for One-Zone-IA or Glacier)
- Report is updated daily
- 24 to 48 hours to start seeing data analysis
- Good first step to put together Lifecycle Rules (or improve them)

### S3 – Requester Pays

- In general, bucket owners pay for all Amazon S3 storage and data transfer costs
- With **Requester Pays buckets**, the requester (instead of the bucket owner) pays the cost of the request and data download
- Helpful when you want to share large datasets with other accounts
- The requester must be authenticated in AWS (cannot be anonymous)

### S3 Event Notifications

- S3 events: `S3:ObjectCreated`, `S3:ObjectRemoved`, `S3:ObjectRestore`, `S3:Replication`, etc.
- Object name filtering possible (e.g., `*.jpg`)
- **Use case:** generate thumbnails of images uploaded to S3
- Can create as many "S3 events" as desired
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer

**Destinations:**
- SNS
- SQS
- Lambda Function
- Amazon EventBridge (with advanced filtering options, multiple destinations, reliable delivery)

**EventBridge advantages:**
- Advanced filtering options with JSON rules (metadata, object size, name, etc.)
- Multiple destinations – e.g., Step Functions, Kinesis Streams/Firehose
- EventBridge capabilities – Archive, Replay Events, Reliable delivery

### S3 – Baseline Performance

- Amazon S3 automatically scales to high request rates, latency 100–200 ms
- Achieve at least **3,500 PUT/COPY/POST/DELETE** or **5,500 GET/HEAD** requests per second per prefix in a bucket
- No limits to the number of prefixes in a bucket
- If you spread reads across 4 prefixes evenly, you can achieve 22,000 requests/second for GET and HEAD

### S3 Performance Optimizations

**Multi-Part Upload:**
- Recommended for files > 100 MB; must use for files > 5 GB
- Parallelizes uploads (speeds up transfers)

**S3 Transfer Acceleration:**
- Increases transfer speed by transferring file to an AWS edge location, which forwards data to the S3 bucket in the target region via the private AWS network
- Compatible with multi-part upload

**S3 Byte-Range Fetches:**
- Parallelize GETs by requesting specific byte ranges
- Better resilience in case of failures
- Can retrieve only partial data (e.g., just the header/first bytes of a file)

### S3 Batch Operations

- Perform bulk operations on existing S3 objects with a single request:
  - Modify object metadata & properties
  - Copy objects between S3 buckets
  - Encrypt un-encrypted objects
  - Modify ACLs, tags
  - Restore objects from S3 Glacier
  - Invoke Lambda function to perform custom action on each object
- A job consists of a list of objects, the action to perform, and optional parameters
- S3 Batch Operations manages retries, tracks progress, sends completion notifications, generates reports
- You can use **S3 Inventory** to get object lists and use **Athena** to query and filter

### S3 – Storage Lens

- Understand, analyze, and optimize storage across an entire AWS Organization
- Discover anomalies, identify cost efficiencies, and apply data protection best practices (30 days usage & activity metrics)
- Aggregate data for Organization, specific accounts, regions, buckets, or prefixes
- Default dashboard or create your own dashboards
- Can be configured to export metrics daily to an S3 bucket (CSV, Parquet)

**Storage Lens – Free Metrics:**
- Automatically available for all customers
- Contains around 28 usage metrics
- Data available for queries for 14 days

**Storage Lens – Advanced Metrics (paid):**
- Advanced Metrics: Activity, Advanced Cost Optimization, Advanced Data Protection, Status Code
- CloudWatch Publishing – access metrics in CloudWatch without additional charges
- Prefix Aggregation – collect metrics at the prefix level
- Data available for queries for 15 months

**Storage Lens Metric Categories:**

| Category | Key Metrics | Use Cases |
| --- | --- | --- |
| Summary Metrics | StorageBytes, ObjectCount | Identify fastest-growing (or unused) buckets and prefixes |
| Cost-Optimization Metrics | NonCurrentVersionStorageBytes, IncompleteMultipartUploadStorageBytes | Identify buckets with old incomplete multipart uploads; objects for lower-cost storage classes |
| Data-Protection Metrics | VersioningEnabledBucketCount, MFADeleteEnabledBucketCount, SSEKMSEnabledBucketCount, CrossRegionReplicationRuleCount | Identify buckets not following data-protection best practices |
| Access-management Metrics | ObjectOwnershipBucketOwnerEnforcedBucketCount | Identify Object Ownership settings |
| Event Metrics | EventNotificationEnabledBucketCount | Identify buckets with S3 Event Notifications configured |
| Performance Metrics | TransferAccelerationEnabledBucketCount | Identify buckets with S3 Transfer Acceleration enabled |
| Activity Metrics | AllRequests, GetRequests, PutRequests, ListRequests, BytesDownloaded | Understand how storage is requested |
| Detailed Status Code Metrics | 200OKStatusCount, 403ForbiddenErrorCount, 404NotFoundErrorCount | Insights for HTTP status codes |
