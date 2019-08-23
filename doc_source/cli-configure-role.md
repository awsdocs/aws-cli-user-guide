# Using an IAM Role in the AWS CLI<a name="cli-configure-role"></a>

An [AWS Identity and Access Management \(IAM\) role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an authorization tool that lets an IAM user gain additional \(or different\) permissions, or get permissions to perform actions in a different AWS account\. 

You can configure the AWS Command Line Interface \(AWS CLI\) to use an IAM role by defining a profile for the role in the `~/.aws/credentials` file\. 

The following example shows a role profile named `marketingadmin`\. If you run commands with `--profile marketingadmin` \(or specify it with the [AWS\_PROFILE environment variable](cli-configure-envvars.md)\), then the CLI uses the credentials defined in the profile `user1` to assume the role with the Amazon Resource Name \(ARN\) `arn:aws:iam::123456789012:role/marketingadminrole`\. You can run any operations that are allowed by the permissions assigned to that role\.

```
[marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadminrole
source_profile = user1
```

You must specify a `source_profile` that points to a separate named profile that contains IAM user credentials with permission to use the role\. In the previous example, the `marketingadmin` profile uses the credentials in the `user1` profile\. When you specify that an AWS CLI command is to use the profile `marketingadmin`, the CLI automatically looks up the credentials for the linked `user1` profile and uses them to request temporary credentials for the specified IAM role\. The CLI uses the [sts:AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) operation in the background to accomplish this\. Those temporary credentials are then used to run the requested CLI command\. The specified role must have attached IAM permission policies that allow the requested CLI command to run\.

If you want to run a CLI command from within an Amazon EC2 instance or an Amazon ECS container, you can use an IAM role attached to the instance profile or the container\. If you specify no profile or set no environment variables, that role is used directly\. This enables you to avoid storing long\-lived access keys on your instances\. You can also use those instance or container roles only to get credentials for another role\. To do this, you use `credential_source` \(instead of `source_profile`\) to specify how to find the credentials\. The `credential_source` attribute supports the following values:
+ `Environment` – to retrieve the source credentials from environment variables\.
+ `Ec2InstanceMetadata` – to use the IAM role attached to the Amazon EC2 instance profile\.
+ `EcsContainer` – to use the IAM role attached to the Amazon ECS container\.

The following example shows the same `marketingadminrole` role used by referencing an Amazon EC2 instance profile:

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadminrole
credential_source = Ec2InstanceMetadata
```

When you invoke a role, you have additional options that you can require, such as the use of multi\-factor authentication, an External ID \(used by third party companies to access their clients' resources\), and specify unique role session names that can be more easily audited in AWS CloudTrail logs\.

**Topics**
+ [Configuring and Using a Role](#cli-role-prepare)
+ [Using Multi\-Factor Authentication](#cli-configure-role-mfa)
+ [Cross\-Account Roles & External ID](#cli-configure-role-xaccount)
+ [Specifying a Role Session Name for Easier Auditing](#cli-configure-role-session-name)
+ [Clearing Cached Credentials](#cli-configure-role-cache)

## Configuring and Using a Role<a name="cli-role-prepare"></a>

When you run commands using a profile that specifies an IAM role, the AWS CLI uses the source profile's credentials to call AWS Security Token Service \(AWS STS\) and request temporary credentials for the specified role\. The user in the source profile must have permission to call `sts:assume-role` for the role in the specified profile\. The role must have a trust relationship that allows the user in the source profile to use the role\. The process of retrieving and then using temporary credentials for a role is often referred to as *assuming the role*\.

You can create a new role in IAM with the permissions that you want users to assume by following the procedure under [Creating a Role to Delegate Permissions to an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-creatingrole-user.html) in the *AWS Identity and Access Management User Guide*\. If the role and the source profile's IAM user are in the same account, you can enter your own account ID when configuring the role's trust relationship\.

After creating the role, modify the trust relationship to allow the IAM user \(or the users in the AWS account\) to assume it\. 

The following example shows a trust policy that you could attach to a role\. This policy allows the role to be assumed by any IAM user in the account 123456789012, ***if*** the administrator of that account explicitly grants the `sts:assumerole` permission to the user\.

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

The trust policy doesn't actually grant permissions\. The administrator of the account must delegate the permission to assume the role to individual users by attaching a policy with the appropriate permissions\. The following example shows a policy that you can attach to an IAM user that allows the user to assume only the `marketingadminrole` role\. For more information about granting a user access to assume a role, see [Granting a User Permission to Switch Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_permissions-to-switch.html) in the *IAM User Guide*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::123456789012:role/marketingadminrole"
    }
  ]
}
```

The IAM user doesn't need to have any additional permissions to run the CLI commands using the role profile\. Instead, the permissions to run the command come from those attached to the *role*\. You attach permission policies to the role to specify which actions can be performed against which AWS resources\. For more information about attaching permissions to a role \(which works identically to an IAM user\), see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

Now that you have the role profile, role permissions, role trust relationship, and user permissions properly configured, you can use the role at the command line by invoking the `--profile` option\. For example, the following command calls the Amazon S3 `ls` command using the permissions attached to the `marketingadmin` role as defined by the example at the beginning of this topic\.

```
$ aws s3 ls --profile marketingadmin
```

To use the role for several calls, you can set the `AWS_DEFAULT_PROFILE` environment variable for the current session from the command line\. While that environment variable is defined, you don't have to specify the `--profile` option on each command\. 

**Linux, macOS, or Unix**

```
$ export AWS_PROFILE=marketingadmin
```

**Windows**

```
C:\> setx AWS_PROFILE marketingadmin
```

For more information on configuring IAM users and roles, see [Users and Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html) and [Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html) in the *IAM User Guide*\.

## Using Multi\-Factor Authentication<a name="cli-configure-role-mfa"></a>

For additional security, you can require that users provide a one\-time key generated from a multi\-factor authentication \(MFA\) device, a U2F device, or mobile app when they attempt to make a call using the role profile\.

First, you can choose to modify the trust relationship on the IAM role to require MFA\. This prevents anyone from using the role without first authenticating by using MFA\. For an example, see the `Condition` line in the following example\. This policy allows the IAM user named `anika` to assume the role the policy is attached to, but only if she authenticates by using MFA\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::123456789012:user/anika" },
      "Action": "sts:AssumeRole",
      "Condition": { "Bool": { "aws:multifactorAuthPresent": true } }
    }
  ]
}
```

Next, add a line to the role profile that specifies the ARN of the user's MFA device\. The following sample `config` file entries show two role profiles that both use the access keys for the IAM user `anika` to request temporary credentials for the role `cli-role`\. The user `anika` has permissions to assume the role, granted by the role's trust policy\.

```
[profile role-without-mfa]
region = us-west-2
role_arn= arn:aws:iam::128716708097:role/cli-role
source_profile=cli-user

