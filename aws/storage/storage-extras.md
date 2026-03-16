## AWS Storage Extras


### AWS Snowball

- Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
- Helps migrate up to Petabytes of data

| Device | Compute | Memory | Storage (SSD) |
| --- | --- | --- | --- |
| Snowball Edge Storage Optimized | 104 vCPUs | 416 GB | 210 TB |
| Snowball Edge Compute Optimized | 104 vCPUs | 416 GB | 28 TB |


### Data Migrations with Snowball

**Challenges with network transfers:**

| Data Size | 100 Mbps | 1 Gbps | 10 Gbps |
| --- | --- | --- | --- |
| 10 TB | 12 days | 30 hours | 3 hours |
| 100 TB | 124 days | 12 days | 30 hours |
| 1 PB | 3 years | 124 days | 12 days |

Other challenges: limited connectivity, limited bandwidth, high network cost, shared bandwidth (can’t maximize the line), connection stability.

AWS Snowball: offline devices to perform data migrations. **If it takes more than a week to transfer over the network, use Snowball devices!**

**Transfer options:**

- **Direct upload to S3:** Client uploads directly over internet (10 Gbit/s max)
- **With Snowball:** Client ships data to AWS Snowball device → AWS imports to S3


### What is Edge Computing?

- Process data while it’s being created on an edge location
- Examples: a truck on the road, a ship on the sea, a mining station underground
- These locations may have limited internet and no access to computing power
- We setup a Snowball Edge device to do edge computing
- Snowball Edge Compute Optimized (dedicated for that use case) & Storage Optimized
- Run EC2 Instances or Lambda functions at the edge
- Use cases: preprocess data, machine learning, transcoding media


### Solution Architecture: Snowball into Glacier

- Snowball cannot import to Glacier directly
- You must use Amazon S3 first, in combination with an S3 lifecycle policy

```
Snowball → Amazon S3 → (S3 lifecycle policy) → Amazon Glacier
```


### Amazon FSx – Overview

- Launch 3rd party high-performance file systems on AWS
- Fully managed service

Available options:
- FSx for Windows File Server
- FSx for Lustre
- FSx for NetApp ONTAP
- FSx for OpenZFS


### Amazon FSx for Windows (File Server)

- FSx for Windows is a fully managed Windows file system share drive
- Supports SMB protocol & Windows NTFS
- Microsoft Active Directory integration, ACLs, user quotas
- Can be mounted on Linux EC2 instances
- Supports Microsoft’s Distributed File System (DFS) Namespaces (group files across multiple FS)
- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
- Storage Options:
  - SSD – latency sensitive workloads (databases, media processing, data analytics, …)
  - HDD – broad spectrum of workloads (home directory, CMS, …)
- Can be accessed from your on-premises infrastructure (VPN or Direct Connect)
- Can be configured to be Multi-AZ (high availability)
- Data is backed-up daily to S3


### Amazon FSx for Lustre

- Lustre is a type of parallel distributed file system, for large-scale computing
- The name Lustre is derived from “Linux” and “cluster”
- Machine Learning, High Performance Computing (HPC)
- Video Processing, Financial Modeling, Electronic Design Automation
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
- Storage Options:
  - SSD – low-latency, IOPS intensive workloads, small & random file operations
  - HDD – throughput-intensive workloads, large & sequential file operations
- Seamless integration with S3: can “read S3” as a file system (through FSx) and write output back to S3
- Can be used from on-premises servers (VPN or Direct Connect)

**FSx Lustre – File System Deployment Options:**

| Option | Scratch File System | Persistent File System |
| --- | --- | --- |
| Storage | Temporary | Long-term |
| Replication | Not replicated | Replicated within same AZ |
| Burst speed | High burst (6x faster, 200 MBps per TiB) | Standard |
| Failed files | Not replaced | Replace within minutes |
| Use case | Short-term processing, optimize costs | Long-term processing, sensitive data |


### Amazon FSx for NetApp ONTAP

- Managed NetApp ONTAP on AWS
- File System compatible with NFS, SMB, iSCSI protocol
- Move workloads running on ONTAP or NAS to AWS
- Works with: Linux, Windows, MacOS, VMware Cloud on AWS, Amazon Workspaces & AppStream 2.0, Amazon EC2, ECS and EKS
- Storage shrinks or grows automatically
- Snapshots, replication, low-cost, compression and data de-duplication
- Point-in-time instantaneous cloning (helpful for testing new workloads)


### Amazon FSx for OpenZFS

- Managed OpenZFS file system on AWS
- File System compatible with NFS (v3, v4, v4.1, v4.2)
- Move workloads running on ZFS to AWS
- Works with: Linux, Windows, MacOS, VMware Cloud on AWS, Amazon Workspaces & AppStream 2.0, Amazon EC2, ECS and EKS
- Up to 1,000,000 IOPS with < 0.5ms latency
- Snapshots, compression and low-cost
- Point-in-time instantaneous cloning (helpful for testing new workloads)


