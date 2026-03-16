## Serverless Overview


### What’s Serverless?

- Serverless is a new paradigm in which developers don’t have to manage servers anymore
- They just deploy code / functions!
- Initially, Serverless == FaaS (Function as a Service)
- Serverless was pioneered by AWS Lambda but now also includes anything that’s managed: “databases, messaging, storage, etc.”
- Serverless does not mean there are no servers — it means you just don’t manage/provision/see them

### Serverless in AWS

- AWS Lambda
- DynamoDB
- AWS Cognito
- AWS API Gateway
- Amazon S3
- AWS SNS & SQS
- AWS Kinesis Data Firehose
- Aurora Serverless
- Step Functions
- Fargate

### Why AWS Lambda

| Amazon EC2 | Amazon Lambda |
| --- | --- |
| Virtual Servers in the Cloud | Virtual functions – no servers to manage! |
| Limited by RAM and CPU | Limited by time – short executions |
| Continuously running | Run on-demand |
| Scaling means intervention to add/remove servers | Scaling is automated! |

### Benefits of AWS Lambda

- Easy pricing:
  - Pay per request and compute time
  - Free tier of 1,000,000 Lambda requests and 400,000 GBs of compute time
- Integrated with the whole AWS suite of services
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per function (up to 10 GB of RAM!)
- Increasing RAM will also improve CPU and network!

### AWS Lambda Language Support

- Node.js (JavaScript)
- Python
- Java
- C# (.NET Core) / PowerShell
- Ruby
- Custom Runtime API (community supported, e.g., Rust, Golang)
- Lambda Container Image:
  - The container image must implement the Lambda Runtime API
  - ECS / Fargate is preferred for running arbitrary Docker images

### AWS Lambda Integrations (Main Ones)

API Gateway, Kinesis, DynamoDB, S3, CloudFront, CloudWatch Events / EventBridge, CloudWatch Logs, SNS, SQS, Cognito

### Example: Serverless Thumbnail Creation

- New image uploaded to S3 triggers AWS Lambda function
- Lambda creates a thumbnail and stores it in a new S3 bucket
- Lambda also stores metadata (image name, size, creation date, etc.) in DynamoDB

### Example: Serverless CRON Job

- CloudWatch Events / EventBridge rule fires every hour (or any schedule)
- Triggers an AWS Lambda function to perform a task

### AWS Lambda Pricing: Example

Full pricing info: https://aws.amazon.com/lambda/pricing/

- **Pay per calls:**
  - First 1,000,000 requests are free
  - $0.20 per 1 million requests thereafter ($0.0000002 per request)
- **Pay per duration** (in increments of 1 ms):
  - 400,000 GB-seconds of compute time per month for FREE
    - = 400,000 seconds if function is 1 GB RAM
    - = 3,200,000 seconds if function is 128 MB RAM
  - After that: $1.00 for 600,000 GB-seconds
- It is usually very cheap to run AWS Lambda

### AWS Lambda Limits to Know – Per Region

- **Execution:**
  - Memory allocation: 128 MB – 10 GB (1 MB increments)
  - Maximum execution time: 900 seconds (15 minutes)
  - Environment variables: 4 KB
  - Disk capacity in the function container (`/tmp`): 512 MB to 10 GB
  - Concurrency executions: 1000 (can be increased)
- **Deployment:**
  - Lambda function deployment size (compressed .zip): 50 MB
  - Size of uncompressed deployment (code + dependencies): 250 MB
  - Can use `/tmp` directory to load other files at startup
  - Size of environment variables: 4 KB

### Lambda Concurrency and Throttling

- Concurrency limit: up to 1000 concurrent executions
- Can set a “reserved concurrency” at the function level (= limit)
- Each invocation over the concurrency limit will trigger a “Throttle”
- **Throttle behavior:**
  - Synchronous invocation → return `ThrottleError - 429`
  - Asynchronous invocation → retry automatically, then go to DLQ
- If you need a higher limit, open a support ticket

### Lambda Concurrency Issue

- If you don’t reserve (= limit) concurrency, the following can happen:
  - Many users via Application Load Balancer → 1000 concurrent executions all consumed
  - Few users via API Gateway or SDK/CLI → THROTTLED
- Best practice: set reserved concurrency per function to prevent starvation

### Concurrency and Asynchronous Invocations

- If the function doesn’t have enough concurrency to process all events, additional requests are throttled
- For throttling errors (429) and system errors (500-series), Lambda returns the event to the queue and attempts to run the function again for up to 6 hours
- The retry interval increases exponentially from 1 second after the first attempt to a maximum of 5 minutes

### Cold Starts & Provisioned Concurrency

- **Cold Start:**
  - New instance → code is loaded and code outside the handler runs (init)
  - If the init is large (code, dependencies, SDK…) this process can take time
  - First request served by new instances has higher latency than the rest
