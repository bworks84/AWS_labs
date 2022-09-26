\*unable to connect via SSH to EC2 instance. Reviewed EC2 security group inbound rules from tutorial since I was getting the timed out error. Then went back through the VPC settings for this lab, reviewed the security group settings, noticed I did not have public subnet explicit association. Once adding the public subnet, I was able to SSH into EC2 instance.

Today, attempted to recreate the RDS instance, EFS, and then proceeded to launch an EC2 instance with its own security group. Was able to SSH into the EC2 instance, however, unable to mount the EFS to the instance. I received a timeout error, which makes me think there's a security group network rule not configured properly.

Will try to run through the steps again tomorrow instead of trying to build on each other every day. Might help to recheck all security group rules.