### Hybrid Cloud for Storage

- AWS is pushing for “hybrid cloud” – part on-premises, part in the cloud
- Reasons: long cloud migrations, security requirements, compliance requirements, IT strategy
- S3 is a proprietary storage technology (unlike EFS / NFS), so how do you expose S3 data on-premises?
- **AWS Storage Gateway!**

**AWS Storage Cloud Native Options:**

| Type | Services |
| --- | --- |
| Block | Amazon EBS, EC2 Instance Store |
| File | Amazon EFS, Amazon FSx |
| Object | Amazon S3, Amazon Glacier |


### AWS Storage Gateway

- Bridge between on-premises data and cloud data
- Use cases: disaster recovery, backup & restore, tiered storage, on-premises cache & low-latency file access
- Types:
  - S3 File Gateway
  - Volume Gateway
  - Tape Gateway


### Amazon S3 File Gateway

- Configured S3 buckets are accessible using the NFS and SMB protocol
- Most recently used data is cached in the file gateway
- Supports S3 Standard, S3 Standard-IA, S3 One Zone-IA, S3 Intelligent Tiering
- Transition to S3 Glacier using a Lifecycle Policy
- Bucket access using IAM roles for each File Gateway
- SMB Protocol has integration with Active Directory (AD) for user authentication


### Volume Gateway

- Block storage using iSCSI protocol backed by S3
- Backed by EBS snapshots which can help restore on-premises volumes
- **Cached volumes:** Low latency access to most recent data
- **Stored volumes:** Entire dataset is on-premises, scheduled backups to S3


### Tape Gateway

- Some companies have backup processes using physical tapes
- With Tape Gateway, companies use the same processes but in the cloud
- Virtual Tape Library (VTL) backed by Amazon S3 and Glacier
- Back up data using existing tape-based processes (and iSCSI interface)
- Works with leading backup software vendors


### AWS Storage Gateway – Summary

| Gateway Type | Protocol | Target | Use Case |
| --- | --- | --- | --- |
| S3 File Gateway | NFS/SMB | Any S3 Storage Class (including Glacier) | User/group file shares |
| Volume Gateway | iSCSI | Amazon S3 → EBS Snapshots | Application server block storage |
| Tape Gateway | iSCSI VTL | Amazon S3 / Glacier | Backup application (tape replacement) |

**Deployment:** VM (VMware, Hyper-V, KVM) or hardware appliance


### AWS Transfer Family

- A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol
- Supported Protocols:
  - AWS Transfer for FTP (File Transfer Protocol)
  - AWS Transfer for FTPS (File Transfer Protocol over SSL)
  - AWS Transfer for SFTP (Secure File Transfer Protocol)
- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
- Pay per provisioned endpoint per hour + data transfers in GB
- Store and manage users’ credentials within the service
- Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)
- Usage: sharing files, public datasets, CRM, ERP, …


### AWS DataSync

- Move large amount of data to and from:
  - On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) – needs agent
  - AWS to AWS (different storage services) – no agent needed
- Can synchronize to: Amazon S3 (any storage class – including Glacier), Amazon EFS, Amazon FSx (Windows, Lustre, NetApp, OpenZFS…)
- Replication tasks can be scheduled hourly, daily, weekly
- File permissions and metadata are preserved (NFS POSIX, SMB…)
- One agent task can use 10 Gbps; can set up a bandwidth limit
- AWS Snowcone has the DataSync agent pre-installed


### Storage Comparison

| Storage | Type | Notes |
| --- | --- | --- |
| S3 | Object Storage | Scalable, durable |
| S3 Glacier | Object Archival | Low cost, retrieval delay |
| EBS volumes | Network Block Storage | One EC2 instance at a time |
| Instance Storage | Physical Block Storage | High IOPS, linked to EC2 |
| EFS | Network File System | Linux instances, POSIX filesystem |
| FSx for Windows | Network File System | Windows servers, SMB |
| FSx for Lustre | HPC File System | Linux, high performance |
| FSx for NetApp ONTAP | Multi-protocol File System | High OS compatibility |
| FSx for OpenZFS | Managed ZFS | ZFS-compatible |
| Storage Gateway | Hybrid Bridge | S3/FSx File Gateway, Volume Gateway, Tape Gateway |
| Transfer Family | Managed FTP/FTPS/SFTP | On top of S3 or EFS |
| DataSync | Data Sync Service | On-premises ↔ AWS, or AWS ↔ AWS |
| Snowcone/Snowball/Snowmobile | Physical Migration | Move large data physically |
| Database | Indexed Storage | Specific workloads with indexing and querying |
