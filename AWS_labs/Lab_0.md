Created a new AWS Group for lab use.

Pretty straightforward, logged into my Admin account, utilizing the account login url from the root User account. Once in the admin account, I created a new Group and attached the following policies to it with full access:
VPC, EC2, EFS, RDS

Then created a new user 'labuser' and added the user to the new group, with access type 'AWS Management Console Access', no programmatic access.

Then logged into this new user's account using the sign-in link given upon creation. Once in the account I verified the access privileges by searching the 4 different services and ensuring I could use them (not seeing any errors either).

Questions:
What is the difference between programmatic access and AWS management console access? When would each type of access be appropriate?

- Programmatic access allows you to invoke actions on your AWS resources either through an application that you write or through a third-party tool. You use an access key ID and a secret access key to sign your requests for authorization to AWS. If resources need programmatic access and are running inside AWS, best practices is to use IAM roles. An IAM role is a defined set of permissions - it's not associated with a specific user or group. Instead any entity can assume the role to perform a specific business task.
- Essentially, AWS management console access would be appropriate when the above mentioned is not necessary. Normal or routine operations, not needing an app or third-party to do something....need to research this more.

Review the Policies page on the IAM console. What data format are policies written in?

- JSON
