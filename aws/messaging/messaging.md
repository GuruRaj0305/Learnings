## AWS Integration & Messaging

SQS, SNS & Kinesis


### Section Introduction

- When we start deploying multiple applications, they will inevitably need to communicate with one another
- There are two patterns of application communication:
  1. **Synchronous communications** (application to application)
  2. **Asynchronous / Event-based** (application to queue to application)
- Synchronous communication between applications can be problematic if there are sudden spikes of traffic
  - What if you need to suddenly encode 1000 videos but usually it’s 10?
  - In that case, it’s better to **decouple** your applications:
    - using **SQS**: queue model
    - using **SNS**: pub/sub model
    - using **Kinesis**: real-time streaming model
- These services can scale independently from our application!

### Amazon SQS – Standard Queue

- Oldest offering (over 10 years old)
- Fully managed service, used to decouple applications
- Attributes:
  - Unlimited throughput, unlimited number of messages in queue
  - Default retention of messages: 4 days, maximum of 14 days
  - Low latency (<10 ms on publish and receive)
  - Limitation of 1,024 KB per message sent
  - Can have duplicate messages (at least once delivery, occasionally)
  - Can have out-of-order messages (best effort ordering)

### SQS – Producing Messages

- Produced to SQS using the SDK (`SendMessage` API)
- The message is persisted in SQS until a consumer deletes it
- Message retention: default 4 days, up to 14 days
- Example: send an order to be processed (Order ID, Customer ID, any attributes)
- SQS standard: unlimited throughput

### SQS – Consuming Messages

- Consumers (running on EC2 instances, servers, or AWS Lambda)
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the messages (e.g., insert into RDS database)
- Delete the messages using the `DeleteMessage` API

### SQS – Multiple EC2 Instances Consumers

- Consumers receive and process messages in parallel
- At least once delivery; best-effort message ordering
- Consumers delete messages after processing them
- We can scale consumers horizontally to improve throughput of processing

### SQS with Auto Scaling Group (ASG)

- EC2 instances poll for messages from SQS Queue
- Auto Scaling Group scales based on CloudWatch Metric – Queue Length (`ApproximateNumberOfMessages`)
- Alarm triggers scale-out when metric breaches threshold

### SQS to Decouple Between Application Tiers

- Front-end web app sends messages to SQS via `SendMessage`
- Back-end processing application receives messages via `ReceiveMessages`
- SQS Queue is infinitely scalable
- Both tiers auto-scale independently

### Amazon SQS – Security

- **Encryption:**
  - In-flight encryption using HTTPS API
  - At-rest encryption using KMS keys
  - Client-side encryption if the client wants to perform encryption/decryption itself
- **Access Controls:** IAM policies to regulate access to the SQS API
- **SQS Access Policies** (similar to S3 bucket policies):
  - Useful for cross-account access to SQS queues
  - Useful for allowing other services (SNS, S3…) to write to an SQS queue

### SQS – Message Visibility Timeout

- After a message is polled by a consumer, it becomes **invisible** to other consumers
- By default, the message visibility timeout is **30 seconds**
- That means the message has 30 seconds to be processed
- After the message visibility timeout is over, the message is “visible” in SQS again
- If a message is not processed within the timeout, it will be processed twice
- A consumer could call the `ChangeMessageVisibility` API to get more time
- If timeout is high (hours) and consumer crashes, re-processing will take time
- If timeout is too low (seconds), we may get duplicates

### Amazon SQS – Long Polling

- When a consumer requests messages from the queue, it can optionally “wait” for messages to arrive if there are none in the queue — this is called **Long Polling**
- Long Polling decreases the number of API calls made to SQS while increasing efficiency and reducing latency
- The wait time can be between 1 sec to 20 sec (20 sec preferable)
- Long Polling is preferable to Short Polling
- Long polling can be enabled at the queue level or at the API level using `WaitTimeSeconds`

### Amazon SQS – FIFO Queue

- FIFO = First In First Out (ordering of messages in the queue)
- Limited throughput: 300 msg/s without batching, 3000 msg/s with batching
- Exactly-once send capability (by removing duplicates using Deduplication ID)
- Messages are processed in order by the consumer
- Ordering by Message Group ID (all messages in the same group are ordered) — mandatory parameter

