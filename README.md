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
    
    