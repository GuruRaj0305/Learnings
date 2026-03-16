## High Availability & Scalability

### Scalability & High Availability

- **Scalability** means that an application/system can handle greater loads by adapting
- There are two kinds of scalability:
  - Vertical Scalability
  - Horizontal Scalability (= elasticity)
- Scalability is linked but different to High Availability

### Vertical Scalability

- Vertically scalability means increasing the size of the instance
- For example, your application runs on a t2.micro
- Scaling that application vertically means running it on a t2.large
- Vertical scalability is very common for non-distributed systems, such as a database
- RDS, ElastiCache are services that can scale vertically
- There’s usually a limit to how much you can vertically scale (hardware limit)

### Horizontal Scalability

- Horizontal Scalability means increasing the number of instances/systems for your application
- Horizontal scaling implies distributed systems
- This is very common for web applications and modern applications
- It’s easy to horizontally scale thanks to cloud offerings such as Amazon EC2

### High Availability

- High Availability usually goes hand in hand with horizontal scaling
- High availability means running your application/system in at least 2 data centers (Availability Zones)
- The goal of high availability is to survive a data center loss
- High availability can be:
  - **Passive** (e.g., RDS Multi-AZ)
  - **Active** (e.g., horizontal scaling with multiple instances)

### High Availability & Scalability For EC2

| Type | Description |
| --- | --- |
| Vertical Scaling | Increase instance size (scale up/down). From t2.nano (0.5G RAM, 1 vCPU) to u-12tb1.metal (12.3 TB RAM, 448 vCPUs) |
| Horizontal Scaling | Increase number of instances (scale out/in) using Auto Scaling Group and Load Balancer |
| High Availability | Run instances across multi-AZ using Auto Scaling Group multi-AZ and Load Balancer multi-AZ |

### What is Load Balancing?

- Load balancers are servers that forward traffic to multiple servers (e.g., EC2 instances) downstream
- They distribute requests across downstream instances

**Why use a load balancer?**
- Spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances
- Do regular health checks to your instances
- Provide SSL termination (HTTPS) for your websites
- Enforce stickiness with cookies
- High availability across zones
- Separate public traffic from private traffic

### Why use an Elastic Load Balancer?

- An Elastic Load Balancer is a managed load balancer
  - AWS guarantees that it will be working
  - AWS takes care of upgrades, maintenance, high availability
  - AWS provides only a few configuration knobs
- It costs less to set up your own load balancer but it will be a lot more effort
- It is integrated with many AWS offerings/services:
  - EC2, EC2 Auto Scaling Groups, Amazon ECS
  - AWS Certificate Manager (ACM), CloudWatch
  - Route 53, AWS WAF, AWS Global Accelerator

### Health Checks

- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (`/health` is common)
- If the response is not 200 (OK), then the instance is unhealthy

### Types of Load Balancers on AWS

AWS has 4 kinds of managed Load Balancers:

| Load Balancer | Generation | Year | Protocols |
| --- | --- | --- | --- |
| Classic Load Balancer (CLB) | v1 (old) | 2009 | HTTP, HTTPS, TCP, SSL |
| Application Load Balancer (ALB) | v2 (new) | 2016 | HTTP, HTTPS, WebSocket |
| Network Load Balancer (NLB) | v2 (new) | 2017 | TCP, TLS, UDP |
| Gateway Load Balancer (GWLB) | v2 (new) | 2020 | Layer 3 (IP Protocol) |

- It is recommended to use the newer generation load balancers as they provide more features
- Some load balancers can be set up as internal (private) or external (public) ELBs

**Load Balancer Security Groups:**
- Load Balancer Security Group: allow HTTP/HTTPS from anywhere (0.0.0.0/0)
- Application Security Group: allow traffic only from the Load Balancer’s security group

### Classic Load Balancers (v1)

- Supports TCP (Layer 4), HTTP & HTTPS (Layer 7)
- Health checks are TCP or HTTP based
- Fixed hostname: `XXX.region.elb.amazonaws.com`

