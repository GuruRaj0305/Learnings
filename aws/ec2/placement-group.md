# Placement groups

+ Some times you want control over the EC2 
+ That strategy can be defined using placement groups.
+ when you create a placement group, you specify one of the following strategies : 
  + Cluster - cluster instances into a low-latency group in a single AZ
  + spread - spreads instances accross underlying hardware (max 7 instances per group per AZ)
  + Partition - spreads instances across many different partitions (which relay on different sets of racks ) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
  