## Amazon Route 53

### What is DNS?

- Domain Name System translates human-friendly hostnames into machine IP addresses
- `www.google.com` => `172.217.18.36`
- DNS is the backbone of the Internet
- DNS uses a hierarchical naming structure: `example.com`, `www.example.com`, `api.example.com`

### DNS Terminologies

| Term | Description |
| --- | --- |
| Domain Registrar | Amazon Route 53, GoDaddy, … |
| DNS Records | A, AAAA, CNAME, NS, … |
| Zone File | Contains DNS records |
| Name Server | Resolves DNS queries (Authoritative or Non-Authoritative) |
| Top Level Domain (TLD) | .com, .us, .in, .gov, .org, … |
| Second Level Domain (SLD) | amazon.com, google.com, … |

**FQDN (Fully Qualified Domain Name) breakdown:**
`http://api.www.example.com.`
- Protocol: `http://`
- Subdomain: `api.www`
- Second Level Domain: `example`
- Top Level Domain: `.com`
- Root: `.`

### How DNS Works

1. Browser asks Local DNS Server for `example.com`
2. Local DNS Server asks Root DNS Server (managed by ICANN)
3. Root DNS Server returns the TLD Name Server address
4. Local DNS Server asks TLD DNS Server (managed by IANA)
5. TLD DNS Server returns the SLD (authoritative) Name Server address
6. Local DNS Server asks SLD DNS Server (authoritative, managed by Domain Registrar)
7. SLD DNS Server returns the IP address `9.10.11.12`
8. Local DNS Server returns `9.10.11.12` to the browser

### Amazon Route 53

- A highly available, scalable, fully managed and **Authoritative** DNS
  - **Authoritative** = the customer (you) can update the DNS records
- Route 53 is also a Domain Registrar
- Ability to check the health of your resources
- The only AWS service which provides 100% availability SLA
- Why "Route 53"? 53 is a reference to the traditional DNS port

### Route 53 – Records

Each record contains:
- **Domain/subdomain Name** – e.g., `example.com`
- **Record Type** – e.g., A or AAAA
- **Value** – e.g., `12.34.56.78`
- **Routing Policy** – how Route 53 responds to queries
- **TTL** – amount of time the record is cached at DNS Resolvers

Route 53 supports the following DNS record types:
- **(must know)** A / AAAA / CNAME / NS
- **(advanced)** CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV

### Route 53 – Record Types

| Type | Maps |
| --- | --- |
| A | Hostname to IPv4 |
| AAAA | Hostname to IPv6 |
| CNAME | Hostname to another hostname (target must have A or AAAA record). Cannot be created for the root domain (Zone Apex). Example: can create for `www.example.com` but NOT for `example.com` |
| NS | Name Servers for the Hosted Zone. Controls how traffic is routed for a domain |

### Route 53 – Hosted Zones

- A container for records that define how to route traffic to a domain and its subdomains
- **Public Hosted Zones** – contain records that specify how to route traffic on the Internet (public domain names), e.g., `application1.mypublicdomain.com`
- **Private Hosted Zones** – contain records that specify how you route traffic within one or more VPCs (private domain names), e.g., `application1.company.internal`
- You pay $0.50 per month per hosted zone

### Route 53 – Records TTL (Time To Live)

- **High TTL** – e.g., 24 hours
  - Less traffic on Route 53 (cheaper)
  - Records may be outdated for longer
- **Low TTL** – e.g., 60 seconds
  - More traffic on Route 53 (costs more)
  - Records are outdated for less time
  - Easy to change records
- Except for Alias records, TTL is mandatory for each DNS record

### CNAME vs Alias

AWS Resources (Load Balancer, CloudFront, etc.) expose an AWS hostname like `lb1-1234.us-east-2.elb.amazonaws.com`, and you want to map `myapp.mydomain.com` to it.

**CNAME:**
- Points a hostname to any other hostname (`app.mydomain.com` => `blabla.anything.com`)
- **Only for non-root domains** (aka `something.mydomain.com`)
- Not free

**Alias:**
- Points a hostname to an AWS Resource (`app.mydomain.com` => `blabla.amazonaws.com`)
- Works for **ROOT DOMAIN and NON ROOT DOMAIN** (aka `mydomain.com`)
- Free of charge
- Native health check

### Route 53 – Alias Records

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resource’s IP addresses
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g., `example.com`
- Alias Record is always of type A/AAAA for AWS resources (IPv4/IPv6)
- You can’t set the TTL

**Alias Records Targets:**
- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator
- Route 53 record in the same hosted zone
- **Note:** You cannot set an ALIAS record for an EC2 DNS name

### Route 53 – Routing Policies

Route 53 supports the following Routing Policies:
- Simple
- Weighted
- Failover
- Latency-based
- Geolocation
- Multi-Value Answer
- Geoproximity (using Route 53 Traffic Flow feature)
- IP-based Routing

> **Note:** "Routing" here refers to how Route 53 responds to DNS queries, NOT routing traffic like a Load Balancer. DNS only responds to queries; it doesn’t route traffic.

### Routing Policies – Simple

- Typically routes traffic to a single resource
- Can specify multiple values in the same record
- If multiple values are returned, a **random one** is chosen by the client
- When Alias is enabled, specify only one AWS resource
- **Cannot** be associated with Health Checks

### Routing Policies – Weighted

- Control the percentage of requests that go to each specific resource
- Assign each record a relative weight:

$$\text{traffic (\%)} = \frac{\text{weight of specific record}}{\text{sum of all weights for all records}}$$

- Weights don’t need to sum up to 100
- DNS records must have the same name and type
- Can be associated with Health Checks
- **Use cases:** load balancing between regions, testing new application versions
- Assign a weight of 0 to a record to stop sending traffic to a resource
- If all records have weight 0, all records are returned equally

