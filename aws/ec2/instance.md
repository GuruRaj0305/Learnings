# EC2 Instance Types – Overview

- Different types of EC2 instances are optimized for different use cases
- Types: https://aws.amazon.com/ec2/instance-types/
- AWS naming convention: **m5.2xlarge**
  - `m`: instance class (m = general purpose)
  - `5`: generation (AWS improves them over time)
  - `2xlarge`: size within the instance class

## EC2 Instance Types

### General Purpose
- Great for a diversity of workloads such as web servers or code repositories
- Good balance between Compute, Memory, and Networking
- Instance codes start with `m` or `t`

### Compute Optimized
- Great for compute-intensive tasks that require high performance processors
- Use cases:
  - Batch processing workloads
  - Media transcoding
  - High performance web servers
  - High performance computing (HPC)
  - Scientific modeling & machine learning
  - Dedicated gaming servers
- Instance codes start with `c`

### Memory Optimized
- Fast performance for workloads that process large data sets in memory
- Use cases:
  - High performance relational/non-relational databases
  - Distributed web scale cache stores
  - In-memory databases optimized for BI (business intelligence)
  - Applications performing real-time processing of big unstructured data
- Instance codes typically start with `r`

### Storage Optimized
- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
- Use cases:
  - High frequency online transaction processing (OLTP) systems
  - Relational & NoSQL databases
  - Cache for in-memory databases (Redis)
  - Data warehousing applications
  - Distributed file systems

> To compare all instance types in detail: https://instances.vantage.sh
