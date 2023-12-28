# AWS Certified Solutions Architect Associate SAA-C03

## **Amazon EC2**
+ EC2 is one of the most popular of AWS offering 
+ EC2 = Elastic Compute Cloud = Infrastructure as a Service
+ It mainly consists in the capability of :
    + Renting virtual machines (EC2)
    + Storing data on virtual drives (EBS)
    + Distributing load across machines (ELB)
    + Scaling the services using an auto-scaling group (ASG)
+ Knowing EC2 is fundamental to understand how the Cloud works

#### EC2 sizing & configuration options
+ Operating System (**OS**): Linux, Windows or Mac OS
+ How much compute power & cores (**CPU**)
+ How much random-access memory (**RAM**)
+ How much storage space:
    + Network-attached (EBS & EFS)
    + hardware (EC2 Instance Store)
+ Network card: speed of the card, Public IP address
+ Firewall rules: **security group**
+ Bootstrap script (configure at first launch): EC2 User Data

#### EC2 User Data
+ It is possible to bootstrap our instances using an EC2 User data script.
+ bootstrapping means launching commands when a machine starts
+ That script is only run once at the instance first start
+ EC2 user data is used to automate boot tasks such as:
    + Installing updates
    + Installing software
    + Downloading common files from the internet
    + Anything you can think of
+ The EC2 User Data Script runs with the root user
    
    ##### Example of scripts
    ```shell scripts
    #1/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    sudo yum update -y
    sudo yum install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
    sudo chmod a+w /var/www/html
    sudo echo "<h1>Hello Welcome, $(hostname -f)</h1>" > /var/www/html/index.html
    
    ```
## EC2 Instance Types - Overview
 + You can use different types of EC2 instances that are optimised for 
