## Amazon EC2 – Instance Storage

### What’s an EBS Volume?

- An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
- It allows your instances to persist data, even after their termination
- They can only be mounted to one instance at a time (at the CCP level)
- They are bound to a specific availability zone
- Analogy: Think of them as a "network USB stick"

**EBS Volume – Key Properties:**
- It’s a network drive (not a physical drive)
- It uses the network to communicate with the instance, which means there might be some latency
- It can be detached from an EC2 instance and attached to another one quickly
- It’s locked to an Availability Zone (AZ)
  - An EBS Volume in us-east-1a cannot be attached to us-east-1b
  - To move a volume across AZs, you first need to snapshot it
- Has a provisioned capacity (size in GBs, and IOPS)
- You get billed for all the provisioned capacity
- You can increase the capacity of the drive over time

### EBS – Delete on Termination Attribute

- Controls the EBS behaviour when an EC2 instance terminates
- By default, the root EBS volume is deleted (attribute enabled)
- By default, any other attached EBS volume is not deleted (attribute disabled)
- This can be controlled by the AWS console / AWS CLI
- **Use case:** preserve root volume when instance is terminated

### EBS Snapshots

- Make a backup (snapshot) of your EBS volume at a point in time
- Not necessary to detach the volume to do a snapshot, but recommended
- Can copy snapshots across AZ or Region

**EBS Snapshot Features:**

| Feature | Description |
| --- | --- |
| EBS Snapshot Archive | Move a snapshot to an "archive tier" that is 75% cheaper. Takes 24–72 hours for restoring |
| Recycle Bin for EBS Snapshots | Set up rules to retain deleted snapshots so you can recover them after accidental deletion. Specify retention from 1 day to 1 year |
| Fast Snapshot Restore (FSR) | Force full initialization of snapshot to have no latency on first use (costs extra) |

### AMI Overview

- AMI = Amazon Machine Image
- AMI are a customization of an EC2 instance
  - You add your own software, configuration, operating system, monitoring, etc.
  - Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- You can launch EC2 instances from:
  - **Public AMI:** AWS provided
  - **Your own AMI:** you make and maintain them yourself
  - **AWS Marketplace AMI:** an AMI someone else made (and potentially sells)

**AMI Process (from an EC2 instance):**
1. Start an EC2 instance and customize it
2. Stop the instance (for data integrity)
3. Build an AMI – this will also create EBS snapshots
4. Launch instances from the newly created AMI

### EC2 Instance Store

- EBS volumes are network drives with good but "limited" performance
- If you need a high-performance hardware disk, use EC2 Instance Store
- **Better I/O performance**
- EC2 Instance Store loses its storage if the instance is stopped (ephemeral)
- Good for buffer / cache / scratch data / temporary content
- Risk of data loss if hardware fails
- Backups and Replication are your responsibility

### EBS Volume Types

EBS Volumes come in 6 types:

| Type | Category | Description |
| --- | --- | --- |
| gp2 / gp3 | SSD | General purpose SSD that balances price and performance for a wide variety of workloads |
| io1 / io2 Block Express | SSD | Highest-performance SSD for mission-critical, low-latency or high-throughput workloads |
| st1 | HDD | Low cost HDD designed for frequently accessed, throughput-intensive workloads |
| sc1 | HDD | Lowest cost HDD designed for less frequently accessed workloads |

- EBS Volumes are characterized in Size | Throughput | IOPS (I/O Ops Per Sec)
- Only gp2/gp3 and io1/io2 Block Express can be used as boot volumes

**General Purpose SSD (gp2/gp3):**
- Cost-effective storage, low-latency
- System boot volumes, virtual desktops, development and test environments
- 1 GiB - 16 TiB
- **gp3:** Baseline of 3,000 IOPS and throughput of 125 MiB/s. Can increase IOPS up to 16,000 and throughput up to 1,000 MiB/s independently
- **gp2:** Small gp2 volumes can burst IOPS to 3,000. Size and IOPS are linked, max IOPS is 16,000 (3 IOPS per GiB)

**Provisioned IOPS (PIOPS) SSD (io1/io2):**
- Critical business applications with sustained IOPS performance
- For applications that need more than 16,000 IOPS
- Great for database workloads (sensitive to storage performance and consistency)
- **io1 (4 GiB - 16 TiB):** Max PIOPS: 64,000 for Nitro EC2 instances & 32,000 for others. Can increase PIOPS independently from storage size
- **io2 Block Express (4 GiB – 64 TiB):** Sub-millisecond latency. Max PIOPS: 256,000 with IOPS:GiB ratio of 1,000:1. Supports EBS Multi-attach