- **Provisioned Concurrency:**
  - Concurrency is allocated before the function is invoked (in advance)
  - The cold start never happens and all invocations have low latency
  - Application Auto Scaling can manage concurrency (schedule or target utilization)
- Note: Cold starts in VPC have been dramatically reduced (Oct/Nov 2019)
  - https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/
- Reference: https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html

### Lambda SnapStart

- Improves Lambda functions performance up to 10x at no extra cost for Java, Python & .NET
- When enabled, function is invoked from a pre-initialized state (no function initialization from scratch)
- When you publish a new version:
  - Lambda initializes your function
  - Takes a snapshot of memory and disk state of the initialized function
  - Snapshot is cached for low-latency access

### Customization at the Edge

- Many modern applications execute some form of logic at the edge
- **Edge Function:** A code that you write and attach to CloudFront distributions; runs close to users to minimize latency
- CloudFront provides two types: **CloudFront Functions** & **Lambda@Edge**
- You don’t have to manage any servers; deployed globally
- Use case: customize CDN content
- Pay only for what you use; fully serverless

### CloudFront Functions & Lambda@Edge Use Cases

- Website Security and Privacy
- Dynamic Web Application at the Edge
- Search Engine Optimization (SEO)
- Intelligently Route Across Origins and Data Centers
- Bot Mitigation at the Edge
- Real-time Image Transformation
- A/B Testing
- User Authentication and Authorization
- User Prioritization
- User Tracking and Analytics

### CloudFront Functions

- Lightweight functions written in JavaScript
- For high-scale, latency-sensitive CDN customizations
- Sub-ms startup times, millions of requests/second
- Used to change Viewer requests and responses:
  - **Viewer Request:** after CloudFront receives a request from a viewer
  - **Viewer Response:** before CloudFront forwards the response to the viewer
- Native feature of CloudFront (manage code entirely within CloudFront)

### Lambda@Edge

- Lambda functions written in NodeJS or Python
- Scales to 1000s of requests/second
- Used to change CloudFront requests and responses:
  - **Viewer Request** – after CloudFront receives a request from a viewer
  - **Origin Request** – before CloudFront forwards the request to the origin
  - **Origin Response** – after CloudFront receives the response from the origin
  - **Viewer Response** – before CloudFront forwards the response to the viewer
- Author functions in one AWS Region (us-east-1); CloudFront replicates to its locations

### CloudFront Functions vs. Lambda@Edge

| Feature | CloudFront Functions | Lambda@Edge |
| --- | --- | --- |
| Runtime Support | JavaScript | Node.js, Python |
| # of Requests | Millions per second | Thousands per second |
| CloudFront Triggers | Viewer Request/Response | Viewer Request/Response, Origin Request/Response |
| Max. Execution Time | < 1 ms | 5 – 10 seconds |
| Max. Memory | 2 MB | 128 MB up to 10 GB |
| Total Package Size | 10 KB | 1 MB – 50 MB |
| Network Access, File System Access | No | Yes |
| Access to the Request Body | No | Yes |
| Pricing | Free tier available, 1/6th price of @Edge | No free tier; charged per request & duration |

### CloudFront Functions vs. Lambda@Edge – Use Cases

**CloudFront Functions:**
- Cache key normalization – transform request attributes (headers, cookies, query strings, URL) to create an optimal cache key
- Header manipulation – insert/modify/delete HTTP headers in the request or response
- URL rewrites or redirects
- Request authentication & authorization – create and validate user-generated tokens (e.g., JWT) to allow/deny requests

**Lambda@Edge:**
- Longer execution time (several ms)
- Adjustable CPU or memory
- Your code depends on 3rd party libraries (e.g., AWS SDK to access other AWS services)
- Network access to use external services for processing
- File system access or access to the body of HTTP requests

### Lambda by Default

- By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)
- Therefore, it cannot access resources in your VPC (RDS, ElastiCache, internal ELB…)
- DynamoDB and public AWS services work fine (they’re public)

### Lambda in VPC

- You must define the VPC ID, the Subnets, and the Security Groups
- Lambda will create an ENI (Elastic Network Interface) in your subnets
- Lambda function → ENI in private subnet → connects to RDS (with proper security group rules)

### Lambda with RDS Proxy

- If Lambda functions directly access your database, they may open too many connections under high load
- **RDS Proxy benefits:**
  - Improve scalability by pooling and sharing DB connections
  - Improve availability by reducing by 66% the failover time and preserving connections
  - Improve security by enforcing IAM authentication and storing credentials in Secrets Manager
- The Lambda function must be deployed in your VPC, because RDS Proxy is never publicly accessible

### Invoking Lambda from RDS & Aurora

