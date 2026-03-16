## Serverless Architectures


### Mobile Application: MyTodoList

**Requirements:**
- Expose as REST API with HTTPS
- Serverless architecture
- Users should be able to directly interact with their own folder in S3
- Users should authenticate through a managed serverless service
- Users can write and read to-dos, but mostly read
- Database should scale and have high read throughput

**Architecture:**
1. **REST API layer:** Mobile Client → Amazon API Gateway → AWS Lambda → Amazon DynamoDB; authenticated via Amazon Cognito
2. **Giving users access to S3:** Cognito provides permissions → Users can store/retrieve files in Amazon S3
3. **High read throughput:** Add DAX caching layer in front of DynamoDB for faster reads
4. **Caching at API Gateway:** Cache REST API responses at the API Gateway level

**Summary:**
- Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
- Cognito generates temporary credentials to access S3 bucket with restricted policy
- Caching reads on DynamoDB using DAX
- Caching REST requests at the API Gateway level
- Security with Cognito for authentication and authorization

### Serverless Hosted Website: MyBlog.com

**Requirements:**
- Must scale globally
- Blogs are rarely written, but often read
- Some website pages are purely static files; the rest is a dynamic REST API
- Caching must be implemented where possible
- New users who subscribe should receive a welcome email
- Photos uploaded should have a thumbnail generated

**Architecture:**

1. **Serving static content globally:**
   - Amazon S3 + Amazon CloudFront for global distribution
   - OAC (Origin Access Control) with bucket policy: only authorize requests from the CloudFront Distribution

2. **Adding a public serverless REST API:**
   - Amazon API Gateway → AWS Lambda → DAX → DynamoDB

3. **Leveraging DynamoDB Global Tables:** DynamoDB Global Tables for low-latency reads in multiple regions

4. **User welcome email flow:**
   - DynamoDB Streams captures changes → invokes Lambda → Lambda uses SES (Simple Email Service) to send welcome email with an IAM Role

5. **Thumbnail generation flow:**
   - Upload photos to S3 (with optional Transfer Acceleration via CloudFront)
   - S3 triggers Lambda function via OAC trigger → Lambda generates thumbnail and stores in another S3 bucket

**Summary:**
- Static content distributed using CloudFront with S3
- REST API is serverless; didn’t need Cognito because it’s public
- Leveraged a Global DynamoDB table to serve data globally (could also use Aurora Global DB)
- Enabled DynamoDB Streams to trigger a Lambda function
- Lambda function has IAM role to use SES for sending emails
- S3 can trigger SQS / SNS / Lambda to notify of events

### Microservices Architecture

- We want to switch to a microservice architecture
- Many services interact with each other directly using a REST API
- Each microservice architecture may vary in form and shape
- Goal: leaner development lifecycle for each service

**Microservices Environment:**
- `service1.example.com` → Amazon Route 53 DNS → Elastic Load Balancing → ECS → DynamoDB
- `service2.example.com` → Amazon API Gateway → AWS Lambda → ElastiCache
- `service3.example.com` → Elastic Load Balancing → Amazon EC2 Auto Scaling → Amazon RDS

**Discussion on Microservices:**
- Free to design each microservice the way you want
- Synchronous patterns: API Gateway, Load Balancers
- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)
- **Challenges with microservices:**
  - Repeated overhead for creating each new microservice
  - Issues with optimizing server density/utilization
  - Complexity of running multiple versions simultaneously
  - Proliferation of client-side code requirements for many separate services
- **Serverless patterns solve some challenges:**
  - API Gateway, Lambda scale automatically and you pay per usage
  - Easily clone API, reproduce environments
  - Generated client SDK through Swagger integration for API Gateway

### Software Updates Offloading

**Problem:**
- Application running on EC2 distributes software updates occasionally
- When a new software update is out, many requests arrive and content is distributed in mass over the network — very costly
- Don’t want to change the application; want to optimize cost and CPU

**Solution: Add CloudFront in Front of the ASG**
- CloudFront caches software update files at the edge
- Software update files are static (never changing)
- Our EC2/ASG doesn’t need to scale as much → save in EC2 costs
- Also save in availability, network bandwidth cost
- No architecture changes required — easy way to make an existing application more scalable and cheaper!
