## RDS, Aurora & ElastiCache

### Amazon RDS Overview

- RDS stands for Relational Database Service
- It’s a managed DB service for databases using SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS
- Supported engines: Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server, IBM DB2, Aurora (AWS Proprietary)

### Advantages of RDS over Deploying DB on EC2

RDS is a managed service providing:
- Automated provisioning, OS patching
- Continuous backups and restore to specific timestamp (Point in Time Restore)
- Monitoring dashboards
- Read replicas for improved read performance
- Multi-AZ setup for Disaster Recovery (DR)
- Maintenance windows for upgrades
- Scaling capability (vertical and horizontal)
- Storage backed by EBS
- **Note:** You can’t SSH into your instances

### RDS – Storage Auto Scaling

- Helps you increase storage on your RDS DB instance dynamically
- When RDS detects you are running out of free database storage, it scales automatically
- Avoid manually scaling your database storage
- You must set a Maximum Storage Threshold (maximum limit for DB storage)
- Automatically modifies storage if:
  - Free storage is less than 10% of allocated storage
  - Low-storage lasts at least 5 minutes
  - 6 hours have passed since last modification
- Useful for applications with unpredictable workloads
- Supports all RDS database engines

### RDS Read Replicas for Read Scalability

- Up to 15 Read Replicas
- Within AZ, Cross AZ, or Cross Region
- Replication is **ASYNC**, so reads are eventually consistent
- Replicas can be promoted to their own standalone DB
- Applications must update the connection string to leverage read replicas

**RDS Read Replicas – Use Cases:**
- You have a production database taking on normal load
- You want to run a reporting application to run some analytics
- Create a Read Replica to run the new workload without affecting the production application
- Read replicas are used for SELECT (read-only) statements — not INSERT, UPDATE, DELETE

**RDS Read Replicas – Network Cost:**
- In AWS there’s a network cost when data goes from one AZ to another
- For RDS Read Replicas **within the same region**, you don’t pay that fee
- Cross-region replication incurs additional charges

### RDS Multi-AZ (Disaster Recovery)

- **SYNC** replication
- One DNS name – automatic app failover to standby
- Increases availability
- Failover in case of loss of AZ, loss of network, instance or storage failure
- No manual intervention in apps
- Not used for scaling

> **Note:** Read Replicas can be set up as Multi-AZ for Disaster Recovery (DR)

### RDS – From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB)
- Just click on "modify" for the database
- What happens internally:
  1. A snapshot is taken
  2. A new DB is restored from the snapshot in a new AZ
  3. Synchronization is established between the two databases

### RDS Custom

- Managed Oracle and Microsoft SQL Server Database with OS and database customization
- **RDS:** Automates setup, operation, and scaling of database in AWS
- **Custom:** Access to the underlying database and OS so you can:
  - Configure settings
  - Install patches
  - Enable native features
  - Access the underlying EC2 instance using SSH or SSM Session Manager
- De-activate Automation Mode to perform your customization; take a DB snapshot first
- **RDS vs. RDS Custom:**
  - RDS: entire database and OS managed by AWS
  - RDS Custom: full admin access to the underlying OS and the database

### Amazon Aurora

- Aurora is a proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB (your drivers will work as if Aurora was Postgres or MySQL)
- Aurora is "AWS cloud optimized":
  - Claims 5x performance improvement over MySQL on RDS
  - Over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10 GB, up to 256 TB
- Aurora can have up to 15 replicas and the replication process is faster than MySQL (sub 10 ms replica lag)
- Failover in Aurora is instantaneous. It’s HA (High Availability) native
- Aurora costs more than RDS (20% more) – but is more efficient

### Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZs:
  - 4 copies out of 6 needed for writes
  - 3 copies out of 6 needed for reads
  - Self-healing with peer-to-peer replication
  - Storage is striped across 100s of volumes
- One Aurora instance takes writes (master)
- Automated failover for master in less than 30 seconds
- Master + up to 15 Aurora Read Replicas serve reads
- Support for Cross-Region Replication

### Aurora DB Cluster

- **Writer Endpoint:** Points to the master instance
- **Reader Endpoint:** Provides connection load balancing across Read Replicas
- Auto Scaling of Read Replicas
- Shared storage volume: auto expanding from 10 GB to 256 TB

### Features of Aurora

- Automatic fail-over
- Backup and recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated patching with zero downtime
- Advanced monitoring
- Routine maintenance
- **Backtrack:** restore data at any point in time without using backups

### Aurora Replicas – Auto Scaling

- When CPU usage on reader replicas is high, Aurora auto-scales the number of replicas
- The Reader Endpoint is automatically extended to include new replicas

### Aurora – Custom Endpoints

- Define a subset of Aurora instances as a Custom Endpoint
- Example: run analytical queries on larger/more powerful specific replicas
- The Reader Endpoint is generally not used after defining Custom Endpoints

### Aurora Serverless

- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent, intermittent, or unpredictable workloads
- No capacity planning needed
- Pay per second – can be more cost-effective

### Global Aurora

- **Aurora Cross-Region Read Replicas:** Useful for disaster recovery, simple to put in place
- **Aurora Global Database (recommended):**
  - 1 Primary Region (read/write)
  - Up to 10 secondary (read-only) regions, replication lag is less than 1 second
  - Up to 16 Read Replicas per secondary region
  - Helps decrease latency
  - Promoting another region (for disaster recovery) has an RTO of < 1 minute
  - Typical cross-region replication takes less than 1 second

### Aurora Machine Learning

- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services
- **Supported services:**
  - Amazon SageMaker (use with any ML model)
  - Amazon Comprehend (for sentiment analysis)
- You don’t need to have ML experience
- **Use cases:** fraud detection, ads targeting, sentiment analysis, product recommendations

