# Getting credentials from EC2 instance metadata<a name="cli-configure-metadata"></a>

When you run the AWS CLI from within an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, you can simplify providing credentials to your commands\. Each Amazon EC2 instance contains metadata that the AWS CLI can directly query for temporary credentials\. To provide these, create an AWS Identity and Access Management \(IAM\) role that has access to the resources needed, and attach that role to the Amazon EC2 instance when you launch it\.

Launch the instance and check to see if the AWS CLI is already installed \(it comes preinstalled on Amazon Linux\)\. If necessary, install the AWS CLI\. You must still configure a default AWS Region to avoid having to specify it in every command\. 

In a named profile, to specify that you want to use the credentials available in the hosting Amazon EC2 instance profile, use the following line in the configuration file\. 

```
credential_source = Ec2InstanceMetadata 
```

The following example shows how to assume the `marketingadminrole` role by referencing it in an Amazon EC2 instance profile\.

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadminrole
credential_source = Ec2InstanceMetadata
```

To set the Region and default output format by running `aws configure` without specifying credentials, press **Enter** twice to skip the first two prompts\. 

```
$ aws configure
AWS Access Key ID [None]: ENTER
AWS Secret Access Key [None]: ENTER
Default region name [None]: us-west-2
Default output format [None]: json
```

When an IAM role is attached to the instance, the AWS CLI automatically and securely retrieves the credentials from the instance metadata\. For more information, see [Granting Applications That Run on Amazon EC2 Instances Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in the *IAM User Guide*\.