**Hard Disk Drives (HDD) – st1/sc1:**
- Cannot be a boot volume
- 125 GiB to 16 TiB
- **Throughput Optimized HDD (st1):** Big Data, Data Warehouses, Log Processing. Max throughput 500 MiB/s – max IOPS 500
- **Cold HDD (sc1):** For infrequently accessed data where lowest cost is important. Max throughput 250 MiB/s – max IOPS 250

### EBS Multi-Attach – io1/io2 Family

- Attach the same EBS volume to multiple EC2 instances in the same AZ
- Each instance has full read & write permissions to the high-performance volume
- **Use case:**
  - Achieve higher application availability in clustered Linux applications (e.g., Teradata)
  - Applications must manage concurrent write operations
- Up to 16 EC2 instances at a time
- Must use a file system that’s cluster-aware (not XFS, EXT4, etc.)

### EBS Encryption

When you create an encrypted EBS volume, you get:
- Data at rest encrypted inside the volume
- All data in flight moving between the instance and the volume is encrypted
- All snapshots are encrypted
- All volumes created from the snapshot are encrypted
- Encryption and decryption are handled transparently (nothing required from you)
- Encryption has a minimal impact on latency
- EBS Encryption leverages keys from KMS (AES-256)
- Copying an unencrypted snapshot allows encryption
- Snapshots of encrypted volumes are encrypted

**How to encrypt an unencrypted EBS volume:**
1. Create an EBS snapshot of the volume
2. Encrypt the EBS snapshot (using copy)
3. Create a new EBS volume from the snapshot (the volume will also be encrypted)
4. Now you can attach the encrypted volume to the original instance

### Amazon EFS – Elastic File System

- Managed NFS (network file system) that can be mounted on many EC2 instances
- EFS works with EC2 instances in multi-AZ
- Highly available, scalable, expensive (3x gp2), pay per use
- **Use cases:** content management, web serving, data sharing, WordPress
- Uses NFSv4.1 protocol
- Uses security group to control access to EFS
- Compatible with Linux-based AMI (not Windows)
- Encryption at rest using KMS
- POSIX file system (~Linux) that has a standard file API
- File system scales automatically, pay-per-use, no capacity planning!

### EFS – Performance & Storage Classes

**EFS Scale:**
- 1000s of concurrent NFS clients, 10 GB+/s throughput
- Grows to Petabyte-scale network file system automatically

**Performance Mode (set at EFS creation time):**
- **General Purpose (default):** latency-sensitive use cases (web server, CMS, etc.)
- **Max I/O:** higher latency, throughput, highly parallel (big data, media processing)

**Throughput Mode:**
- **Bursting:** 1 TB = 50 MiB/s + burst of up to 100 MiB/s
- **Provisioned:** set your throughput regardless of storage size, e.g.: 1 GiB/s for 1 TB storage
- **Elastic:** automatically scales throughput up or down based on your workloads
  - Up to 3 GiB/s for reads and 1 GiB/s for writes
  - Used for unpredictable workloads

### EFS – Storage Classes

**Storage Tiers (lifecycle management feature – move file after N days):**

| Tier | Description |
| --- | --- |
| Standard | For frequently accessed files |
| Infrequent Access (EFS-IA) | Cost to retrieve files, lower price to store |
| Archive | Rarely accessed data (few times each year), ~50% cheaper |

- Implement lifecycle policies to move files between storage tiers

**Availability and Durability:**
- **Standard:** Multi-AZ, great for production
- **One Zone:** One AZ, great for dev; backup enabled by default; compatible with IA (EFS One Zone-IA)
- Over 90% in cost savings possible with One Zone-IA

### EBS vs EFS

**EBS volumes:**
- Attached to one instance (except Multi-Attach io1/io2)
- Locked at the Availability Zone (AZ) level
- gp2: IO increases if the disk size increases
- gp3 & io1: can increase IO independently
- To migrate an EBS volume across AZ: take a snapshot, then restore the snapshot to another AZ
- EBS backups use IO and shouldn’t be run while your application handles heavy traffic
- Root EBS volumes are terminated by default when the EC2 instance is terminated (can be disabled)

**EFS:**
- Can mount 100s of instances across AZs
- Share website files (e.g., WordPress)
- Only for Linux instances (POSIX)
- Higher price point than EBS
- Can leverage Storage Tiers for cost savings