[profile role-with-mfa]
region = us-west-2
role_arn= arn:aws:iam::128716708097:role/cli-role
source_profile = cli-user
mfa_serial = arn:aws:iam::128716708097:mfa/cli-user

[profile anika]
region = us-west-2
output = json
```

The `mfa_serial` setting can take an ARN, as shown, or the serial number of a hardware MFA token\.

The first profile, `role-without-mfa`, doesn't require MFA\. However, because the previous example trust policy attached to the role requires MFA, any attempt to run a command with this profile fails\.

```
$ aws iam list-users --profile cli-role

An error occurred (AccessDenied) when calling the AssumeRole operation: Access denied
```

The second profile entry, `role-with-mfa`, identifies an MFA device to use\. When the user attempts to run a CLI command with this profile, the CLI prompts the user to enter the one\-time password \(OTP\) provided by the MFA device\. If the MFA authentication is succesful, the command then performs the requested operation\. The OTP is not displayed on the screen\.

```
$ aws iam list-users --profile cli-role-mfa
Enter MFA code for arn:aws:iam::123456789012:mfa/cli-user:
{
    "Users": [
        {
            ...
```

## Cross\-Account Roles & External ID<a name="cli-configure-role-xaccount"></a>

You can enable IAM users to use roles that belong to different accounts by configuring the role as a cross\-account role\. During role creation, set the role type to **Another AWS account**, as described in [Creating a Role to Delegate Permissions to an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html)\. Optionally, select **Require MFA**\. The **Require MFA** option configures the appropriate condition in the trust relationship, as described in [Using Multi\-Factor Authentication](#cli-configure-role-mfa)\.

If you use an [external ID](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) to provide additional control over who can use a role across accounts, you must also add the `external_id` parameter to the role profile\. You typically use this only when the other account is controlled by someone outside your company or organization\.

```
[profile crossaccountrole]
role_arn = arn:aws:iam::234567890123:role/SomeRole
source_profile = default
mfa_serial = arn:aws:iam::123456789012:mfa/saanvi
external_id = 123456
```

## Specifying a Role Session Name for Easier Auditing<a name="cli-configure-role-session-name"></a>

When a role is shared by many individuals, auditing becomes more of a challenge\. You want to associate each operation invoked with the individual who invoked the action\. However, when the individual uses a role, the assumption of the role by the individual is a separate action from the invoking of an operation, and you must manually correlate the two\.

You can simplify this by specifying unique role session names when users assume a role\. You do this by adding a `role_session_name` parameter to each named profile in the `config` file that specifies a role\. The `role_session_name` value is passed to the `AssumeRole` operation and becomes part of the ARN for the role session\. It is also included in the AWS CloudTrail logs for all logged operations\.

For example, you could create a role\-based profile as follows: 

```
[profile namedsessionrole]
role_arn = arn:aws:iam::234567890123:role/SomeRole
source_profile = default
role_session_name = Session_Maria_Garcia
```

This results in the role session having the following ARN:

```
arn:aws:iam::234567890123:assumed-role/SomeRole/Session_Maria_Garcia
```

Also, all AWS CloudTrail logs include the role session name in the information captured for each operation\.

## Clearing Cached Credentials<a name="cli-configure-role-cache"></a>

When you use a role, the AWS CLI caches the temporary credentials locally until they expire\. The next time you try to use them, the AWS CLI attempts to renew them on your behalf\. 

If your role's temporary credentials are [revoked](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html), they are not renewed automatically, and attempts to use them fail\. However, you can delete the cache to force the AWS CLI to retrieve new credentials\.

**Linux, macOS, or Unix**

```
$ rm -r ~/.aws/cli/cache
```

**Windows**

```
C:\> del /s /q %UserProfile%\.aws\cli\cache
```