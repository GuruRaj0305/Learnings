# EC2 Instance types - overview

+ Different types of EC2 instances that are optimized for different use cases.
+ types : <a href = "https://aws.amazon.com/ec2/instance-types/"> See types here </a>
+ AWS has following naming convention: **m5.2xlarge**
+ m: instance class (m for general perpose)
+ 5: generation (AWS improves them over time)
+ 2xlarge : size with the instance class

## EC2 instance types:

+ ### General purpose
  + Great for a diversity of workloads such as web servers or code repositories.
  + Good balance between : 
    + Compute 
    + memory
    + networking
  + instance code starts from m
  
+ ### Compute Optimized
  + Great for compute-intensive tasks that require high performance.
  + Perposes : 
    + Batch processing workloads
    + Media transcoding
    + High performance web servers
    + High performance computing (HPC)
    + Scientific modeling & machine learning
    + Dedicated gaming servers
  + instance code starts from c

+ ### Memory Optimized 
  + Fast performance for workloads that process large data sets in memory
  + Use Cases : 
    + High performance, relational / non-relational databases.
    + Distributed web scale cache stores
    + In-memory databases optimized for BI (business intelligence)
    + Application performing real-time processing og big unstructured data
  + normally starts with r
+ ### Storage Optimized 
  + Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage
  + Use cases:
    + High frequency online transaction processing (OLTP) systems
    + Relational & NoSQL databases
    + Cache for in-memory databases (Redis)
    + Data warehousing applications
    + Distributed file systems

`To see all in detail visit https://instances.vantage.sh`

