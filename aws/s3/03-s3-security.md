## Amazon S3 – Security

### Amazon S3 – Object Encryption

You can encrypt objects in S3 buckets using one of 4 methods:
1. **Server-Side Encryption with S3-Managed Keys (SSE-S3)** – Enabled by default
2. **Server-Side Encryption with KMS Keys (SSE-KMS)** – Use AWS KMS to manage keys
3. **Server-Side Encryption with Customer-Provided Keys (SSE-C)** – You manage your own keys
4. **Client-Side Encryption** – Encrypt before uploading

### SSE-S3

- Encryption using keys handled, managed, and owned by AWS
- Object is encrypted server-side
- Encryption type is AES-256
- Must set header `"x-amz-server-side-encryption": "AES256"`
- Enabled by default for new buckets & new objects

### SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service)
- **KMS advantages:** user control + audit key usage using CloudTrail
- Object is encrypted server-side
- Must set header `"x-amz-server-side-encryption": "aws:kms"`

**SSE-KMS Limitation:**
- When you upload, it calls the `GenerateDataKey` KMS API
- When you download, it calls the `Decrypt` KMS API
- These count towards the KMS quota per second (5,500, 10,000, 30,000 req/s based on region)
- You can request a quota increase using the Service Quotas Console

### SSE-C

- Server-Side Encryption using keys fully managed by the customer outside of AWS
- Amazon S3 does **NOT** store the encryption key you provide
- **HTTPS must be used**
- Encryption key must be provided in HTTP headers for every HTTP request made

### Client-Side Encryption

- Use client libraries such as Amazon S3 Client-Side Encryption Library
- Clients must encrypt data themselves before sending to Amazon S3
- Clients must decrypt data themselves when retrieving from Amazon S3
- Customer fully manages the keys and encryption cycle

### Amazon S3 – Encryption in Transit (SSL/TLS)

- Amazon S3 exposes two endpoints:
  - **HTTP Endpoint** – not encrypted
  - **HTTPS Endpoint** – encryption in flight
- HTTPS is recommended
- HTTPS is **mandatory** for SSE-C

**Force Encryption in Transit:**
- Use a bucket policy with the condition `aws:SecureTransport: false` to deny HTTP requests
- `"Condition": {"Bool": {"aws:SecureTransport": false}}`

### Amazon S3 – Default Encryption vs. Bucket Policies

- SSE-S3 encryption is automatically applied to new objects stored in S3 buckets
- Optionally, you can "force encryption" using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)
- **Note:** Bucket Policies are evaluated **before** "Default Encryption"

### What is CORS?

- **Cross-Origin Resource Sharing (CORS)**
- **Origin** = scheme (protocol) + host (domain) + port
  - Example: `https://www.example.com` (implied port is 443 for HTTPS, 80 for HTTP)
- Web browser mechanism to allow requests to other origins while visiting the main origin
- **Same origin:** `http://example.com/app1` & `http://example.com/app2`
- **Different origins:** `http://www.example.com` & `http://other.example.com`
- Requests won’t be fulfilled unless the other origin allows for the requests, using CORS Headers (e.g., `Access-Control-Allow-Origin`)

**Amazon S3 – CORS:**
- If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers
- You can allow for a specific origin or for `*` (all origins)

### Amazon S3 – MFA Delete

- **MFA (Multi-Factor Authentication)** – forces users to generate a code on a device before doing important operations on S3
- **MFA will be required to:**
  - Permanently delete an object version
  - Suspend Versioning on the bucket
- **MFA won’t be required to:**
  - Enable Versioning
  - List deleted versions
- To use MFA Delete, **Versioning must be enabled** on the bucket
- Only the bucket owner (root account) can enable/disable MFA Delete

### S3 Access Logs

- For audit purposes, you may want to log all access to S3 buckets
- Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
- That data can be analyzed using data analysis tools
- The target logging bucket must be in the same AWS region
- Log format: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html

> **Warning:** Do NOT set your logging bucket to be the monitored bucket. This creates a logging loop, and your bucket will grow exponentially.

### Amazon S3 – Pre-Signed URLs

- Generate pre-signed URLs using the S3 Console, AWS CLI, or SDK
- **URL Expiration:**
  - S3 Console: 1 min up to 720 mins (12 hours)
  - AWS CLI: configure expiration with `--expires-in` parameter in seconds (default 3600 secs, max 604800 secs ≈ 168 hours)
- Users given a pre-signed URL inherit the permissions of the user that generated the URL for GET/PUT

**Examples:**
- Allow only logged-in users to download a premium video from your S3 bucket
- Allow an ever-changing list of users to download files by generating URLs dynamically
- Temporarily allow a user to upload a file to a precise location in your S3 bucket

### S3 Glacier Vault Lock

- Adopt a **WORM (Write Once Read Many)** model
- Create a Vault Lock Policy
- Lock the policy for future edits (can no longer be changed or deleted)
- Helpful for compliance and data retention

### S3 Object Lock (versioning must be enabled)

- Adopt a **WORM (Write Once Read Many)** model
- Block an object version deletion for a specified amount of time

| Mode | Description |
| --- | --- |
| Retention mode – Compliance | Object versions can’t be overwritten or deleted by ANY user, including root. Retention modes and periods can’t be changed or shortened. |
| Retention mode – Governance | Most users can’t overwrite, delete, or alter lock settings. Some users with special permissions can change retention or delete the object. |

- **Retention Period:** protect the object for a fixed period; it can be extended
- **Legal Hold:** protect the object indefinitely, independent from retention period; can be freely placed and removed using `s3:PutObjectLegalHold` IAM permission

### S3 – Access Points

- Access Points simplify security management for S3 Buckets
- Each Access Point has:
  - Its own DNS name (Internet Origin or VPC Origin)
  - An access point policy (similar to bucket policy) – manage security at scale

**S3 Access Points – VPC Origin:**
- Define the access point to be accessible only from within the VPC
- You must create a VPC Endpoint to access the Access Point (Gateway or Interface Endpoint)
- The VPC Endpoint Policy must allow access to the target bucket and Access Point

### S3 Object Lambda

- Use AWS Lambda Functions to change the object before it is retrieved by the caller application
- Only one S3 bucket is needed, on top of which we create S3 Access Points and S3 Object Lambda Access Points

**Use Cases:**
- Redacting personally identifiable information (PII) for analytics or non-production environments
- Converting across data formats (e.g., converting XML to JSON)
- Resizing and watermarking images on the fly using caller-specific details