- Invoke Lambda functions from within your DB instance
- Allows you to process data events from within a database (e.g., INSERT triggers email via SES)
- Supported for **RDS for PostgreSQL** and **Aurora MySQL**
- Must allow outbound traffic to your Lambda function from the DB instance (Public, NAT GW, VPC Endpoints)
- DB instance must have the required permissions to invoke the Lambda function (Lambda Resource-based Policy & IAM Policy)

### RDS Event Notifications

- Notifications about the DB instance itself (created, stopped, started, …)
- You don’t have any information about the data itself
- Subscribe to event categories: DB instance, DB snapshot, DB Parameter Group, DB Security Group, RDS Proxy, Custom Engine Version
- Near real-time events (up to 5 minutes)
- Send notifications to SNS or subscribe to events using EventBridge → Lambda

### Amazon DynamoDB

- Fully managed, highly available with replication across multiple AZs
- NoSQL database – not a relational database – with transaction support
- Scales to massive workloads, distributed database
- Millions of requests per second, trillions of rows, 100s of TB of storage
- Fast and consistent performance (single-digit millisecond)
- Integrated with IAM for security, authorization, and administration
- Low cost and auto-scaling capabilities
- No maintenance or patching, always available
- Standard & Infrequent Access (IA) Table Class

### DynamoDB – Basics

- DynamoDB is made of Tables
- Each table has a **Primary Key** (must be decided at creation time)
- Each table can have an infinite number of items (= rows)
- Each item has attributes (can be added over time – can be null)
- Maximum size of an item is **400 KB**
- Data types supported:
  - Scalar Types – String, Number, Binary, Boolean, Null
  - Document Types – List, Map
  - Set Types – String Set, Number Set, Binary Set
- In DynamoDB you can rapidly evolve schemas

### DynamoDB – Table Example

| Partition Key (User_ID) | Sort Key (Game_ID) | Score | Result |
| --- | --- | --- | --- |
| 7791a3d6-… | 4421 | 92 | Win |
| 873e0634-… | 1894 | 14 | Lose |
| 873e0634-… | 4521 | 77 | Win |

### DynamoDB – Read/Write Capacity Modes

- **Provisioned Mode (default):**
  - You specify the number of reads/writes per second
  - You need to plan capacity beforehand
  - Pay for provisioned RCU (Read Capacity Units) & WCU (Write Capacity Units)
  - Possibility to add auto-scaling mode for RCU & WCU
- **On-Demand Mode:**
  - Read/writes automatically scale up/down with workloads
  - No capacity planning needed
  - Pay for what you use, more expensive ($$$)
  - Great for unpredictable workloads, steep sudden spikes

### DynamoDB Accelerator (DAX)

- Fully-managed, highly available, seamless in-memory cache for DynamoDB
- Helps solve read congestion by caching
- Microseconds latency for cached data
- Doesn’t require application logic modification (compatible with existing DynamoDB APIs)
- 5 minutes TTL for cache (default)

### DynamoDB Accelerator (DAX) vs. ElastiCache

- **DAX:** Ideal for individual object caching, Query & Scan cache (in front of DynamoDB)
- **ElastiCache:** Ideal for storing aggregation results (computed queries stored separately)

### DynamoDB – Stream Processing

- Ordered stream of item-level modifications (create/update/delete) in a table
- **Use cases:**
  - React to changes in real-time (e.g., welcome email to new users)
  - Real-time usage analytics
  - Insert into derivative tables
  - Implement cross-region replication
  - Invoke AWS Lambda on changes to your DynamoDB table

| Feature | DynamoDB Streams | Kinesis Data Streams (newer) |
| --- | --- | --- |
| Retention | 24 hours | 1 year |
| Consumers | Limited # | High # |
| Processing | AWS Lambda Triggers, DynamoDB Stream Kinesis adapter | AWS Lambda, Kinesis Data Analytics, Kinesis Data Firehose, AWS Glue Streaming ETL |

**DynamoDB Streams architectures:**
- DynamoDB Table → DynamoDB Streams → Lambda → SNS (messaging, notifications)
- DynamoDB Table → Kinesis Data Streams → Kinesis Data Firehose → S3 / Redshift / OpenSearch (analytics, indexing, archiving)

### DynamoDB Global Tables

- Make a DynamoDB table accessible with low latency in multiple regions
- **Active-Active** replication
- Applications can READ and WRITE to the table in any region
- Must enable DynamoDB Streams as a pre-requisite

### DynamoDB – Time To Live (TTL)

- Automatically delete items after an expiry timestamp
- **Use cases:** reduce stored data by keeping only current items, adhere to regulatory obligations, web session handling
- TTL is an epoch timestamp stored as an attribute in each item
- Items with TTL in the past are scanned and deleted automatically

### DynamoDB – Backups for Disaster Recovery

- **Continuous backups using point-in-time recovery (PITR):**
  - Optionally enabled for the last 35 days
  - Point-in-time recovery to any time within the backup window
  - Recovery process creates a new table
