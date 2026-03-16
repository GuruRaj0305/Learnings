## Disaster Recovery & Migrations


### Disaster Recovery Overview

- Any event that has a negative impact on a company's business continuity or finances is a disaster
- Disaster recovery (DR) is about preparing for and recovering from a disaster
- Disaster recovery options:
  - On-premise ⇒ On-premise: traditional DR, very expensive
  - On-premise ⇒ AWS Cloud: hybrid recovery
  - AWS Cloud Region A ⇒ AWS Cloud Region B
- Two key terms to define:
  - **RPO:** Recovery Point Objective – how much data loss is acceptable (point in time to recover to)
  - **RTO:** Recovery Time Objective – how long can the system be down (time to recover)


### RPO and RTO

- **RPO (Recovery Point Objective):** The maximum acceptable amount of data loss measured in time. Defines how frequently you must back up data.
- **RTO (Recovery Time Objective):** The maximum acceptable downtime. Defines how quickly you must restore service after a disaster.
- The gap between the disaster and the last backup = data loss (RPO)
- The gap between the disaster and full restoration = downtime (RTO)


### Disaster Recovery Strategies

Listed from highest RPO/RTO (cheapest) to lowest (most expensive/fastest recovery):

1. **Backup and Restore** – High RPO, high RTO, lowest cost
2. **Pilot Light** – Core systems always running in cloud; faster than Backup & Restore
3. **Warm Standby** – Full system running at minimum size; scale up on disaster
4. **Hot Site / Multi-Site Approach** – Full production scale running on both AWS and On-Premise; very low RTO, highest cost


### Backup and Restore (High RPO)

- Regular snapshots and backups pushed to AWS
- On disaster: restore from snapshot/backup
- Tools:
  - AWS Storage Gateway (sync on-premises data to S3)
  - AWS Snowball (bulk data migration)
  - S3 lifecycle policies → Glacier for archival
  - Scheduled RDS and EC2 snapshots
- High RPO (depends on backup frequency) and high RTO (restore takes time)


### Disaster Recovery – Pilot Light

- A small version of the app is always running in the cloud
- Useful for the critical core (pilot light)
- Very similar to Backup and Restore
- Faster than Backup and Restore as critical systems are already up
- Typical setup:
  - RDS is running and continuously replicated from on-premises
  - EC2 instances are stopped (not running) – started only on failover
  - Route 53 is updated to point to the AWS environment on disaster


### Warm Standby

- Full system is up and running, but at minimum size
- Upon disaster, scale to production load
- Typical setup:
  - Route 53 routes traffic to AWS on failover
  - EC2 Auto Scaling group running at minimum capacity (scaled up on disaster)
  - RDS Secondary is running (replicated from Primary)
  - Reverse proxy / load balancer routes traffic to EC2 instances


### Multi Site / Hot Site Approach

- Very low RTO (minutes or seconds) – very expensive
- Full Production Scale running on both AWS and On-Premise (active-active)
- Route 53 routes traffic to both environments simultaneously
- On disaster, Route 53 removes the failed environment
- All AWS Multi-Region version: two AWS regions running active-active, with Aurora Global (primary + secondary) and EC2 Auto Scaling in both regions


### Disaster Recovery Tips

**Backup:**

- EBS Snapshots, RDS automated backups / Snapshots, etc…
- Regular pushes to S3 / S3 IA / Glacier, Lifecycle Policy, Cross Region Replication
- From On-Premise: Snowball or Storage Gateway

**High Availability:**

- Use Route 53 to migrate DNS over from Region to Region
- RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
- Site-to-Site VPN as a recovery from Direct Connect

**Replication:**

- RDS Replication (Cross Region), AWS Aurora + Global Databases
- Database replication from on-premises to RDS
- Storage Gateway

**Automation:**

- CloudFormation / Elastic Beanstalk to re-create a whole new environment
- Recover / Reboot EC2 instances with CloudWatch if alarms fail
- AWS Lambda functions for customized automations

**Chaos Engineering:**

- Netflix has a “simian-army” randomly terminating EC2 instances


### AWS Elastic Disaster Recovery (DRS)

- Used to be named “CloudEndure Disaster Recovery”
- Quickly and easily recover your physical, virtual, and cloud-based servers into AWS
- Example: protect your most critical databases (including Oracle, MySQL, and SQL Server), enterprise apps (SAP), protect your data from ransomware attacks
- Continuous block-level replication for your servers
- Workflow:
  1. AWS Replication Agent installed on source servers (corporate data center or any cloud)
  2. Continuous replication to low-cost staging EC2 instances and EBS volumes in AWS
  3. On failover: launch target EC2 instances and EBS volumes (within minutes/seconds)
  4. On failback: replicate back to the original environment


### DMS – Database Migration Service

- Quickly and securely migrate databases to AWS; resilient, self-healing
- The source database remains available during the migration
- Supports:
  - **Homogeneous migrations:** e.g., Oracle to Oracle
  - **Heterogeneous migrations:** e.g., Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC (Change Data Capture)
- You must create an EC2 instance to perform the replication tasks


### DMS Sources and Targets

| | Sources | Targets |
| --- | --- | --- |
| On-Premises / EC2 | Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2 | Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP |
| Amazon RDS | All including Aurora | Redshift, DynamoDB, S3 |
| Other | Azure SQL Database, Amazon S3, DocumentDB | OpenSearch Service, Kinesis Data Streams, Apache Kafka, DocumentDB, Amazon Neptune, Redis, Babelfish |


### AWS Schema Conversion Tool (SCT)

- Convert your Database’s Schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
- Prefer compute-intensive instances to optimize data conversions
- You do not need to use SCT if you are migrating the same DB engine
  - Ex: On-Premise PostgreSQL ⇒ RDS PostgreSQL (the DB engine is still PostgreSQL; RDS is the platform)


