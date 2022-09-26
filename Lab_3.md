Create an EFS

Today I added an Elastic File System to my application lab. Elastic File System (Amazon EFS) provides a simple, serverless, set-and-forget elastic file system for use with AWS Cloud services and on-premises resources. The service is designed to be highly scalable, highly available, and highly durable. Amazon EFS file systems using Standard storage classes store data and metadata across multiple Availability Zones in an AWS Region. EFS file systems can grow to petabyte scale, drive high levels of throughput, and allow massively parallel access from compute instances to your data.

Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol, so the applications and tools that you use today work seamlessly with Amazon EFS.

Amazon EFS supports authentication, authorization, and encryption capabilities to help you meet your security and compliance requirements. Amazon EFS supports two forms of encryption for file systems, encryption in transit and encryption at rest.

First I created the security group to allow NFS traffic. I edited the inbound rules for the security group to allow NFS traffic from the public subnet created in the VPC lab (10.10.100.0/24).

Then I moved over to the EFS service via the console. Created a new EFS, selecting my lab VPC, the security group just created, gave it a name, and left the default performance mode on.

I did have one problem on this lab. When connecting the mount targets I was unable to figure out how to connect the EFS system to the 2nd private subnet. The EFS had both the public and private subnet from us-east-1a hooked up, so I deleted the public subnet mount target and attempted to create a new mount target to the other private subnet using the subnet ID. I hit a snag with the IP address for this subnet. Will need to look into further....

\*\*\*Update: took another shot at this one today. Rechecked configuration of VPC, subnets, and route table. Then recreated EFS and addressed mount target issue from above. Realized it did not have to do with IP address.

- Started by changing the security group to the newly created one in this lab for the private subnet in us-east-1b that was listed in the EFS>Networking tab.
- Then, removed the mount target for the public subnet in us-east-1a.
- Added a new mount target with second private subnet in the custom VPC by using the subnet id, IP address was defaulted to "automatic", and then selected the default security group.
- After this mount target was successfully created, I went back and changed the security group from the default security group to the one created above that allows traffic from the public subnet. (10.10.100.0/24)

Questions:

1.  What version(s) of NFS is/are supported by EFS?

- Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol, so the applications and tools that you use today work seamlessly with Amazon EFS.

What makes EFS unique when compared to other types of storage? Why are we using EFS in this scenario? Why are other types of storage, such as S3 or EBS, inappropriate for this scenario?

- EFS is unique because it can be mounted to multiple EC2 instances, unlike EBS volumes which can only be mounted to one instance at a time.

EFS is useful in our scenario because it allows us to have a mountable filesystem across multiple EC2 instances. Our topology uses shared storage for the backend web code. This would not be possible using EBS, as we can only have an EBS volume attached to a single instance. While we could probably write code to use the S3 API to constantly download and refresh our backend web code, that would not be ideal. S3 isn't a mountable filesystem.

What types of storage performance are available?

- AWS offers two types of storage performance: General Purpose and Max I/O. General purpose storage is the default, and is optimized for latency-sensitive workloads. Max I/O is optimized for higher aggregate throughput and operations, but will experience slightly higher latency.

http://docs.aws.amazon.com/efs/latest/ug/performance.html#performancemodes

What is a mount target?

- A mount target is an endpoint for NFS connectivity to an EFS. Mount targets are IP addresses in an availability zone. Instances within that availability zone connect to the mount target via NFS to access the EFS. NFS mounting is done using the DNS name of the mount target.

http://docs.aws.amazon.com/efs/latest/ug/how-it-works.html