### Babelfish for Aurora PostgreSQL

- Allows Aurora PostgreSQL to understand commands targeted for MS SQL Server (e.g., T-SQL)
- Microsoft SQL Server based applications can work on Aurora PostgreSQL
- Requires no (or little) code changes (using the same MS SQL Server client driver)
- The same applications can be used after a migration of your database (using AWS SCT and DMS)

### RDS Backups

**Automated backups:**
- Daily full backup of the database (during the backup window)
- Transaction logs are backed up by RDS every 5 minutes
- Ability to restore to any point in time (from oldest backup to 5 minutes ago)
- 1 to 35 days of retention; set 0 to disable automated backups

**Manual DB Snapshots:**
- Manually triggered by the user
- Retention of backup for as long as you want
- **Tip:** In a stopped RDS database, you still pay for storage. If stopped for a long time, snapshot & restore instead.

### Aurora Backups

**Automated backups:**
- 1 to 35 days (cannot be disabled)
- Point-in-time recovery in that timeframe

**Manual DB Snapshots:**
- Manually triggered by the user
- Retention of backup for as long as you want

### RDS & Aurora Restore Options

- Restoring a RDS/Aurora backup or a snapshot creates a new database
- **Restoring MySQL RDS database from S3:**
  1. Create a backup of your on-premises database
  2. Store it on Amazon S3
  3. Restore the backup file onto a new RDS instance running MySQL
- **Restoring MySQL Aurora cluster from S3:**
  1. Create a backup of your on-premises database using Percona XtraBackup
  2. Store the backup file on Amazon S3
  3. Restore the backup file onto a new Aurora cluster running MySQL

### Aurora Database Cloning

- Create a new Aurora DB Cluster from an existing one
- Faster than snapshot & restore
- Uses copy-on-write protocol:
  - Initially, the new DB cluster uses the same data volume as the original (no copying needed)
  - When updates are made to the new DB cluster, additional storage is allocated and data is copied separately
- Very fast & cost-effective
- Useful to create a "staging" database from a "production" database without impacting production

### RDS & Aurora Security

- **At-rest encryption:**
  - Database master & replicas encryption using AWS KMS – must be defined at launch time
  - If the master is not encrypted, the read replicas cannot be encrypted
  - To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
- **In-flight encryption:** TLS-ready by default; use the AWS TLS root certificates client-side
- **IAM Authentication:** IAM roles to connect to your database (instead of username/password)
- **Security Groups:** Control network access to your RDS/Aurora DB
- No SSH available except on RDS Custom
- Audit Logs can be enabled and sent to CloudWatch Logs for longer retention

### Amazon RDS Proxy

- Fully managed database proxy for RDS
- Allows apps to pool and share DB connections established with the database
- Improves database efficiency by reducing stress on database resources (e.g., CPU, RAM) and minimizing open connections (and timeouts)
- Serverless, autoscaling, highly available (multi-AZ)
- Reduced RDS & Aurora failover time by up to 66%
- Supports RDS (MySQL, PostgreSQL, MariaDB, MS SQL Server) and Aurora (MySQL, PostgreSQL)
- No code changes required for most apps
- Enforce IAM Authentication for DB, and securely store credentials in AWS Secrets Manager
- RDS Proxy is never publicly accessible (must be accessed from VPC)

### Amazon ElastiCache Overview

- ElastiCache is to get managed Redis or Memcached in the same way RDS is to get managed Relational Databases
- Caches are in-memory databases with really high performance and low latency
- Helps reduce load off of databases for read-intensive workloads
- Helps make your application stateless
- AWS takes care of OS maintenance/patching, optimizations, setup, configuration, monitoring, failure recovery, and backups
- **Note:** Using ElastiCache involves application code changes

### ElastiCache – Solution Architecture

**DB Cache:**
- Applications query ElastiCache first; if not available (cache miss), get from RDS and store in ElastiCache
- Helps relieve load in RDS
- Cache must have an invalidation strategy to ensure only the most current data is used

**User Session Store:**
- User logs into any instance of the application
- The application writes the session data into ElastiCache
- When the user hits another instance of the application, that instance retrieves the session from ElastiCache
- The user is already logged in (stateless application)

### ElastiCache – Redis vs Memcached

| Feature | Redis | Memcached |
| --- | --- | --- |
| Multi-AZ with Auto-Failover | ✓ | ✗ |
| Read Replicas | ✓ (scale reads & HA) | ✗ |
| Data Durability | ✓ (AOF persistence) | ✗ (non-persistent) |
| Backup and Restore | ✓ | ✗ |
| Sets and Sorted Sets | ✓ | ✗ |
| Multi-node sharding | ✓ | ✓ |
| Multi-threaded architecture | ✗ | ✓ |
| High Availability (replication) | ✓ | ✗ |

### ElastiCache – Cache Security

**Redis:**
- Supports IAM Authentication
- IAM policies on ElastiCache are only used for AWS API-level security
- **Redis AUTH:** Set a password/token when you create a Redis cluster (extra level of security on top of security groups)
- Supports SSL in-flight encryption

**Memcached:**
- Supports SASL-based authentication (advanced)

### Patterns for ElastiCache

| Pattern | Description |
| --- | --- |
| Lazy Loading | All read data is cached; data can become stale in cache |
| Write Through | Adds or updates data in the cache when written to a DB (no stale data) |
| Session Store | Store temporary session data in cache using TTL features |

> "There are only two hard things in Computer Science: cache invalidation and naming things."

### ElastiCache – Redis Use Case

- Gaming Leaderboards are computationally complex
- Redis Sorted Sets guarantee both uniqueness and element ordering
- Each time a new element is added, it’s ranked in real time, then added in correct order
- Clients interact with a real-time Leaderboard stored in ElastiCache for Redis
