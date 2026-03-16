## CloudFront & Global Accelerator


### Amazon CloudFront

- Content Delivery Network (CDN)
- Improves read performance; content is cached at the edge
- Improves user experience
- Hundreds of Points of Presence globally (edge locations, caches)
- DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall

### CloudFront – Origins

- **S3 bucket**
  - For distributing files and caching them at the edge
  - For uploading files to S3 through CloudFront
  - Secured using Origin Access Control (OAC)
- **VPC Origin**
  - For applications hosted in VPC private subnets
  - Private Application Load Balancer / Network Load Balancer / EC2 Instances
- **Custom Origin (HTTP)**
  - S3 website (must first enable the bucket as a static S3 website)
  - Any public HTTP backend (e.g., Public ALB)

### CloudFront vs S3 Cross Region Replication

| Feature | CloudFront | S3 Cross Region Replication |
| --- | --- | --- |
| Network | Global Edge network | Must be set up per region |
| Caching | Files cached for a TTL (e.g., a day) | Files updated in near real-time |
| Access | Read & Write | Read only |
| Best for | Static content that must be available everywhere | Dynamic content needing low-latency in a few regions |

### CloudFront – ALB or EC2 as an Origin

**Using VPC Origins:**
- Allows you to deliver content from applications hosted in VPC private subnets (no need to expose them on the Internet)
- Delivers traffic to private:
  - Application Load Balancer
  - Network Load Balancer
  - EC2 Instances

**Using Public Network:**
- Security group must allow Public IPs of CloudFront Edge Locations
- Reference: http://d7uri8nf7uskq.cloudfront.net/tools/list-cloudfront-ips
- EC2 instances must be Public; ALB must be Public
- Security group of ALB can allow Security Group of Load Balancer

### CloudFront Geo Restriction

- You can restrict who can access your distribution:
  - **Allowlist:** Allow users to access content only if they’re in an approved country
  - **Blocklist:** Prevent users from accessing content if they’re in a banned country
- The “country” is determined using a 3rd party Geo-IP database
- **Use case:** Copyright laws to control access to content

### CloudFront – Cache Invalidations

- If you update the back-end origin, CloudFront won’t immediately serve the refreshed content — it waits until the TTL expires
- You can force an entire or partial cache refresh (bypassing TTL) by performing a **CloudFront Invalidation**
- You can invalidate all files (`*`) or a specific path (e.g., `/images/*`)

### Global Users for Our Application

- You have deployed an application and have global users who want to access it directly
- They go over the public internet, which can add lots of latency due to many hops
- We wish to go as fast as possible through the AWS network to minimize latency

### Unicast IP vs Anycast IP

- **Unicast IP:** One server holds one IP address
- **Anycast IP:** All servers hold the same IP address and the client is routed to the nearest one

### AWS Global Accelerator

- Leverages the AWS internal network to route to your application
- 2 Anycast IPs are created for your application
- The Anycast IPs send traffic directly to Edge Locations
- The Edge Locations send the traffic to your application

**Features:**
- Works with Elastic IP, EC2 instances, ALB, NLB, public or private
- **Consistent Performance:**
  - Intelligent routing to lowest latency and fast regional failover
  - No issue with client cache (because the IP doesn’t change)
  - Internal AWS network
- **Health Checks:**
  - Global Accelerator performs a health check of your applications
  - Helps make your application global (failover less than 1 minute for unhealthy)
  - Great for disaster recovery
- **Security:**
  - Only 2 external IPs need to be whitelisted
  - DDoS protection thanks to AWS Shield

### AWS Global Accelerator vs CloudFront

Both use the AWS global network and its edge locations around the world. Both services integrate with AWS Shield for DDoS protection.

| Feature | CloudFront | Global Accelerator |
| --- | --- | --- |
| Caching | Yes – content served at the edge | No – proxying packets at the edge |
| Protocol | HTTP | TCP or UDP |
| Use case | Static/dynamic content, API acceleration | Non-HTTP (gaming, IoT, VoIP); HTTP needing static IP or deterministic regional failover |
