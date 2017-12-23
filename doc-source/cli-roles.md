# Assuming a Role<a name="cli-roles"></a>

An [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is a authorization tool that lets a user gain additional permissions, or get permission to perform actions in a different account\.

You can configure the AWS Command Line Interface to use a role by creating a profile for the role in the `~/.aws/config` file\. The following example shows a role profile named `marketingadmin` that is assumed by the default profile\.

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadmin
source_profile = default
```

In this case, the default profile is an IAM user with credentials and permission to assume a role named marketingadmin\. To access the role, you create a named profile\. Instead of configuring this profile with credentials, you specify the ARN of the role and the name of the profile that has access to it\.


+ [Configuring and Using a Role](#cli-role-prepare)
+ [Using Multifactor Authentication](#cli-roles-mfa)
+ [Cross Account Roles](#cli-roles-xaccount)
+ [Clearing Cached Credentials](#cli-roles-cache)

## Configuring and Using a Role<a name="cli-role-prepare"></a>

When you run commands using the role profile, the AWS CLI uses the source profile's credentials to call AWS Security Token Service and assume the specified role\. The source profile must have permission to call `sts:assume-role` against the role, and the role must have a trust relationship with the source profile to allow itself to be assumed\.

Create a new role in IAM with the permissions that you want users to assume by following the procedure under [ Creating a Role to Delegate Permissions to an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-creatingrole-user.html) in the *AWS Identity and Access Management User Guide*\. If the role and the target IAM user are in the same account, you can enter your own account ID when configuring the role's trust relationship\.

After creating the role, modify the trust relationship to allow the IAM user to assume it\. The following example shows a trust relationship that allows a role to be assumed by an IAM user named `jonsmith`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:user/jonsmith"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Next, grant your IAM user permission to assume the role\. The following example shows an AWS Identity and Access Management policy that allows an IAM user to assume the `marketingadmin` role:

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

The user doesn't need to have any additional permissions to run commands using the role profile\. If you want your users to be able to access AWS resources without using the role, apply additional inline or managed policies for those resources\.

 With the role profile, role permissions, trust relationship and user permissions applied, you can assume the role at the command line by using the `profile` option, for example:

```
$ aws s3 ls --profile marketingadmin
```

To use the role for multiple calls, you can set the AWS\_PROFILE environment variable for the current session from the command line:

**Linux, macOS, or Unix**

```
$ export AWS_PROFILE=marketingadmin
```

**Windows**

```
> set AWS_PROFILE=marketingadmin
```

For more information on configuring IAM users and roles, see [ Users and Groups](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html) and [ Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/roles-toplevel.html) in the *AWS Identity and Access Management User Guide*\.

## Using Multifactor Authentication<a name="cli-roles-mfa"></a>

For additional security, you can require users to provide a one time key generated from a multifactor authentication device or mobile app when they attempt to make a call using the role profile\.

First, modify the trust relationship on the role to require multifactor authentication:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::123456789012:user/jonsmith" },
      "Action": "sts:AssumeRole",
      "Condition": { "Bool": { "aws:MultiFactorAuthPresent": true } }
    }
  ]
}
```

Next, add a line to the role profile that specifies the ARN of the user's MFA device:

```
[profile marketingadmin]
role_arn = arn:aws:iam::123456789012:role/marketingadmin
source_profile = default
mfa_serial = arn:aws:iam::123456789012:mfa/jonsmith
```

The `mfa_serial` setting can take an ARN, as shown, or the serial number of a hardware MFA token\.

## Cross Account Roles<a name="cli-roles-xaccount"></a>

You can enable IAM users to assume roles that belong to different accounts by configuring the role as a cross account role\. During role creation, set the role type to one of the options under **[Role for Cross\-Account Access](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html)** and optionally select **Require MFA**\. The **Require MFA** option configures the appropriate condition in the trust relationship as described in \.

If you use an [external ID](http://docs.aws.amazon.com/STS/latest/UsingSTS/sts-delegating-externalid.html) to provide additional control over who can assume a role across accounts, add an `external_id` parameter to the role profile: 

```
[profile crossaccountrole]
role_arn = arn:aws:iam::234567890123:role/xaccount
source_profile = default
mfa_serial = arn:aws:iam::123456789012:mfa/jonsmith
external_id = 123456
```

## Clearing Cached Credentials<a name="cli-roles-cache"></a>

When you assume a role, the AWS CLI caches the temporary credentials locally until they expire\. If your role's temporary credentials are [revoked](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html), you can delete the cache to force the AWS CLI to retrieve new credentials\.

**Linux, macOS, or Unix**

```
$ rm -r ~/.aws/cli/cache
```

**Windows**

```
> del /s /q %UserProfile%\.aws\cli\cache
```