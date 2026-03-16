## Amazon S3

### Amazon S3 Overview

- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as "infinitely scaling" storage
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration

**Use cases:**
- Backup and storage
- Disaster Recovery
- Archive
- Hybrid Cloud storage
- Application hosting
- Media hosting
- Data lakes & big data analytics
- Software delivery
- Static website

### Amazon S3 – Buckets

- Amazon S3 allows people to store objects (files) in "buckets" (directories)
- Buckets must have a **globally unique name** (across all regions and all accounts)
- Buckets are defined at the region level
- S3 looks like a global service but buckets are created in a specific region

**Bucket Naming Convention:**
- No uppercase, no underscore
- 3–63 characters long
- Not an IP address
- Must start with lowercase letter or number
- Must NOT start with the prefix `xn--`
- Must NOT end with the suffix `-s3alias`

### Amazon S3 – Objects

- Objects (files) have a **Key**
- The key is the **FULL path**, e.g.:
  - `s3://my-bucket/my_file.txt`
  - `s3://my-bucket/my_folder1/another_folder/my_file.txt`
- The key is composed of `prefix + object name`
- There’s no concept of "directories" within buckets (just keys with slashes)

**Object properties:**
- **Max Object Size:** 5 TB (5000 GB)
- If uploading more than 5 GB, must use "multi-part upload"
- **Metadata:** list of text key/value pairs – system or user metadata
- **Tags:** Unicode key/value pair – up to 10 – useful for security/lifecycle
- **Version ID:** if versioning is enabled

### Amazon S3 – Security

- **User-Based:** IAM Policies – which API calls should be allowed for a specific IAM user
- **Resource-Based:**
  - **Bucket Policies** – bucket-wide rules from the S3 console; allows cross-account
  - **Object Access Control List (ACL)** – finer grain (can be disabled)
  - **Bucket Access Control List (ACL)** – less common (can be disabled)
- **Note:** An IAM principal can access an S3 object if:
  - The user IAM permissions ALLOW it OR the resource policy ALLOWS it
  - AND there’s no explicit DENY
- **Encryption:** encrypt objects in Amazon S3 using encryption keys

### S3 Bucket Policies

- JSON-based policies with:
  - **Resources:** buckets and objects
  - **Effect:** Allow / Deny
  - **Actions:** Set of API calls to Allow or Deny
  - **Principal:** The account or user to apply the policy to
- **Use S3 bucket policy to:**
  - Grant public access to the bucket
  - Force objects to be encrypted at upload
  - Grant access to another account (Cross-Account)

### Bucket Settings for Block Public Access

- These settings were created to prevent company data leaks
- If you know your bucket should never be public, leave these on
- Can be set at the account level

### Amazon S3 – Static Website Hosting

- S3 can host static websites and have them accessible on the Internet
- The website URL formats (depending on the region):
  - `http://bucket-name.s3-website-aws-region.amazonaws.com`
  - `http://bucket-name.s3-website.aws-region.amazonaws.com`
- If you get a 403 Forbidden error, make sure the bucket policy allows public reads!

### Amazon S3 – Versioning

- You can version your files in Amazon S3, enabled at the bucket level
- Same key overwrite will change the "version": 1, 2, 3…
- **Best practice:** version your buckets
  - Protect against unintended deletes (ability to restore a version)
  - Easy roll back to previous version
- **Notes:**
  - Any file that is not versioned prior to enabling versioning will have version `null`
  - Suspending versioning does not delete the previous versions

### Amazon S3 – Replication (CRR & SRR)

- Must enable **Versioning** in both source and destination buckets
- **Cross-Region Replication (CRR):** compliance, lower latency access, replication across accounts
- **Same-Region Replication (SRR):** log aggregation, live replication between production and test accounts
- Buckets can be in different AWS accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3

**Replication – Notes:**
- After you enable Replication, only new objects are replicated
- Optionally, replicate existing objects using **S3 Batch Replication** (also replicates objects that failed replication)
- For DELETE operations:
  - Can replicate delete markers from source to target (optional setting)
  - Deletions with a version ID are NOT replicated (to avoid malicious deletes)
- There is no "chaining" of replication (bucket 1 → bucket 2, bucket 2 → bucket 3: objects from 1 are NOT replicated to 3)

### S3 Storage Classes

| Storage Class | Availability | Availability Zones | Min. Storage Duration | Min. Object Size | Retrieval Fee |
| --- | --- | --- | --- | --- | --- |
| S3 Standard – General Purpose | 99.99% | ≥ 3 | None | None | None |
| S3 Intelligent-Tiering | 99.9% | ≥ 3 | None | None | None |
| S3 Standard-IA | 99.9% | ≥ 3 | 30 days | 128 KB | Per GB retrieved |
| S3 One Zone-IA | 99.5% | 1 | 30 days | 128 KB | Per GB retrieved |
| S3 Glacier Instant Retrieval | 99.9% | ≥ 3 | 90 days | 128 KB | Per GB retrieved |
| S3 Glacier Flexible Retrieval | 99.99% | ≥ 3 | 90 days | 40 KB | Per GB retrieved |
| S3 Glacier Deep Archive | 99.99% | ≥ 3 | 180 days | 40 KB | Per GB retrieved |

**All classes:** Durability = 99.999999999% (11 9’s)

### S3 Standard – General Purpose

- 99.99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- **Use Cases:** Big Data analytics, mobile & gaming applications, content distribution

### S3 Storage Classes – Infrequent Access

For data that is less frequently accessed but requires rapid access when needed. Lower cost than S3 Standard.

- **S3 Standard-IA:** 99.9% Availability. Use cases: Disaster Recovery, backups
- **S3 One Zone-IA:** High durability in a single AZ (data lost when AZ is destroyed). 99.5% Availability. Use cases: Storing secondary backup copies of on-premises data, or recreatable data

### Amazon S3 Glacier Storage Classes

- Low-cost object storage for archiving/backup
- Pricing: price for storage + object retrieval cost

| Class | Retrieval Time | Min. Duration |
| --- | --- | --- |
| Glacier Instant Retrieval | Milliseconds (great for data accessed once a quarter) | 90 days |
| Glacier Flexible Retrieval | Expedited (1–5 min), Standard (3–5 hrs), Bulk (5–12 hrs – free) | 90 days |
| Glacier Deep Archive | Standard (12 hrs), Bulk (48 hrs) | 180 days |

### S3 Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- No retrieval charges

| Tier | Transition |
| --- | --- |
| Frequent Access (automatic) | Default tier |
| Infrequent Access (automatic) | Objects not accessed for 30 days |
| Archive Instant Access (automatic) | Objects not accessed for 90 days |
| Archive Access (optional) | Configurable from 90 to 700+ days |
| Deep Archive Access (optional) | Configurable from 180 to 700+ days |

### S3 Express One Zone

- High performance, single Availability Zone storage class
- Objects stored in a Directory Bucket (bucket in a single AZ)
- Handle 100,000s of requests per second with single-digit millisecond latency
- Up to 10x better performance than S3 Standard, 50% lower costs
- High Durability (99.999999999%) and Availability (99.95%)
- Co-locate your storage and compute resources in the same AZ (reduces latency)
- **Use cases:** latency-sensitive apps, data-intensive apps, AI & ML training, financial modeling, media processing, HPC
- Best integrated with SageMaker Model Training, Athena, EMR, Glue
