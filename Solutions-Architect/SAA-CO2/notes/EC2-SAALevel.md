## **AWS Solutions Architect Notes - EC2_SAA Level**

**Private vs Public IP (IPv4)**

- IPv4 allows for 3.7 billion different addresses in the public space
- IPv6 is newer and solves problems for the Internet of Things (IoT)

**Private vs Public IP (IPv4) Fundamental Differences**

- Public IP:
  - Public IP means the machine can be identified on the internet (WWW)
  - Must be unique across the whole web (not two machines can have the same public IP)
  - Can be geo-located easily
- Private IP:
  - Private IP means the machine can only be identified on a private network only
  - The IP must be unique across the private network
  - BUT two different private networks (two companies) can have the same IPs
  - Machines connect to WWW using a NAT + internet gateway (a proxy)
  - Only a specified range of IPs can be used as private IP

**Elastic IPs**

- With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another
  instance in your account
- You can only have 5 Elastic IP in your account (you can ask AWS to increase that)
- Overall, try to avoid using Elastic IP:
  - They often reflect poor architectural decisions
  - Instead, use a random public IP and register a DNS name to it 
  - Or; as we'll see later; use a Load Balancer and don't uas a public IP

- By default, your EC2 machine comes with:
  - A private IP for internal AWS Network
  - A public IP, for the WWW.
- When we are doing SSH into our EC2 machines:
  - We can't use a private IP, because we are not in the same network
  - We can only use the public IP.
- If your machine is stopped and then started, the public IP can change

**Placement Groups**

- Sometimes you want control over the EC2 Instance placement strategy
- That strategy can be defined using placement groups
- When you create a placement group, you specify one of the following strategies for the group:
  - Cluster - clusters instances into a low latency group in a single Availability Zone
  - Spread - spreads instances across underlying hardware (max 7 instances per group per AZ) - critical applications
  - Partition - spreads instances across many partitions (which rely on different sets of racks) within an AZ.
    Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)

**Placement Groups Cluster**

- Pros: Great network (10 Gbps bandwidth between instances)
- Cons: If the rack fails, all instances fail at the same time.
- Use case:
  - Big Data jobs that need to complete fast.
  - Application that needs extremely low latency and high network throughput

**Placement Groups Spread**

- Pros:
  - Can span across Availability Zones (AZ)
  - Reduced risk is simultaneous failure
  - EC2 Instances are on different physical hardware
- Cons:
  - Limited to 7 instances per AZ per placement group
- Use case:
  - Application that needs to maximize high availability
  - Critical Applications where each instance must be isolated form failure form each other

**Placement Groups Partition**

- Up to 7 partitions per AZ
- Can span across multiple AZs in the same region
- Up to 100s of EC2 instances
- The instances in a partition do not share racks with the instances in the other partitions
- A partition failure can affect many EC2 but won't affect other partitions
- EC2 instances get access to the partition information as metadata
- Use cases: HDFS, HBase, Cassandra, Kafka

**Elastic Network Interfaces(ENI)**

- Logical component in a VPC that represents a virtual network card
- The ENI can have the following attributes:
  - Primary private IPv4, one or more secondary IPv4
  - One elastic IP (IP4) per private IPv4
  - One Public IPv4
  - One or more security groups
  - A MAC address
