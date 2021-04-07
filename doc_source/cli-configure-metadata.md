# Using credentials for Amazon EC2 instance metadata<a name="cli-configure-metadata"></a>

When you run the AWS CLI from within an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, you can simplify providing credentials to your commands\. Each Amazon EC2 instance contains metadata that the AWS CLI can directly query for temporary credentials\. When an IAM role is attached to the instance, the AWS CLI automatically and securely retrieves the credentials from the instance metadata\. 

## Prerequisites<a name="cli-configure-metadata-prereqs"></a>

To use Amazon EC2 credentials with the AWS CLI, you need to complete the following:
+ Launch the Amazon EC2 instance and confirm the AWS CLI is already installed\. If the AWS CLI is not installed, install the AWS CLI\. For more information, see [Installing the AWS CLI](cli-chap-install.md)\.
+ You understand configuration files\. For more information, see [Configuration and credential file settings](cli-configure-files.md)\. 
+ You understand named profiles\. For more information, see [Named profiles](cli-configure-profiles.md)\. 
+ You've created an AWS Identity and Access Management \(IAM\) role that has access to the resources needed, and attached that role to the Amazon EC2 instance when you launch it\. For more information, see [IAM policies for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html) in the *Amazon EC2 User Guide for Linux Instances* and [Granting Applications That Run on Amazon EC2 Instances Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in the *IAM User Guide*\.

## Configuring a profile for Amazon EC2 metadata<a name="cli-configure-metadata-configure"></a>

To specify that you want to use the credentials available in the hosting Amazon EC2 instance profile, use the following syntax in the in a named profile in your configuration file\. See the following steps for more instructions\. 

```
[profile profilename]
role_arn = arn:aws:iam::123456789012:role/rolename
credential_source = Ec2InstanceMetadata
region = region
```

1. Create a profile in your configuration file\.

   ```
   [profile profilename]
   ```

1. Add your IAM arn role that has access to the resources needed\.

   ```
   role_arn = arn:aws:iam::123456789012:role/rolename
   ```

1. Specify `Ec2InstanceMetadata` as your credential source\.

   ```
   credential_source = Ec2InstanceMetadata
   ```

1. Set your region\.

   ```
   region = region
   ```

**Example**

The following example assumes the *`marketingadminrole`* role and uses the `us-west-2` region in an Amazon EC2 instance profile named `marketingadmin`\.

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadminrole
credential_source = Ec2InstanceMetadata
region = us-west-2
```