different use cases (https://aws.amazon.com/ec2/instance-types/)

 + AWS has the following naming convention:
    > m5.2xlarge
 + m: instance class
 + 5: generation (AWS improves them over time)   
 + 2xlarge: size within the instance class

## EC2 Instance Types – General Purpose
+ Great for a diversity of workloads such as web servers or code repositories
+ Balance between:
    + Compute
    + Memory
    + Networking
+ In the course, we will be using the t2.micro which is a General Purpose EC2 
instance

## EC2 Instance Types – Compute Optimized
+ Great for compute-intensive tasks that require high performance 
processors:
    + Batch processing workloads
    + Media transcoding
    + High performance web servers
    + High performance computing (HPC)
    + Scientific modeling & machine learning
    + Dedicated gaming servers

## EC2 Instance Types – Memory Optimized
+ • Fast performance for workloads that process large data sets in memory
+ Use cases: 
    + High performance, relational/non-relational databases
    + Distributed web scale cache stores
    + In-memory databases optimized for BI (business intelligence)
    + Applications performing real-time processing of big unstructured data

## EC2 Instance Types – Storage Optimized
+ Great for storage-intensive tasks that require high, sequential read and write 
access to large data sets on local storage
+ Use cases:
    + High frequency online transaction processing (OLTP) systems
    + Relational & NoSQL databases
    + Cache for in-memory databases (for example, Redis)
    + Data warehousing applications
    + Distributed file systems
### EC2 Instance Types: example
| Instance    | vCPU | Mem (GiB) | Storage          | Network Performance | EBS Bandwidth (Mbps) |
|-------------|------|-----------|------------------|---------------------|----------------------|
| t2.micro    | 1    | 1         | EBS-Only         | Low to Moderate     |                      |
| t2.xlarge   | 4    | 16        | EBS-Only         | Moderate            |                      |
| c5d.4xlarge | 16   | 32        | 1 x 400 NVMe SSD | Up to 10 Gbps       | 4,750                |
| r5.16xlarge | 64   | 512       | EBS-Only         | 20 Gbps             | 13,600               |
| m5.8xlarge  | 32   | 128       | EBS-Only         | 10 Gbps             | 6,800                |

> t2.micro is part of the AWS free tier (up to 750 hours per month)

# Introduction to Security Groups
+ Security Groups are the fundamental of network security in AWS
+ They control how traffic is allowed into or out of our EC2 Instances.
![Security Groups](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/Security%20Groups/Security%20Groups.PNG?raw=true "Security Groups")
+ Security groups only contain rules
+ Security groups rules can reference by IP or by security group

## Security Groups Deeper Dive
+ Security groups are acting as a “firewall” on EC2 instances
+ They regulate:
    + Access to Ports
    + Authorised IP ranges – IPv4 and IPv6
    + Control of inbound network (from other to the instance)
    + Control of outbound network (from the instance to other)

![Security Groups Deeper Dive](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/98fa105de06e6e969dbf9d629fe7887aea53973a/Security%20Groups/Security-Groups-Deeper-Dive.PNG?raw=true "Security Groups Deeper Dive")

## Security Groups Diagram
![Security Groups Diagram](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/98fa105de06e6e969dbf9d629fe7887aea53973a/Security%20Groups/Security-Group-Diagram.PNG?raw=true "Security Group Diagram")

## Security Groups Good to know
+ Can be attached to multiple instances
+ Locked down to a region / VPC combination
+ Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see it
+ >It’s good to maintain one separate security group for SSH access
+ If your application is not accessible (time out), then it’s a security group issue
+ If your application gives a “connection refused“ error, then it’s an application 
error or it’s not launched
+ All inbound traffic is **blocked** by default
+ All outbound traffic is **authorised** by default

## Referencing other security groups Diagram
![Referencing other security groups
Diagram](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/Security%20Groups/Referencing-other-security-groups.PNG?raw=true "Referencing other security groups
Diagram")

## Classic Ports to know
+ 22 = SSH (Secure Shell) - log into a Linux instance
+ 21 = FTP (File Transfer Protocol) – upload files into a file share
+ 22 = SFTP (Secure File Transfer Protocol) – upload files using SSH
+ 80 = HTTP – access unsecured websites
+ 443 = HTTPS – access secured websites
+ 3389 = RDP (Remote Desktop Protocol) – log into a Windows instance

## SSH Summary Table
![SSH Summary Table](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/ssh/SSH-summary-table.PNG?raw=true "SSH Summary Table")

## How to SSH into your EC2 Instance Linux / Mac OS X

![SSH Connect](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/ssh/ssh-connect-ec-instance-linux-macos.PNG?raw=true "SSH Connect")

## EC2 Instances Purchasing Options
+ **On-Demand Instances** – short workload, predictable pricing, pay by second
+ **Reserved** (1 & 3 years)
    + **Reserved Instances** – long workloads
    + **Convertible Reserved Instances** – long workloads with flexible instances
+ **Savings Plans** (1 & 3 years) –commitment to an amount of usage, long workload
+ **Spot Instances** – short workloads, cheap, can lose instances (less reliable)
+ **Dedicated Hosts** – book an entire physical server, control instance placement
+ **Dedicated Instances** – no other customers will share your hardware
+ **Capacity Reservations** – reserve capacity in a specific AZ for any duration

## EC2 On Demand
+ Pay for what you use:
    + Linux or Windows - billing per second, after the first minute
    + All other operating systems - billing per hour
+ Has the highest cost but no upfront payment
+ No long-term commitment
+ Recommended for **short-term** and **un-interrupted workloads**, where you can't predict how the application will behave


## EC2 Reserved Instances
+ Up to 72% discount compared to On-demand
+ You reserve a specific instance attributes (**Instance Type, Region, Tenancy, OS**)
+ **Reservation Period – 1 year** (+discount) or **3 years** (+++discount)
+ **Payment Options – No Upfront (+), Partial Upfront (++), All Upfront (+++)**
+ **Reserved Instance’s Scope – Regional or Zonal** (reserve capacity in an AZ)
+ Recommended for steady-state usage applications (think database)
+ You can buy and sell in the Reserved Instance Marketplace
+ **Convertible Reserved Instance**
    + Can change the EC2 instance type, instance family, OS, scope and tenancy
    + Up to 66% discount

## EC2 Savings Plans
+ Get a discount based on long-term usage (up to 72% - same as RIs)
+ Commit to a certain type of usage ($10/hour for 1 or 3 years)
+ Usage beyond EC2 Savings Plans is billed at the On-Demand price
+ Locked to a specific instance family & AWS region (e.g., M5 in us-east-1)
+ Flexible across:
    + Instance Size (e.g., m5.xlarge, m5.2xlarge)
    + OS (e.g., Linux, Windows)
    + Tenancy (Host, Dedicated, Default)

## EC2 Spot Instances
+ Can get a **discount of up to 90%** compared to On-demand
+ Instances that you can “lose” at any point of time if your max price is less than the 
current spot price
+ The **MOST cost-efficient** instances in AWS
+ Useful for workloads that are resilient to failure
    + Batch jobs
    + Data analysis
    + Image processing
    + Any **distributed** workloads
    + Workloads with a flexible start and end time
+ **Not suitable for critical jobs or databases**

## EC2 Dedicated Hosts
+ A physical server with EC2 instance capacity fully dedicated to your use
+ Allows you address **compliance requirements** and **use your existing server- bound software licenses** (per-socket, per-core, pe—VM software
+ Purchasing Options:
    + **On-demand** – pay per second for active Dedicated Host
    + **Reserved** - 1 or 3 years (No Upfront, Partial Upfront, All Upfront)
+ The most expensive option
+ Useful for software that have complicated licensing model (BYOL – Bring Your Own License)
+ Or for companies that have strong regulatory or compliance needs

## EC2 Dedicated Instances
+ Instances run on hardware that’s 
dedicated to you
+  May share hardware with other 
instances in same account
+ No control over instance placement 
(can move hardware after Stop / Start)

## EC2 Capacity Reservations
+ Reserve **On-Demand** instances capacity in a specific AZ for any duration
+ You always have access to EC2 capacity when you need it
+ **No time commitment** (create/cancel anytime), **no billing discounts**
+ Combine with Regional Reserved Instances and Savings Plans to benefit from billing discounts
+ You’re charged at On-Demand rate whether you run instances or not
+ Suitable for short-term, uninterrupted workloads that needs to be in a specific AZ

## Which purchasing option is right for me?
+ **On demand:** coming and staying in resort
+ **Reserved:** like planning ahead and if we plan to stay for a long time, we may get a good discount.
+ **Savings Plans:** pay a certain amount per hour for certain period and stay in any room type (e.g., 
King, Suite, Sea View, …)
+ **Spot instances:** the hotel allows people to bid for the empty rooms and the highest bidder keeps the 
rooms. You can get kicked out at any time
+ **Dedicated Hosts:** We book an entire building of the resort.
+ **Capacity Reservations:** you book a room for a period with full price even you don’t stay

## Price Comparison Example – m4.large – us-east-1
|**Price Type**      |  **Price (per hour)**           |
| ------------- |:-------------:|
| On-Demand     | $0.10  |
| Spot Instance (Spot Price)     | $0.038 - $0.039 (up to 61% off) |
| Reserved Instance (1 year)     | $0.062 (No Upfront) - $0.058 (All Upfront)  |
| Reserved Instance (3 years)    | $0.043 (No Upfront) - $0.037 (All Upfront)  |
| EC2 Savings Plan (1 year)     | $0.062 (No Upfront) - $0.058 (All Upfront) |
| Reserved Convertible Instance (1 year)     | $0.071 (No Upfront) - $0.066 (All Upfront)  |
| Dedicated Host     | On-Demand Price  |
| Dedicated Host Reservation     | Up to 70% off |
| Capacity Reservations     | On-Demand Price
  |

## EC2 Spot Instance Requests
+ Can get a discount of up to 90% compared to On-demand
+ Define **max spot price** and get the instance while **current spot price < max**
    + The hourly spot price varies based on offer and capacity
    + If the current spot price > your max price you can choose to **stop** or **terminate** your instance with a 2 minutes grace period.
+ Other strategy: **Spot Block**
+ **Used for batch jobs, data analysis, or workloads that are resilient to failures.**
+ **Not great for critical jobs or databases**

## EC2 Spot Instances Pricing
![EC2 Spot Instances Pricing](![Alt text](image.png) "EC2 Spot Instances Pricing")

## How to terminate Spot Instances?
![How to terminate Spot Instances?](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/ec2/How_to_terminate_Spot_Instances.PNG?raw=true "How to terminate Spot Instances?")

## Spot Fleets
+ Spot Fleets = set of Spot Instances + (optional) On-Demand Instances
+ The Spot Fleet will try to meet the target capacity with price constraints
    + Define possible launch pools: instance type (m5.large), OS, Availability Zone
    + Can have multiple launch pools, so that the fleet can choose
    + Spot Fleet stops launching instances when reaching capacity or max cost
+ Strategies to allocate Spot Instances:
    + **lowestPrice:** from the pool with the lowest price (cost optimization, short workload)
    + **diversified:** distributed across all pools (great for availability, long workloads)
    + **capacityOptimized:** pool with the optimal capacity for the number of instances
    + **priceCapacityOptimized (recommended):** pools with highest capacity available, then select 
the pool with the lowest price (best choice for most workloads)
+ Spot Fleets allow us to automatically request Spot Instances with the lowest price

## Private vs Public IP (IPv4)
+ Networking has two sorts of IPs. IPv4 and IPv6:
    + IPv4: 1.160.10.240
    + IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
+ In this course, we will only be using IPv4.
+ IPv4 is still the most common format used online.
+ IPv6 is newer and solves problems for the Internet of Things (IoT).
+ IPv4 allows for 3.7 billion different addresses in the public space
+ IPv4: [0-255].[0-255].[0-255].[0-255].

    ### Private vs Public IP (IPv4) Example
    ![Private vs Public IP (IPv4)](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/private_vs_public_network/private_public_ip_example.PNG?raw=true "Private vs Public IP (IPv4)")

    ### Private vs Public IP (IPv4) Fundamental Differences

    +   Public IP:
        + Public IP means the machine can be identified on the internet (WWW)
        + Must be unique across the whole web (not two machines can have the same public IP).
        + Can be geo-located easily
    + Private IP:
        + Private IP means the machine can only be identified on a private network only
        + The IP must be unique across the private network
        + BUT two different private networks (two companies) can have the same IPs.
        + Machines connect to WWW using a NAT + internet gateway (a proxy)
        + Only a specified range of IPs can be used as private IP

    ### Elastic IPs
    + When you stop and then start an EC2 instance, it can change its public 
    IP
    + If you need to have a fixed public IP for your instance, you need an Elastic IP
    + An Elastic IP is a public IPv4 IP you own as long as you don’t delete it
    + You can attach it to one instance at a time

    ### Elastic IP
    + With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
    + You can only have 5 Elastic IP in your account (you can ask AWS to increase that).
    + Overall, **try to avoid using Elastic IP:**
        + They often reflect poor architectural decisions
        + Instead, use a random public IP and register a DNS name to it
        + Or, as we’ll see later, use a Load Balancer and don’t use a public IP

## Placement Groups
+ Sometimes you want control over the EC2 Instance placement strategy
+ That strategy can be defined using placement groups
+ When you create a placement group, you specify one of the following strategies for the group
    + **Cluster**—clusters instances into a low-latency group in a single Availability Zone
    + **Spread**—spreads instances across underlying hardware (max 7 instances per group per AZ)
    + **Partition**—spreads instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)

    ### Placement Groups Cluster
    ![Placement Groups Cluster](https://github.com/AvinashSharma1/aws-solutions-architect-associate-SAA-C03-notes/blob/master/placement-group/placement-group-cluster.PNG?raw=true "Placement Groups Cluster")
    + Pros: Great network (10 Gbps bandwidth between instances with Enhanced Networking enabled - recommended)
    + Cons: If the rack fails, all instances fails at the same time
    + Use case:
        + Big Data job that needs to complete fast
        + Application that needs extremely low latency and high network throughput
    ### Placement Groups Spread
        