- **On-demand backups:**
  - Full backups for long-term retention, until explicitly deleted
  - Doesn’t affect performance or latency
  - Can be configured and managed in AWS Backup (enables cross-region copy)
  - Recovery process creates a new table

### DynamoDB – Integration with Amazon S3

- **Export to S3** (must enable PITR):
  - Works for any point of time in the last 35 days
  - Doesn’t affect the read capacity of your table
  - Perform data analysis on top of DynamoDB export query
  - Retain snapshots for auditing
  - ETL on top of S3 data before importing back into DynamoDB
  - Export in DynamoDB JSON or ION format
- **Import from S3:**
  - Import CSV, DynamoDB JSON, or ION format
  - Doesn’t consume any write capacity
  - Creates a new table
  - Import errors are logged in CloudWatch Logs

### Example: Building a Serverless API

`Client` → `API Gateway` (REST API) → `Lambda` (PROXY REQUESTS) → `DynamoDB` (CRUD)

### AWS API Gateway

- AWS Lambda + API Gateway: No infrastructure to manage
- Support for the WebSocket Protocol
- Handle API versioning (v1, v2…)
- Handle different environments (dev, test, prod…)
- Handle security (Authentication and Authorization)
- Create API keys, handle request throttling
- Swagger / Open API import to quickly define APIs
- Transform and validate requests and responses
- Generate SDK and API specifications
- Cache API responses

### API Gateway – Integrations High Level

- **Lambda Function:** Invoke Lambda function; easy way to expose REST API backed by AWS Lambda
- **HTTP:** Expose HTTP endpoints in the backend (e.g., internal HTTP API on-premises, ALB); add rate limiting, caching, user authentication, API keys
- **AWS Service:** Expose any AWS API through the API Gateway (e.g., start Step Function workflow, post to SQS); add authentication, deploy publicly, rate control

### API Gateway – Endpoint Types

- **Edge-Optimized (default):** For global clients; requests are routed through CloudFront Edge locations (improves latency); API Gateway still lives in only one region
- **Regional:** For clients within the same region; can manually combine with CloudFront for more control
- **Private:** Can only be accessed from your VPC using an interface VPC endpoint (ENI); use a resource policy to define access

### API Gateway – Security

- **User Authentication through:**
  - IAM Roles (useful for internal applications)
  - Cognito (identity for external users, e.g., mobile users)
  - Custom Authorizer (your own logic)
- **Custom Domain Name HTTPS** security through integration with AWS Certificate Manager (ACM):
  - Edge-Optimized endpoint: certificate must be in `us-east-1`
  - Regional endpoint: certificate must be in the API Gateway region
  - Must set up CNAME or A-alias record in Route 53

### AWS Step Functions

- Build serverless visual workflow to orchestrate your Lambda functions
- Features: sequence, parallel, conditions, timeouts, error handling…
- Can integrate with EC2, ECS, on-premises servers, API Gateway, SQS queues, etc.
- Possibility of implementing human approval feature
- **Use cases:** order fulfillment, data processing, web applications, any workflow

### Amazon Cognito

- Give users an identity to interact with our web or mobile application
- **Cognito User Pools (CUP):**
  - Sign-in functionality for app users
  - Integrate with API Gateway & Application Load Balancer
- **Cognito Identity Pools (Federated Identity):**
  - Provide AWS credentials to users so they can access AWS resources directly
  - Integrate with Cognito User Pools as an identity provider
- Cognito vs IAM: “hundreds of users”, “mobile users”, “authenticate with SAML” → use Cognito

### Cognito User Pools (CUP) – User Features

- Create a serverless database of users for your web & mobile apps
- Simple login: Username (or email) / password combination
- Password reset
- Email & Phone Number Verification
- Multi-factor authentication (MFA)
- Federated Identities: users from Facebook, Google, SAML…

### Cognito User Pools (CUP) – Integrations

- CUP integrates with API Gateway and Application Load Balancer:
  - User authenticates → retrieves token → passes token to API Gateway / ALB
  - API Gateway evaluates the Cognito token and routes to backend
  - ALB evaluates the Cognito token and routes to Target Group

### Cognito Identity Pools (Federated Identities)

- Get identities for “users” so they obtain temporary AWS credentials
- Users source can be Cognito User Pools, 3rd party logins, etc.
- Users can then access AWS services directly or through API Gateway
- The IAM policies applied to the credentials are defined in Cognito
- They can be customized based on the `user_id` for fine-grained control
- Default IAM roles for authenticated and guest users
- **Example:** Web/mobile app → Social Identity Provider (login) → exchange token with Cognito Identity Pools → get temporary AWS credentials → directly access private S3 bucket or DynamoDB table
- Supports Row Level Security in DynamoDB