### DMS – Continuous Replication

Setup for full load + CDC replication:

1. Install AWS SCT on a server to convert the schema
2. DMS Replication Instance runs in a public subnet, connected to source (Oracle DB on-premises) and target (Amazon RDS for MySQL)
3. Full load migrates existing data; CDC replication keeps the target in sync with ongoing changes


### AWS DMS – Multi-AZ Deployment

- When Multi-AZ is enabled, DMS provisions and maintains a synchronous stand-by replica in a different AZ
- Advantages:
  - Provides Data Redundancy
  - Eliminates I/O freezes
  - Minimizes latency spikes


### RDS & Aurora MySQL Migrations

**RDS MySQL to Aurora MySQL:**

- Option 1: DB Snapshots from RDS MySQL restored as MySQL Aurora DB
- Option 2: Create an Aurora Read Replica from your RDS MySQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)

**External MySQL to Aurora MySQL:**

- Option 1: Use Percona XtraBackup to create a file backup in Amazon S3, then create an Aurora MySQL DB from Amazon S3
- Option 2: Create an Aurora MySQL DB and use the `mysqldump` utility to migrate MySQL into Aurora (slower than S3 method)
- Use DMS if both databases are up and running


### RDS & Aurora PostgreSQL Migrations

**RDS PostgreSQL to Aurora PostgreSQL:**

- Option 1: DB Snapshots from RDS PostgreSQL restored as PostgreSQL Aurora DB
- Option 2: Create an Aurora Read Replica from your RDS PostgreSQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)

**External PostgreSQL to Aurora PostgreSQL:**

- Create a backup and put it in Amazon S3
- Import it using the `aws_s3` Aurora extension
- Use DMS if both databases are up and running


### On-Premise Strategy with AWS

- **Download Amazon Linux 2 AMI as a VM** (.iso format) for VMware, KVM, VirtualBox, Microsoft Hyper-V
- **VM Import / Export:** Migrate existing applications into EC2; create a DR repository strategy for your on-premises VMs; can export back the VMs from EC2 to on-premises
- **AWS Application Discovery Service:** Gather information about on-premises servers to plan a migration; server utilization and dependency mappings; track with AWS Migration Hub
- **AWS Database Migration Service (DMS):** Replicate On-premise ⇒ AWS, AWS ⇒ AWS, AWS ⇒ On-premise; works with various database technologies
- **AWS Application Migration Service (MGN):** Incremental replication of on-premises live servers to AWS


### AWS Backup

- Fully managed service
- Centrally manage and automate backups across AWS services
- No need to create custom scripts and manual processes
- Supported services:
  - Amazon EC2 / Amazon EBS
  - Amazon S3
  - Amazon RDS (all DB engines) / Amazon Aurora / Amazon DynamoDB
  - Amazon DocumentDB / Amazon Neptune
  - Amazon EFS / Amazon FSx (Lustre & Windows File Server)
  - AWS Storage Gateway (Volume Gateway)
- Supports cross-region backups
- Supports cross-account backups
- Supports PITR for supported services
- On-Demand and Scheduled backups
- Tag-based backup policies

**Backup Plans:**

- Backup frequency (every 12 hours, daily, weekly, monthly, cron expression)
- Backup window
- Transition to Cold Storage (Never, Days, Weeks, Months, Years)
- Retention Period (Always, Days, Weeks, Months, Years)


### AWS Backup Vault Lock

- Enforce a WORM (Write Once Read Many) state for all the backups that you store in your AWS Backup Vault
- Additional layer of defense to protect your backups against:
  - Inadvertent or malicious delete operations
  - Updates that shorten or alter retention periods
- Even the root user cannot delete backups when enabled


### AWS Application Discovery Service

- Plan migration projects by gathering information about on-premises data centers
- Server utilization data and dependency mapping are important for migrations
- **Agentless Discovery (AWS Agentless Discovery Connector):** VM inventory, configuration, and performance history such as CPU, memory, and disk usage
- **Agent-based Discovery (AWS Application Discovery Agent):** System configuration, system performance, running processes, and details of the network connections between systems
- Resulting data can be viewed within AWS Migration Hub


### AWS Application Migration Service (MGN)

- The “AWS evolution” of CloudEndure Migration, replacing AWS Server Migration Service (SMS)
- Lift-and-shift (rehost) solution which simplifies migrating applications to AWS
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- Supports a wide range of platforms, Operating Systems, and databases
- Minimal downtime, reduced costs
- Workflow:
  1. Install AWS Replication Agent on source servers
  2. Continuous replication to low-cost staging EC2 instances in AWS
  3. On cutover: launch target production EC2 instances


### VMware Cloud on AWS

- Some customers use VMware Cloud to manage their on-premises Data Center
- They want to extend Data Center capacity to AWS, but keep using the VMware Cloud software
- Use cases:
  - Migrate your VMware vSphere-based workloads to AWS
  - Run your production workloads across VMware vSphere-based private, public, and hybrid cloud environments
  - Have a disaster recovery strategy
- VMware Cloud on AWS runs in the AWS Cloud alongside native AWS services (EC2, S3, RDS, Redshift, etc.)


### Transferring Large Amounts of Data into AWS

Example: transfer 200 TB of data in the cloud with a 100 Mbps internet connection.

| Method | Setup Time | Transfer Time |
| --- | --- | --- |
| Internet / Site-to-Site VPN (100 Mbps) | Immediate | ~185 days |
| Direct Connect (1 Gbps) | Over a month | ~18.5 days |
| Snowball | ~1 week end-to-end | 1 week |

- Snowball can be combined with DMS for database migrations
- For ongoing replication / transfers: Site-to-Site VPN or DX with DMS or DataSync
