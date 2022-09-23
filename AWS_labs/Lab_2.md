Create and Deploy RDS Instance

First, I had to recreate a VPC after tearing down the last lab. I used the new VPC console for quick setup of 2 private subnets and one public subnet, an IGW, and wired up the Route Table allowing internet traffic to the IGW.

Next, I created a security group for the MySQL DB instance I will be setting up and 'allowing' inbound traffic from the public subnet created in the VPC steps. Security groups act as a virtual firewall for EC2 or RDS Instances. They are stateful, which means they check or 'allow' inbound traffic and remember that traffic, therefor allow it to flow outbound as well.

Afterwards, I moved over to the RDS console, and first created a subnet group. A DB subnet group allows you to specify a particular VPC when you create DB instances using the AWS CLI or RDS API. If you use the console, you can just choose the VPC and subnets you want to use. Each DB subnet group must have at least one subnet in at least two Availability Zones in the AWS Region. (I used my two private subnets here)

Next, I launched an RDS instance with the db.t2.micro instance class. I also set a username and password for additional security on my database.

What is a DB subnet group?

What is multi-AZ? Is it used for increased capacity or availability? Are there different endpoints for each database instance in a multi-AZ configuration?

- In an Amazon RDS Multi-AZ deployment, Amazon RDS automatically creates a primary database (DB) instance and synchronously replicates the data to an instance in a different AZ. When it detects a failure, Amazon RDS automatically fails over to a standby instance without manual intervention. Multi-AZ refers to reliability and HA, rather than performance/capacity. When you set up multi-AZ, you still only have a single RDS endpoint. This endpoint is transparently failed-over to the standby DB instance in the event of a problem.

There was an option for configuring a parameter group. This option was left at its default value. Explain what a parameter group is.

- A DB Parameter Group is a set of configuration values for a database engine. Parameter groups are useful because they can be applied to multiple database instances. This saves time during initial configuration, and it also allows for changes to be made more quickly across an environment. For example: if a parameter must change, it can be changed across all of the RDS Instances that use the parameter group.

http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html

Do you have any access to the underlying operating system of an RDS instance?

- No. The underlying infrastructure for an RDS instance is entirely managed by AWS.

https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Maintenance.html