### Application Load Balancer (v2) – ALB

- Application Load Balancers operate at Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (e.g., containers)
- Support for HTTP/2 and WebSocket
- Support redirects (from HTTP to HTTPS for example)

**Routing rules to different target groups:**
- Routing based on path in URL (e.g., `example.com/users` & `example.com/posts`)
- Routing based on hostname in URL (e.g., `one.example.com` & `other.example.com`)
- Routing based on Query String, Headers (e.g., `example.com/users?id=123&order=false`)

- ALB is a great fit for microservices & container-based applications (Docker & Amazon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS

**Target Groups:**
- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups
- Health checks are at the target group level

**Good to Know:**
- Fixed hostname (`XXX.region.elb.amazonaws.com`)
- The application servers don’t see the IP of the client directly
- The true IP of the client is inserted in the header `X-Forwarded-For`
- You can also get Port (`X-Forwarded-Port`) and proto (`X-Forwarded-Proto`)

### Network Load Balancer (v2) – NLB

- Network Load Balancers operate at Layer 4:
  - Forward TCP & UDP traffic to your instances
  - Handle millions of requests per second
  - Ultra-low latency
- NLB has one static IP per AZ, and supports assigning Elastic IP (helpful for whitelisting specific IPs)
- NLB is used for extreme performance, TCP or UDP traffic

**Target Groups:**
- EC2 instances
- IP Addresses – must be private IPs
- Application Load Balancer
- Health Checks support TCP, HTTP and HTTPS protocols

### Gateway Load Balancer (GWLB)

- Deploy, scale, and manage a fleet of 3rd-party network virtual appliances in AWS
- Examples: Firewalls, Intrusion Detection and Prevention Systems, Deep Packet Inspection systems, payload manipulation
- Operates at Layer 3 (Network Layer) – IP Packets
- Combines two functions:
  - **Transparent Network Gateway** – single entry/exit for all traffic
  - **Load Balancer** – distributes traffic to your virtual appliances
- Uses the GENEVE protocol on port 6081

**Target Groups:**
- EC2 instances
- IP Addresses – must be private IPs

### Sticky Sessions (Session Affinity)

- It is possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer
- This works for Classic Load Balancer, Application Load Balancer, and Network Load Balancer
- For both CLB & ALB, the "cookie" used for stickiness has an expiration date you control
- **Use case:** make sure the user doesn’t lose their session data
- Enabling stickiness may bring imbalance to the load over the backend EC2 instances

**Sticky Sessions – Cookie Names:**

| Type | Cookie | Generated By |
| --- | --- | --- |
| Custom cookie (application-based) | Custom name (specified per target group) | Target (application) |
| Application cookie | AWSALBAPP | Load balancer |
| Duration-based cookie | AWSALB (ALB), AWSELB (CLB) | Load balancer |

> Don’t use AWSALB, AWSALBAPP, or AWSALBTG as custom cookie names (reserved for ELB).

### Cross-Zone Load Balancing

- **With Cross-Zone Load Balancing:** each load balancer instance distributes requests evenly across all registered instances in all AZs
- **Without Cross-Zone Load Balancing:** requests are distributed only across the instances in the node’s own AZ

| Load Balancer | Default | Inter-AZ Charges |
| --- | --- | --- |
| Application Load Balancer | Enabled (can be disabled at Target Group level) | No charges |
| Network Load Balancer & GWLB | Disabled | Charges apply if enabled |
| Classic Load Balancer | Disabled | No charges if enabled |

### SSL/TLS Basics

- An SSL Certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)
- **SSL** refers to Secure Sockets Layer, used to encrypt connections
- **TLS** refers to Transport Layer Security, which is a newer version
- Nowadays, TLS certificates are mainly used, but people still refer to them as SSL
- Public SSL certificates are issued by Certificate Authorities (CA): Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc.
- SSL certificates have an expiration date (you set) and must be renewed

### Load Balancer – SSL Certificates