### Routing Policies – Latency-based

- Redirect to the resource that has the least latency close to the user
- Super helpful when latency for users is a priority
- Latency is based on traffic between users and AWS Regions
- Germany users may be directed to the US (if that’s the lowest latency)
- Can be associated with Health Checks (has a failover capability)

### Route 53 – Health Checks

- HTTP Health Checks are only for **public** resources
- **Health Check => Automated DNS Failover:**
  1. Health checks that monitor an endpoint (application, server, other AWS resource)
  2. Health checks that monitor other health checks (Calculated Health Checks)
  3. Health checks that monitor CloudWatch Alarms (full control) – e.g., throttles of DynamoDB, alarms on RDS, custom metrics (helpful for private resources)
- Health Checks are integrated with CW metrics

### Health Checks – Monitor an Endpoint

- About 15 global health checkers will check the endpoint health
- **Healthy/Unhealthy Threshold** – 3 (default)
- **Interval** – 30 sec (can set to 10 sec – higher cost)
- **Supported protocols:** HTTP, HTTPS, TCP
- If > 18% of health checkers report the endpoint is healthy, Route 53 considers it Healthy
- Health Checks pass only when the endpoint responds with 2xx and 3xx status codes
- Health Checks can be set up to pass/fail based on the text in the first 5,120 bytes of the response
- Configure your router/firewall to allow incoming requests from Route 53 Health Checkers IP ranges: https://ip-ranges.amazonaws.com/ip-ranges.json

### Route 53 – Calculated Health Checks

- Combine the results of multiple Health Checks into a single Health Check
- You can use OR, AND, or NOT
- Can monitor up to 256 Child Health Checks
- Specify how many of the health checks need to pass to make the parent pass
- **Usage:** perform maintenance on your website without causing all health checks to fail

### Health Checks – Private Hosted Zones

- Route 53 health checkers are outside the VPC
- They can’t access private endpoints (private VPC or on-premises resource)
- Solution: Create a CloudWatch Metric and associate a CloudWatch Alarm, then create a Health Check that checks the alarm itself

### Routing Policies – Failover (Active-Passive)

- Primary instance has a mandatory health check
- If primary is unhealthy, Route 53 automatically routes DNS requests to the secondary (Disaster Recovery) instance
- Client always gets the healthy resource

### Routing Policies – Geolocation

- Different from Latency-based!
- Routing is based on **user location**
- Specify location by Continent, Country, or by US State (most precise location is selected if there’s overlap)
- Should create a **"Default"** record (in case there’s no match on location)
- **Use cases:** website localization, restrict content distribution, load balancing
- Can be associated with Health Checks

### Routing Policies – Geoproximity

- Route traffic to your resources based on the geographic location of users and resources
- Ability to shift more traffic to resources based on the defined **bias**
- To change the size of the geographic region, specify bias values:
  - To **expand** (1 to 99) – more traffic to the resource
  - To **shrink** (-1 to -99) – less traffic to the resource
- Resources can be:
  - AWS resources (specify AWS region)
  - Non-AWS resources (specify Latitude and Longitude)
- You must use Route 53 Traffic Flow to use this feature

### Routing Policies – IP-based Routing

- Routing is based on clients’ IP addresses
- You provide a list of CIDRs for your clients and the corresponding endpoints/locations (user-IP-to-endpoint mappings)
- **Use cases:** Optimize performance, reduce network costs
- Example: route end users from a particular ISP to a specific endpoint

| CIDR Collection: Location | CIDR Blocks |
| --- | --- |
| location-1 | 203.0.113.0/24 |
| location-2 | 200.5.4.0/24 |

| Record Name | Value | IP-based |
| --- | --- | --- |
| example.com | 1.2.3.4 | location-1 |
| example.com | 5.6.7.8 | location-2 |

### Routing Policies – Multi-Value

- Use when routing traffic to multiple resources
- Route 53 returns multiple values/resources
- Can be associated with Health Checks (return only values for healthy resources)
- Up to 8 healthy records are returned for each Multi-Value query
- **Multi-Value is not a substitute for having an ELB**

### Domain Registrar vs. DNS Service

- You buy or register your domain name with a Domain Registrar by paying annual charges (e.g., GoDaddy, Amazon Registrar Inc.)
- The Domain Registrar usually provides you with a DNS service to manage your DNS records
- But you can use another DNS service to manage your DNS records
- **Example:** Purchase the domain from GoDaddy and use Route 53 to manage your DNS records

### 3rd Party Registrar with Amazon Route 53

If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider:
1. Create a Hosted Zone in Route 53
2. Update NS Records on the 3rd party website to use Route 53 Name Servers

> **Note:** Domain Registrar != DNS Service (but every Domain Registrar usually comes with some DNS features)

### Route 53 – Hybrid DNS

- By default, Route 53 Resolver automatically answers DNS queries for:
  - Local domain names for EC2 instances
  - Records in Private Hosted Zones
  - Records in public Name Servers
- **Hybrid DNS** – resolving DNS queries between VPC (Route 53 Resolver) and your networks (other DNS Resolvers)
- Networks can be:
  - VPC itself / Peered VPC
  - On-premises Network (connected through Direct Connect or AWS VPN)

### Route 53 – Resolver Endpoints

**Inbound Endpoint:**
- Allows your DNS Resolvers to resolve domain names for AWS resources (e.g., EC2 instances) and records in Private Hosted Zones
- On-premises DNS resolvers can query Route 53 for AWS resource names

**Outbound Endpoint:**
- Route 53 Resolver forwards DNS queries to your DNS Resolvers
- Used to resolve on-premises domain names from within the VPC