### SQS as a Buffer to Database Writes

- Application enqueues requests to SQS queue via `SendMessage`
- Consumer dequeues messages via `ReceiveMessages` and inserts into database
- SQS Queue is infinitely scalable
- Protects database from being overwhelmed by large spikes in traffic

### Amazon SNS

- The “event producer” only sends message to one SNS topic
- As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
- Each subscriber to the topic will get all the messages (new feature: filter messages)
- Up to 12,500,000 subscriptions per topic; 100,000 topics limit
- **Subscribers can be:** SQS, Lambda, Kinesis Data Firehose, Emails, SMS & Mobile Notifications, HTTP(S) Endpoints

### SNS Integrates with a Lot of AWS Services

- Many AWS services can send data directly to SNS for notifications:
  - CloudWatch Alarms, AWS Budgets, Lambda, Auto Scaling Group Notifications
  - S3 Bucket Events, DynamoDB, CloudFormation, AWS DMS, RDS Events

### Amazon SNS – How to Publish

- **Topic Publish** (using the SDK):
  1. Create a topic
  2. Create a subscription (or many)
  3. Publish to the topic
- **Direct Publish** (for mobile apps SDK):
  1. Create a platform application
  2. Create a platform endpoint
  3. Publish to the platform endpoint
  - Works with Google GCM, Apple APNS, Amazon ADM…

### Amazon SNS – Security

- **Encryption:**
  - In-flight encryption using HTTPS API
  - At-rest encryption using KMS keys
  - Client-side encryption if the client wants to perform encryption/decryption itself
- **Access Controls:** IAM policies to regulate access to the SNS API
- **SNS Access Policies** (similar to S3 bucket policies):
  - Useful for cross-account access to SNS topics
  - Useful for allowing other services (S3…) to write to an SNS topic

### SNS + SQS: Fan Out

- Push once in SNS, receive in all SQS queues that are subscribers
- Fully decoupled, no data loss
- SQS allows for: data persistence, delayed processing, and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue access policy allows for SNS to write
- Cross-Region Delivery: works with SQS Queues in other regions

### Application: S3 Events to Multiple Queues

- For the same combination of event type (e.g., object create) and prefix (e.g., `images/`) you can only have one S3 Event rule
- If you want to send the same S3 event to many SQS queues, use fan-out:
  - S3 → SNS Topic → (Fan-out) → multiple SQS Queues / Lambda Functions

### Application: SNS to Amazon S3 through Kinesis Data Firehose

- SNS can send to Kinesis Data Firehose, enabling this architecture:
  - Service → SNS Topic → Kinesis Data Firehose → Amazon S3 / any supported KDF destination

### Amazon SNS – FIFO Topic

- FIFO = First In First Out (ordering of messages in the topic)
- Similar features as SQS FIFO:
  - Ordering by Message Group ID (all messages in the same group are ordered)
  - Deduplication using a Deduplication ID or Content Based Deduplication
- Can have SQS Standard and FIFO queues as subscribers
- Limited throughput (same throughput as SQS FIFO)

### SNS FIFO + SQS FIFO: Fan Out

- In case you need fan-out + ordering + deduplication
- SNS FIFO Topic → multiple SQS FIFO Queues as subscribers

### SNS – Message Filtering

- JSON policy used to filter messages sent to SNS topic’s subscriptions
- If a subscription doesn’t have a filter policy, it receives every message
- **Example:** An order buying service publishes to SNS Topic; different SQS queues receive only relevant messages:
  - Filter `State: Placed` → SQS Queue (Placed orders)
  - Filter `State: Cancelled` → SQS Queue (Cancelled orders) + Email Subscription
  - Filter `State: Declined` → SQS Queue (Declined orders)
  - No filter → SQS Queue (All)

### Amazon Kinesis Data Streams

- Collect and store streaming data in real-time
- **Producers:** Applications, Click Streams, IoT devices, Kinesis Agent, Metrics & Logs
- **Consumers:** Application, Lambda, Amazon Data Firehose, Managed Service for Apache Flink

