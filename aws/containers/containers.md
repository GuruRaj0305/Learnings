## Containers on AWS


### What is Docker?

- Docker is a software development platform to deploy apps
- Apps are packaged in containers that can be run on any OS
- Apps run the same, regardless of where they’re run:
  - Any machine, no compatibility issues
  - Predictable behavior
  - Less work, easier to maintain and deploy
  - Works with any language, any OS, any technology
- Use cases: microservices architecture, lift-and-shift apps from on-premises to AWS, …

### Docker vs. Virtual Machines

- Docker is “sort of” a virtualization technology, but not exactly
- Resources are shared with the host → many containers on one server
- Each Docker container shares the Host OS via Docker Daemon (no Guest OS per container)
- Virtual Machines require a Hypervisor and a separate Guest OS per VM — more overhead

### Where Are Docker Images Stored?

- Docker images are stored in Docker repositories:
  - **Docker Hub** (https://hub.docker.com): Public repository; find base images for Ubuntu, MySQL, etc.
  - **Amazon ECR** (Amazon Elastic Container Registry): Private repository + Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws)

### Docker Containers Management on AWS

| Service | Description |
| --- | --- |
| Amazon ECS | Amazon’s own container platform |
| Amazon EKS | Amazon’s managed Kubernetes (open-source) |
| AWS Fargate | Amazon’s own Serverless container platform; works with ECS and EKS |
| Amazon ECR | Store container images |

### Amazon ECS – EC2 Launch Type

- ECS = Elastic Container Service
- Launch Docker containers on AWS = Launch ECS Tasks on ECS Clusters
- **EC2 Launch Type:** You must provision & maintain the infrastructure (EC2 instances)
- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster
- AWS takes care of starting/stopping containers

### Amazon ECS – Fargate Launch Type

- Launch Docker containers on AWS
- You do **not** provision the infrastructure (no EC2 instances to manage)
- It’s all **Serverless!**
- You just create task definitions
- AWS runs ECS Tasks for you based on the CPU/RAM you need
- To scale, just increase the number of tasks — no more EC2 instances to manage

### Amazon ECS – IAM Roles for ECS

- **EC2 Instance Profile** (EC2 Launch Type only):
  - Used by the ECS agent
  - Makes API calls to ECS service
  - Sends container logs to CloudWatch Logs
  - Pulls Docker image from ECR
  - References sensitive data in Secrets Manager or SSM Parameter Store
- **ECS Task Role:**
  - Allows each task to have a specific role
  - Use different roles for different ECS Services
  - Task Role is defined in the task definition
  - Example: Task A → S3 Role; Task B → DynamoDB Role

### Amazon ECS – Load Balancer Integrations

- **Application Load Balancer:** Supported and works for most use cases
- **Network Load Balancer:** Recommended only for high throughput/high performance use cases, or to pair with AWS Private Link
- **Classic Load Balancer:** Supported but not recommended (no advanced features, no Fargate)

### Amazon ECS – Data Volumes (EFS)

- Mount EFS file systems onto ECS tasks
- Works for both EC2 and Fargate launch types
- Tasks running in any AZ share the same data in the EFS file system
- Fargate + EFS = Serverless
- Use cases: persistent multi-AZ shared storage for your containers
- Note: Amazon S3 cannot be mounted as a file system

### ECS Service Auto Scaling

- Automatically increase/decrease the desired number of ECS tasks
- Amazon ECS Auto Scaling uses AWS Application Auto Scaling
- Scaling metrics:
  - ECS Service Average CPU Utilization
  - ECS Service Average Memory Utilization (RAM)
  - ALB Request Count Per Target
- Scaling types:
  - **Target Tracking** – scale based on target value for a specific CloudWatch metric
  - **Step Scaling** – scale based on a specified CloudWatch Alarm
  - **Scheduled Scaling** – scale based on date/time (predictable changes)
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
- Fargate Auto Scaling is much easier to set up (because Serverless)

### EC2 Launch Type – Auto Scaling EC2 Instances