- The load balancer uses an X.509 certificate (SSL/TLS server certificate)
- You can manage certificates using ACM (AWS Certificate Manager)
- You can create/upload your own certificates alternatively

**HTTPS listener:**
- You must specify a default certificate
- You can add an optional list of certs to support multiple domains
- Clients can use SNI (Server Name Indication) to specify the hostname they reach
- Ability to specify a security policy to support older versions of SSL/TLS (legacy clients)

### SSL – Server Name Indication (SNI)

- SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
- It’s a "newer" protocol, and requires the client to indicate the hostname of the target server in the initial SSL handshake
- The server will then find the correct certificate, or return the default one
- **Note:** Only works for ALB & NLB (newer generation), CloudFront. Does not work for CLB (older generation)

### Elastic Load Balancers – SSL Certificates

| Load Balancer | SSL Certificate Support |
| --- | --- |
| Classic Load Balancer (v1) | Only one SSL certificate. Must use multiple CLBs for multiple hostname/SSL certificates |
| Application Load Balancer (v2) | Supports multiple listeners with multiple SSL certificates. Uses SNI |
| Network Load Balancer (v2) | Supports multiple listeners with multiple SSL certificates. Uses SNI |

### Connection Draining

- **Feature naming:**
  - Connection Draining – for CLB
  - Deregistration Delay – for ALB & NLB
- Time to complete "in-flight requests" while the instance is de-registering or unhealthy
- Stops sending new requests to the EC2 instance which is de-registering
- Between 1 to 3600 seconds (default: 300 seconds)
- Can be disabled (set value to 0)
- Set to a low value if your requests are short

### What’s an Auto Scaling Group?

- In real life, the load on your websites and applications can change
- In the cloud, you can create and get rid of servers very quickly
- The goal of an Auto Scaling Group (ASG) is to:
  - **Scale out** (add EC2 instances) to match an increased load
  - **Scale in** (remove EC2 instances) to match a decreased load
  - Ensure a minimum and maximum number of EC2 instances running
  - Automatically register new instances to a load balancer
  - Re-create an EC2 instance in case a previous one is terminated (e.g., if unhealthy)
- ASG are free (you only pay for the underlying EC2 instances)

### Auto Scaling Group Attributes

A Launch Template (older "Launch Configurations" are deprecated) contains:
- AMI + Instance Type
- EC2 User Data
- EBS Volumes
- Security Groups
- SSH Key Pair
- IAM Roles for your EC2 instances
- Network + Subnets information
- Load Balancer information

**Scaling Configuration:**
- Min Size / Max Size / Desired Capacity
- Scaling Policies

### Auto Scaling – CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as Average CPU, or a custom metric)
- Metrics such as Average CPU are computed for the overall ASG instances
- Based on the alarm:
  - Create scale-out policies (increase the number of instances)
  - Create scale-in policies (decrease the number of instances)

### Auto Scaling Groups – Scaling Policies

**Dynamic Scaling:**
- **Target Tracking Scaling** – Simple to set up. Example: keep average ASG CPU at around 40%
- **Simple/Step Scaling** – When a CloudWatch alarm is triggered (CPU > 70%), add 2 units; when CPU < 30%, remove 1

**Scheduled Scaling:**
- Anticipate a scaling based on known usage patterns
- Example: increase the min capacity to 10 at 5 PM on Fridays

**Predictive Scaling:**
- Continuously forecast load and schedule scaling ahead

### Good Metrics to Scale On

| Metric | Description |
| --- | --- |
| CPUUtilization | Average CPU utilization across your instances |
| RequestCountPerTarget | Make sure the number of requests per EC2 instance is stable |
| Average Network In/Out | Useful if your application is network-bound |
| Custom Metric | Any metric you push using CloudWatch |

### Auto Scaling Groups – Scaling Cooldowns

- After a scaling activity happens, you are in the cooldown period (default 300 seconds)
- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
- **Advice:** Use a ready-to-use AMI to reduce configuration time in order to be serving requests faster and reduce the cooldown period