### Kinesis Data Streams

- Retention between up to 365 days
- Ability to reprocess (replay) data by consumers
- Data can’t be deleted from Kinesis (until it expires)
- Data up to 10 MiB (typical use case is lots of “small” real-time data)
- Data ordering guarantee for data with the same “Partition ID”
- At-rest KMS encryption, in-flight HTTPS encryption
- Kinesis Producer Library (KPL) to write an optimized producer application
- Kinesis Client Library (KCL) to write an optimized consumer application

### Kinesis Data Streams – Capacity Modes

- **Provisioned mode:**
  - Choose number of shards
  - Each shard gets 1 MB/s in (or 1000 records per second)
  - Each shard gets 2 MB/s out
  - Scale manually to increase or decrease shards
  - Pay per shard provisioned per hour
- **On-demand mode:**
  - No need to provision or manage capacity
  - Default capacity: 4 MB/s in or 4000 records per second
  - Scales automatically based on observed throughput peak during the last 30 days
  - Pay per stream per hour & data in/out per GB

### Amazon Data Firehose

- Note: used to be called “Kinesis Data Firehose”
- Fully Managed Service; automatic scaling, serverless, pay for what you use
- **Destinations:**
  - AWS: Amazon S3, Amazon Redshift, Amazon OpenSearch
  - 3rd party: Splunk, MongoDB, Datadog, NewRelic, …
  - Custom HTTP Endpoint
- Near Real-Time with buffering capability based on size / time
- Supports CSV, JSON, Parquet, Avro, Raw Text, Binary data
- Conversions to Parquet / ORC; compressions with gzip / snappy
- Custom data transformations using AWS Lambda (e.g., CSV to JSON)
- All or failed data can be backed up to an S3 bucket

### Kinesis Data Streams vs Amazon Data Firehose

| Feature | Kinesis Data Streams | Amazon Data Firehose |
| --- | --- | --- |
| Purpose | Streaming data collection | Load streaming data into S3 / Redshift / OpenSearch / 3rd party / custom HTTP |
| Code | Producer & Consumer code required | Fully managed |
| Latency | Real-time | Near real-time |
| Capacity | Provisioned / On-Demand mode | Automatic scaling |
| Storage | Data storage up to 365 days | No data storage |
| Replay | Replay capability | Doesn’t support replay |

### SQS vs SNS vs Kinesis

| Feature | SQS | SNS | Kinesis |
| --- | --- | --- | --- |
| Model | Consumer “pull data” | Push data to many subscribers | Standard: pull data; Enhanced fan-out: push data |
| Throughput | No need to provision | No need to provision | 2 MB per shard (standard), 2 MB per shard per consumer (enhanced) |
| Subscribers/Consumers | As many workers as we want | Up to 12,500,000 subscribers; 100,000 topics | 1 per shard (standard) or many (enhanced fan-out) |
| Data persistence | Data deleted after consumed | Data not persisted (lost if not delivered) | Data expires after X days (up to 365) |
| Ordering | On FIFO queues only | Via SNS FIFO | At the shard level |
| Replay | Individual message delay capability | FIFO capability for SNS FIFO | Possibility to replay data |
| Use case | Decoupling; delayed processing | Fan-out; pub/sub | Real-time big data, analytics, ETL |

### Amazon MQ

- SQS, SNS are “cloud-native” services: proprietary protocols from AWS
- Traditional applications running from on-premises may use open protocols: MQTT, AMQP, STOMP, Openwire, WSS
- When migrating to the cloud, instead of re-engineering the application to use SQS and SNS, we can use **Amazon MQ**
- Amazon MQ is a managed message broker service (supports RabbitMQ and ActiveMQ)
- Amazon MQ doesn’t “scale” as much as SQS / SNS
- Amazon MQ runs on servers; can run in Multi-AZ with failover
- Amazon MQ has both queue feature (∼SQS) and topic features (∼SNS)

### Amazon MQ – High Availability

- Multi-AZ setup with active/standby brokers
- Active broker in one AZ uses Amazon EFS (shared storage) with standby in another AZ
- Client connects to Active broker; automatically fails over to Standby if Active goes down