- **Auto Scaling Group Scaling:** Scale your ASG based on CPU Utilization; add EC2 instances over time
- **ECS Cluster Capacity Provider:**
  - Used to automatically provision and scale the infrastructure for ECS Tasks
  - Capacity Provider paired with an Auto Scaling Group
  - Adds EC2 Instances when you’re missing capacity (CPU, RAM…)

### ECS Tasks Invoked by Event Bridge

- Upload object to S3 Bucket → EventBridge event rule → triggers new ECS Task (Fargate)
- ECS Task Role grants access to S3 & DynamoDB
- Task saves result to DynamoDB

### ECS Tasks Invoked by Event Bridge Schedule

- EventBridge rule fires every 1 hour
- Triggers new ECS Task (Fargate) for batch processing
- ECS Task accesses Amazon S3

### ECS – SQS Queue Example

- Messages in SQS Queue polled by ECS Service
- ECS Service Auto Scaling scales tasks based on queue depth

### ECS – Intercept Stopped Tasks Using EventBridge

- ECS Task exits/stops → EventBridge captures event
- Triggers SNS notification to administrator via email

### Amazon ECR

- ECR = Elastic Container Registry
- Store and manage Docker images on AWS
- Private and Public repository (Amazon ECR Public Gallery https://gallery.ecr.aws)
- Fully integrated with ECS, backed by Amazon S3
- Access is controlled through IAM (permission errors → check policy)
- Supports image vulnerability scanning, versioning, image tags, image lifecycle

### Amazon EKS Overview

- Amazon EKS = Amazon Elastic Kubernetes Service
- It is a way to launch managed Kubernetes clusters on AWS
- Kubernetes is an open-source system for automatic deployment, scaling, and management of containerized applications
- Alternative to ECS, similar goal but different API
- EKS supports EC2 if you want to deploy worker nodes or Fargate to deploy serverless containers
- **Use case:** if your company is already using Kubernetes on-premises or in another cloud and wants to migrate to AWS
- Kubernetes is cloud-agnostic (can be used in Azure, GCP…)
- For multiple regions, deploy one EKS cluster per region
- Collect logs and metrics using CloudWatch Container Insights

### Amazon EKS – Node Types

| Type | Description |
| --- | --- |
| Managed Node Groups | Creates and manages Nodes (EC2 instances) for you; nodes are part of an ASG managed by EKS; supports On-Demand or Spot Instances |
| Self-Managed Nodes | Nodes created by you and registered to the EKS cluster managed by an ASG; can use prebuilt Amazon EKS Optimized AMI; supports On-Demand or Spot |
| AWS Fargate | No maintenance required; no nodes managed |

### Amazon EKS – Data Volumes

- Need to specify StorageClass manifest on your EKS cluster
- Leverages a Container Storage Interface (CSI) compliant driver
- Supports:
  - Amazon EBS
  - Amazon EFS (works with Fargate)
  - Amazon FSx for Lustre
  - Amazon FSx for NetApp ONTAP

### AWS App Runner

- Fully managed service that makes it easy to deploy web applications and APIs at scale
- No infrastructure experience required
- Start with your source code or container image (Docker)
- Automatically builds and deploys the web app
- Automatic scaling, highly available, load balancer, encryption
- VPC access support
- Connect to database, cache, and message queue services
- **Use cases:** web apps, APIs, microservices, rapid production deployments

### AWS App2Container (A2C)

- CLI tool for migrating and modernizing Java and .NET web apps into Docker containers
- Lift-and-shift apps running in on-premises bare metal, virtual machines, or any cloud to AWS
- Accelerate modernization; no code changes; migrate legacy apps
- Generates CloudFormation templates (compute, network…)
- Process:
  1. **Discover & Analyze** – create app inventory and analyze runtime dependencies
  2. **Extract & Containerize** – extract an app with dependencies and create a Docker image
  3. **Create Deployment Artifacts** – generate ECS Task and EKS Pod definitions, create CI/CD pipelines and other infrastructure
  4. **Deploy to AWS** – store Docker image in ECR, deploy to ECS, EKS, or App Runner
