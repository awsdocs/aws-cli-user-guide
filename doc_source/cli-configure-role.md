# Assuming an IAM Role in the AWS CLI<a name="cli-configure-role"></a>

An [AWS Identity and Access Management \(IAM\) role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an authorization tool that lets an IAM user gain additional \(or different\) permissions, or get permissions to perform actions in a different AWS account\. 

You can configure the AWS Command Line Interface \(AWS CLI\) to use an IAM role by defining a profile for the role in the `~/.aws/config` file\. 

The following example shows a role profile named `marketingadmin` that is assumed when you run commands that specify the `marketingadmin` profile\.

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadmin
source_profile = user1
```

You must link the role to a separate named profile that contains IAM user credentials with permission to assume the role\. In the previous example, the `marketingadmin` profile is linked using the `source-profile` field to the `user1` profile\. When you specify that an AWS CLI command is to use the profile `marketingadmin`, the CLI automatically looks up the credentials for the linked `user1` profile and uses them to request temporary credentials for the specified IAM role\. Those temporary credentials are then used to run the CLI command\. The specified role must have attached IAM permission policies that allow the CLI command to run\.

If you want to run a CLI command from within an Amazon EC2 instance, you can use an IAM role attached to an Amazon EC2 instance profile or an Amazon ECS container role\. This enables you to avoid storing long\-lived access keys on your instances\. In that case you could replace the `source_profile` attribute with `credential_source` and specify how to find the credentials\. This attribute supports the following values:
+ `Environment` – to retrieve the credentials from environment variables\.
+ `Ec2InstanceMetadata` – to use the attached Amazon EC2 instance role\.
+ `EcsContainer` – to use the Amazon ECS container credentials\.

The following example shows the same `marketingadmin` role assumed by referencing an Amazon EC2 instance profile:

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadmin
credential_source = Ec2InstanceMetadata
```

For more information, see [AWS CLI Configuration Variables](https://docs.aws.amazon.com/cli/latest/topic/config-vars.html)\. 

**Topics**
+ [Configuring and Using a Role](#cli-role-prepare)
+ [Using Multi\-Factor Authentication](#cli-configure-role-mfa)
+ [Cross\-Account Roles](#cli-configure-role-xaccount)
+ [Clearing Cached Credentials](#cli-configure-role-cache)

## Configuring and Using a Role<a name="cli-role-prepare"></a>

When you run commands using a profile that specifies an IAM role, the AWS CLI uses the source profile's credentials to call AWS Security Token Service \(AWS STS\) and request temporary credentials for the specified role\. The user in the source profile must have permission to call `sts:assume-role` for the role in the specified profile\. The role must have a trust relationship that allows the user in the source profile to assume the role\. The process of retrieving and then using temporary credentials for a role is often referred to as *assuming the role*\.

You can create a new role in IAM with the permissions that you want users to assume by following the procedure under [Creating a Role to Delegate Permissions to an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-creatingrole-user.html) in the *AWS Identity and Access Management User Guide*\. If the role and the source profile's IAM user are in the same account, you can enter your own account ID when configuring the role's trust relationship\.

After creating the role, modify the trust relationship to allow the IAM user \(or the users in the AWS account\) to assume it\. 

The following example shows a trust relationship that allows a role to be assumed by any IAM user in the account 123456789012, ***if*** the administrator of that account explicitly grants the `sts:assumerole` permission to the user\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement": [
 4.     {
 5.       "Effect": "Allow",
 6.       "Principal": {
 7.         "AWS": "arn:aws:iam::123456789012:root"
 8.       },
 9.       "Action": "sts:AssumeRole"
10.     }
11.   ]
12. }
```

The trust policy doesn't actually grant permissions\. The administrator of the account must delegate the permission to assume the role to individual users by attaching a policy with the appropriate permissions\. The following example allows the attached IAM user to assume only the `marketingadmin` role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::123456789012:role/marketingadmin"
    }
  ]
}
```

The IAM user doesn't need to have any additional permissions to run the CLI commands using the role profile\. Instead, the permissions needed to run the command come from those attached to the *role*\. However, to enable your users to access AWS resources *without* using a role, you must attach additional inline or managed policies to the IAM user that grants permissions for those resources\.

Now that you have the role profile, role permissions, role trust relationship, and user permissions properly configured, you can use the role at the command line by invoking the `--profile` option\. For example, the following command calls the Amazon S3 `ls` command using the permissions attached to the `marketingadmin` role as defined by the example at the beginning of this topic\.

```
$ aws s3 ls --profile marketingadmin
```

To use the role for several calls, you can set the `AWS_PROFILE` environment variable for the current session from the command line\. While that environment variable is defined, you don't have to specify the `--profile` option on each command\. 

**Linux, macOS, or Unix**

```
$ export AWS_PROFILE=marketingadmin
```

**Windows**

```
C:\> set AWS_PROFILE=marketingadmin
```

For more information on configuring IAM users and roles, see [Users and Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html) and [Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html) in the *IAM User Guide*\.

## Using Multi\-Factor Authentication<a name="cli-configure-role-mfa"></a>

For additional security, you can require that users provide a one\-time key generated from a multi\-factor authentication \(MFA\) device, a U2F device, or mobile app when they attempt to make a call using the role profile\.

First, modify the trust relationship on the IAM role to require MFA\. For an example, see the `Condition` line in the following example\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::123456789012:user/jonsmith" },
      "Action": "sts:AssumeRole",
      "Condition": { "Bool": { "aws:multifactorAuthPresent": true } }
    }
  ]
}
```

Next, add a line to the role profile that specifies the ARN of the user's MFA device\.

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadmin
source_profile = default
mfa_serial = arn:aws:iam::123456789012:mfa/saanvi
```

The `mfa_serial` setting can take an ARN, as shown, or the serial number of a hardware MFA token\.

## Cross\-Account Roles<a name="cli-configure-role-xaccount"></a>

You can enable IAM users to assume roles that belong to different accounts by configuring the role as a cross\-account role\. During role creation, set the role type to **Another AWS account**, as described in [Creating a Role to Delegate Permissions to an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\. Optionally, select **Require MFA**\. The **Require MFA** option configures the appropriate condition in the trust relationship, as described in [Using Multi\-Factor Authentication](#cli-configure-role-mfa)\.

If you use an [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) to provide additional control over who can assume a role across accounts, you must also add the `external_id` parameter to the role profile\. You typically use this only when the other account is controlled by someone outside your company or organization\.

```
[profile crossaccountrole]
role_arn = arn:aws:iam::234567890123:role/SomeRole
source_profile = default
mfa_serial = arn:aws:iam::123456789012:mfa/saanvi
external_id = 123456
```

For more information, see [AWS CLI Configuration Variables](https://docs.aws.amazon.com/cli/latest/topic/config-vars.html)\. 

## Clearing Cached Credentials<a name="cli-configure-role-cache"></a>

When you assume a role, the AWS CLI caches the temporary credentials locally until they expire\. If your role's temporary credentials are [revoked](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html), you can delete the cache to force the AWS CLI to retrieve new credentials\.

**Linux, macOS, or Unix**

```
$ rm -r ~/.aws/cli/cache
```

**Windows**

```
C:\> del /s /q %UserProfile%\.aws\cli\cache
```