- You can create ENI independently and attach them on the fly (move them) on EC2 instances for failover
- Bound to a specific availability zone (AZ)
- [Elastic Network Interfaces in the Virtual Private Cloud](https://aws.amazon.com/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/)

**EC2 Hibernate**

- We know we can stop, terminate instances
  - Stop: the data on disk (EBS) is kept intact in the next start
  - Terminate: any EBS volumes (root) also set-up to be destroyed is lost
- On start, the following happens: 
  - First start: the OS boots & the EC2 User Data script is run
  - Following starts: the OS boots up
  - Then your application starts, caches get warmed up, and that can take time!
- Introducing EC2 Hibernate: 
  - The in-memory (RAM) state is preserved 
  - The instance boot is much faster! (the OS is not stopped / restarted)
  - Under the hood: the RAM state is written to a file iun the root EBS volume
  - The root EBS volume must be encrypted 
- Use cases:
  - long-running processing
  - saving the RAM state 
  - services that take time to initialize 

**EC2 Nitro**

- Underlying Platform for the next generation of EC2 instances 
- New virtualization technology
- Allows for better performance:
  - better networking options (enhanced networking, HPC, IPv6)
  - Higher Speed EBS (Nitro is necessary for 64,000 EBS IOPS = max 32,000 on non-Nitro)
- Better underlying security
- Instance types example:
  - Virtualized: C5 etc..
  - Bare metal

**EC2 - Understanding vCPU**

- EC2 instances come with a combination of RAM and vCPU
- But in some cases, you may want to change the vCPU options:
  - /# of CPU cores: you can decrease it (helpful if you need high RAM and low number of
    CPU) -- to decrease licensing costs
  - /# of threads per core: disable multithreading to have 1 thread per CPU -- helpful for high performance computing
    (HPC) workloads
  - Only specified during instance launch

**EC2 - Capacity Reservations**

- Capacity Reservations ensure you have EC2 Capacity when needed
- Manual or planned end-date for the reservation
- No need for 1 or 3 year commitment
- Capacity access is immediate, you get billed as soon as it starts
- Specify:
  - The Availability Zone in which to reserve the capacity (only one)
  - The number of instances for which to reserve capacity
  - The instance attributes, including the instance type, tenancy, and platform/OS
- Combine and reserved instances and Saving Plans to do cost saving


**Questions**

1. You have launched an EC2 instance that will host a NodeJS application. After installing all the required software and configured your application, you noted down the EC2 instance public IPv4 so you can access it. Then, you stopped and then started your EC2 instance to complete the application configuration. After restart, you can't access the EC2 instance, and you found that the EC2 instance public IPv4 has been changed. What should you do to assign a fixed public IPv4 to your EC2 instance?
   - Allocate an Elastic IP and assign it to your EC2 instance
     - Elastic IP is a public IPv4 that you onw as long as you want and you can attach it to one EC2 instance at a time.
2. You want to run a batch job that requires a set of EC2 instances. This batch job should run for about 4 hours and must not be interrupted. Which EC2 Purchasing Option should you choose to run this batch job with the lowest cost?
   - Spot Block Instances
     - By using Spot Block Instances, you reserve a set of Spot EC2 instances for a specified duration (1-6 hours) without interruption.
3. Spot Fleet is a set of Spot Instances and optionally ...............
   - On-Demand Instances
     - Spot Fleet is a set of Spot Instances and optionally On-demand Instances. It allows you to automatically request Spot Instances with the lowest price.
4. You have an application performing big data analysis hosted on a fleet of EC2 instances. You want to ensure your EC2 instances have the highest networking performance while communicating with each other. Which EC2 Placement Group should you choose?
   - Cluster Placement Group
     - Cluster Placement Groups place your EC2 instances next to each other which gives you high-performance computing and networking.
5. You have a critical application hosted on a fleet of EC2 instances in which you want to achieve maximum availability when there's an AZ failure. Which EC2 Placement Group should you choose?
   - Spread Placement Group
     - Spread Placement Group places your EC2 instances on different physical hardware across different AZs.
6. Elastic Network Interface (ENI) can be attached to EC2 instances in another AZ.
   - False
     - Elastic Network Interfaces (ENIs) are bounded to a specific AZ. You can not attach an ENI to an EC2 instance in a different AZ.
7. The following are true regarding EC2 Hibernate, EXCEPT:
   - EC2 Instance Root Volume must be an Instance Store volume.
     - To enable EC2 Hibernate, the EC2 Instance Root Volume type must be an EBS volume and must be encrypted to ensure the protection of sensitive content.