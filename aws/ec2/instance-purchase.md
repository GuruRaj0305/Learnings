# EC2 Instance Purchasing Options

- **On-Demand Instance**: short workload, predictable pricing, pay by second
- **Reserved** (1 & 3 years):
  - Reserved Instance – long workloads
  - Convertible Reserved Instances – long workloads with flexible instances
- **Savings Plans** (1 & 3 years): commitment to an amount of usage, long workload
- **Spot Instances**: short workloads, cheap, can lose instance (less reliable)
- **Dedicated Hosts**: book an entire physical server, control instance placement
- **Dedicated Instances**: no other customers will share your hardware
- **Capacity Reservations**: reserve capacity in a specific AZ for any duration

## EC2 On-Demand

- Pay for what you use:
  - Linux or Windows: billing per second, after the first minute
  - All other operating systems: billing per hour
- Has the highest cost but no upfront payment
- No long-term commitment

## EC2 Reserved Instances

- Up to 72% discount compared to On-Demand
- Reserve specific instance attributes (Instance Type, Region, Tenancy, OS)
- Reservation Period: 1 year or 3 years
- Payment Options: No Upfront, Partial Upfront, All Upfront
- Reserved Instance's Scope: Regional or Zonal
- You can buy and sell in the Reserved Instance Marketplace
- **Convertible Reserved Instance**:
  - Can change the EC2 instance type, instance family, OS, scope and tenancy
  - Up to 66% discount

## EC2 Savings Plans

- Get a discount based on long-term usage (up to 72% – same as RIs)
- Commit to a certain type of usage (e.g., $10/hour for 1 or 3 years)
- Usage beyond EC2 Savings Plans is billed at the On-Demand price
- Locked to a specific instance family & AWS region
- Flexible across: Instance Size, OS, Tenancy

## EC2 Spot Instances

- Can get a discount of up to 90% compared to On-Demand
- Instances that you can **lose** at any point of time if your max price is less than the current spot price
  - The hourly spot price varies based on offer and capacity
  - If the current spot price > your max price, you can choose to stop or terminate your instance with a 2-minute grace period
- The most cost-efficient instance in AWS
- Useful for workloads that are resilient to failure:
  - Batch jobs
  - Data analysis
  - Image processing
  - Any distributed workloads
  - Workloads with a flexible start and end time
- **Not suitable** for critical jobs or databases

### How to Terminate Spot Instances

- You can only cancel spot instance requests that are **open**, **active**, or **disabled**
- Cancelling a spot request does **not** terminate instances (you must terminate the instances separately)
- You must first cancel a spot request, then terminate the associated spot instances

### Spot Fleets

- Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
- The Spot Fleet will try to meet the target capacity with price constraints
  - Define possible launch pools: instance type (e.g., m5.large), OS, Availability Zone
  - Can have multiple launch pools; the fleet can choose from them
  - Spot Fleet stops launching instances when reaching capacity or max cost
- Strategies to allocate Spot Instances:
  - **lowestPrice**: from the pool with the lowest price (cost optimization, short workload)
  - **diversified**: distributed across all pools (great for availability, long workloads)
  - **capacityOptimized**: pool with the optimal capacity for the number of instances
  - **priceCapacityOptimized** (recommended): pool with highest capacity available, then select the pool with the lowest price (best choice for most workloads)
- Spot Fleets allow us to automatically request Spot Instances with the lowest price

## EC2 Dedicated Hosts

- A physical server with EC2 instance capacity fully dedicated to your use
- Allows you to address compliance requirements and use your existing server-bound software licenses (per-socket, per-core, per-VM)
- Purchasing options:
  - **On-Demand**: pay per second for active Dedicated Host
  - **Reserved**: 1 or 3 years (No Upfront, Partial Upfront, All Upfront)
- The most expensive EC2 option
- Useful for software that has complicated licensing models (BYOL – Bring Your Own License)
- Or for companies that have strong regulatory or compliance needs

## EC2 Dedicated Instances

- Instances run on hardware that's dedicated to you
- May share hardware with other instances in the same account
- No control over instance placement (can move hardware after stop/start)

## EC2 Capacity Reservations

- Reserve On-Demand instance capacity in a specific AZ for any duration
- You always have access to EC2 capacity when you need it
- No time commitment (create/cancel anytime), no billing discounts
- Combine with Regional Reserved Instances and Savings Plans to benefit from billing discounts
- You are charged at the On-Demand rate whether you run instances or not
- Suitable for short-term, uninterrupted workloads that need to be in a specific AZ

