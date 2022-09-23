Today I built out two VPCs, one with the new console and one with the old interface.

The new console was helpful in quickly setting up the VPC, subnets, IGW, and route table and giving a visual representation of the VPC while doing so.

I also created a VPC following the same guidelines as the first, but with the old interface. I was more accustomed to this process and was able to go through and create the VPC almost as quickly.

However, this lab forced me to look up how to configure a few things I haven't done before such as 'Enabling DNS Hostnames' (Actions > Edit DNS Hostnames).

Creating the subnets was easier in the old format, because for this lab I only was tasked with building one public subnet and two private subnets (the new VPC console set up even numbers of subnets). I also configured IPv4 CIDR blocks for each subnet and placed one private subnet in US-East-1a, while placing the other private and public subnet in US-East-1b.

Next I created an IGW and then attached it to my VPC. Then I created a public route table for the VPC. I added a default route for all internet traffic with the IGW as the target. I also added a subnet association for the public subnet, but not the two private subnets.

1. Can a VPC be spread across multiple regions?

- No, a VPC can only be associated with 1 region at a time.

2. What is the largest allowed CIDR block for a VPC?

- /16

3. What does enabling "DNS Hostnames" do? Based on your knowledge of the environment that we are building, why might it be necessary?

- Enabling DNS hostnames allows instances in the VPC to receive public DNS hostnames. Enabling DNS support allows for the resolution of the private, Amazon-provided instance hostnames. Enabling this is necessary in the lab environment because several services will rely on DNS to query each other by hostname.

https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-dns.html#vpc-dns-hostnames

4. How many availability zones can a subnet be located in?

- A subnet can only be located in a single availability zone.

https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-subnet-basics

5. What are the allowed CIDR ranges of a subnet?

- /16 - /28
  https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#vpc-sizing-ipv4

6. How many IPs are reserved by AWS in a subnet? What are they used for?

- 5
- The first four IP addresses and the last IP address in each subnet CIDR block are not available for your use, and they cannot be assigned to a resource, such as an EC2 instance. For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved:

  10.0.0.0: Network address.

  10.0.0.1: Reserved by AWS for the VPC router.

  10.0.0.2: Reserved by AWS. The IP address of the DNS server is the base of the VPC network range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. We also reserve the base of each subnet range plus two for all CIDR blocks in the VPC. For more information, see Amazon DNS server.

  10.0.0.3: Reserved by AWS for future use.

  10.0.0.255: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address

https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html

7. What happens to a subnet if it is not associated with a specific routing table?

- The subnet is associated with the main routing table, which is created at the same time as the VPC.

https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#SubnetRouting

Building on the answer to the previous question: why is it a good idea to explicitly create a "public" routing table for the Internet Gateway association?

- Creating a "public" routing table ensures that only specifically authorized subnets are Internet accessible. New subnets must be manually placed into this separate routing table. While it would be possible to add an Internet or NAT gateway to the default routing table, this would result in newly created subnets being Internet accessible (or having Internet access) by default, and this might not be desired behavior.
