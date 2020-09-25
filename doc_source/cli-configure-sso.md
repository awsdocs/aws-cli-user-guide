# Configuring the AWS CLI to use AWS Single Sign\-On<a name="cli-configure-sso"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

If your organization uses AWS Single Sign\-On \(AWS SSO\), your users can sign in to Active Directory, a built\-in AWS SSO directory, or [another iDP connected to AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-idp.html) and get mapped to an AWS Identity and Access Management \(IAM\) role that enables you to run AWS CLI commands\. Regardless of which iDP you use, AWS SSO abstracts those distinctions away, and they all work with the AWS CLI as described below\. For example, you can connect Microsoft Azure AD as described in the blog article [The Next Evolution in AWS Single Sign\-On](http://aws.amazon.com/blogs/aws/the-next-evolution-in-aws-single-sign-on/)

For more information about AWS SSO, see the [AWS Single Sign\-On User Guide](https://docs.aws.amazon.com/singlesignon/latest/userguide/)\.

This topic describes how to configure the AWS CLI to authenticate the user with AWS SSO to get short\-term credentials to run AWS CLI commands\. It includes the following sections:
+ **[Configuring a named profile to use AWS SSO](#sso-configure-profile)** \- How to create and configure profiles that use AWS SSO for authentication and mapping to an IAM role for AWS permissions\.
+ **[Using an AWS SSO enabled named profile](#sso-using-profile)** \- how to login to AWS SSO from the CLI and use the provided AWS temporary credentials to run AWS CLI commands\. 

## Configuring a named profile to use AWS SSO<a name="sso-configure-profile"></a>

You can configure one or more of your AWS CLI [named profiles](cli-configure-profiles.md) to use a role from AWS SSO You can create and configure multiple profiles and configure each one to use a a different AWS SSO user portal or SSO\-defined role\.

You can configure the profile in the following ways:
+ [Automatically](#sso-configure-profile-auto), using the command `aws configure sso`
+ [Manually](#sso-configure-profile-manual), by editing the \.aws/config file that stores the named profiles\.

### Automatic configuration<a name="sso-configure-profile-auto"></a>

You can add an AWS SSO enabled profile to your AWS CLI by running the following command, providing your AWS SSO start URL and the AWS Region that hosts the AWS SSO directory\. 

```
$ aws configure sso
SSO start URL [None]: [None]: https://my-sso-portal.awsapps.com/start
SSO region [None]:us-east-1
```

The AWS CLI attempts to open your default browser and begin the login process for your AWS SSO account\.

```
SSO authorization page has automatically been opened in your default browser.
Follow the instructions in the browser to complete this authorization request.
```

If the AWS CLI cannot open the browser, the following message appears with instructions on how to manually start the login process\.

```
Using a browser, open the following URL:
 
https://my-sso-portal.awsapps.com/verify

and enter the following code:
QCFK-N451
```

AWS SSO uses the code to associate the AWS SSO session with your current AWS CLI session\. The AWS SSO browser page prompts you to sign in with your AWS SSO account credentials\. This enables the AWS CLI \(through the permissions associated with your AWS SSO account\) to retrieve and display the AWS accounts and roles that you are authorized to use with AWS SSO\.

Next, the AWS CLI displays the AWS accounts available for you to use\. If you are authorized to use only one account, the AWS CLI selects that account for you automatically and skips the prompt\. The AWS accounts that are available for you to use are determined by your user configuration in AWS SSO\.

```
There are 2 AWS accounts available to you.
> DeveloperAccount, developer-account-admin@example.com (123456789011) 
  ProductionAccount, production-account-admin@example.com (123456789022)
```

Use the arrow keys to select the account you want to use with this profile\. The ">" character on the left points to the current choice\. Press ENTER to make your selection\. 

Next, the AWS CLI confirms your account choice, and displays the IAM roles that are available to you in the selected account\. If the selected account lists only one role, the AWS CLI selects that role for you automatically and skips the prompt\. The roles that are available for you to use are determined by your user configuration in AWS SSO\.

```
Using the account ID 123456789011
There are 2 roles available to you.
> ReadOnly
  FullAccess
```

As before, use the arrow keys to select the IAM role you want to use with this profile\. The ">" character on the left points to the current choice\. Press <ENTER> to make your selection\. 

The AWS CLI confirms your role selection\.

```
Using the role name "ReadOnly"
```

Now you can finish the configuration of your profile, by specifying the [default output format](cli-configure-files.md#cli-config-output), the [default AWS Region](cli-configure-files.md#cli-config-region) to send commands to, and providing a [name for the profile](cli-configure-quickstart.md#cli-configure-quickstart-profiles) so you can reference this profile from among all those defined on the local computer\. In the following example, the user enters a default Region, default output format, and the name of the profile\. You can alternatively press `<ENTER>` to select any default values that are shown between the square brackets\. The suggested profile name is the account ID number followed by an underscore followed by the role name\.

```
CLI default client Region [None]: us-west-2<ENTER>
CLI default output format [None]: json<ENTER>
CLI profile name [123456789011_ReadOnly]: my-dev-profile<ENTER>
```

**Note**  
If you specify `default` as the profile name, this profile becomes the one used whenever you run an AWS CLI command and do not specify a profile name\.

A final message describes the completed profile configuration\.

```
To use this profile, specify the profile name using --profile, as shown:

aws s3 ls --profile my-dev-profile
```

The previous example entries would result in a named profile in `~/.aws/config` that looks like the following example\.

```
[profile my-dev-profile]
sso_start_url = https://my-sso-portal.awsapps.com/start
sso_region = us-east-1
sso_account_id = 123456789011
sso_role_name = readOnly
region = us-west-2
output = json
```

At this point, you have a profile that you can use to request temporary credentials\. You must use the `aws sso login` command to actually request and retrieve the temporary credentials needed to run commands\. For instructions, see [Using an AWS SSO enabled named profile](#sso-using-profile)\.

**Note**  
You can also run an AWS CLI command using the specified profile\. If you are not currently logged in to the AWS SSO portal, it starts the login process for you automatically, just as if you had manually ran the command `aws sso login` command\.

### Manual configuration<a name="sso-configure-profile-manual"></a>

To manually add AWS SSO support to a named profile, you must add the following keys and values to the profile definition in the file `~/.aws/config` \(Linux or macOS\) or `%USERPROFILE%/.aws/config` \(Windows\)\.

sso\_start\_url  
The URL that points to the organization's AWS SSO user portal\.  

```
sso_start_url = https://my-sso-portal.awsapps.com/start
```

sso\_region  
The AWS Region that contains the AWS SSO portal host\. This is separate from, and can be a different region than the default CLI `region` parameter\.  

```
sso_region = us_west-2
```

sso\_account\_id  
The AWS account ID that contains the IAM role that you want to use with this profile\.  

```
sso_account_id = 123456789011
```

sso\_role\_name  
The name of the IAM role that defines the user's permissions when using this profile\.   

```
sso_role_name = ReadAccess
```

The presence of these keys identify this profile as one that uses AWS SSO to authenticate the user\. 

You can also include any other keys and values that are valid in the `.aws/config` file, such as `region`, `output`, or `s3`\. However, you can't include any credential related values, such as [role\_arn](cli-configure-files.md#cli-config-role_arn) or [aws\_secret\_access\_key](cli-configure-files.md#cli-config-aws_secret_access_key)\. If you do, the AWS CLI produces an error\.

So a typical AWS SSO profile in `.aws/config` might look similar to the following example\.

```
[profile my-dev-profile]
sso_start_url = https://my-sso-portal.awsapps.com/start
sso_region = us-east-1
sso_account_id = 123456789011
sso_role_name = readOnly
region = us-west-2
output = json
```

At this point, you have a profile that you can use to request temporary credentials\. However, you can't yet run an AWS CLI service command\. You must first use the `aws sso login` command to actually request and retrieve the temporary credentials needed to run commands\. For instructions, see the next section, [Using an AWS SSO enabled named profile](#sso-using-profile)\.

## Using an AWS SSO enabled named profile<a name="sso-using-profile"></a>

This section describes how to use the AWS SSO profile you created in the previous section\.

### Signing in and getting temporary credentials<a name="sso-using-profile-sign-in"></a>

After you configure a named profile automatically or manually, you can invoke it to request temporary credentials from AWS\. Before you can run an AWS CLI service command, you must retrieve and cache a set of temporary credentials\. To get these temporary credentials, run the following command\.

```
$ aws sso login --profile my-dev-profile
```

The AWS CLI opens your default browser and verifies your AWS SSO log in\. 

```
SSO authorization page has automatically been opened in your default browser. 
Follow the instructions in the browser to complete this authorization request.
Successully logged into Start URL: https://my-sso-portal.awsapps.com/start
```

If you are not currently signed in to your AWS SSO account, you must provide your AWS SSO user name and password\.

If the AWS CLI can't open your browser, it prompts you to open it yourself and enter the specified code\.

```
$ aws sso login --profile my-dev-profile
Using a browser, open the following URL:
 
https://my-sso-portal.awsapps.com/verify

and enter the following code:
QCFK-N451
```

The AWS CLI opens your default browser \(or you manually open the browser of your choice\) to the specified page, and enter the provided code\. The webpage then prompts you for your AWS SSO credentials\.

Your AWS SSO session credentials are cached and include an expiration timestamp\. When the credentials expire, the AWS CLI requests you to sign in to AWS SSO again\.

If your AWS SSO credentials are valid, the AWS CLI uses them to securely retrieve AWS temporary credentials for the IAM role specified in the profile\. 

```
Welcome, you have successfully signed-in to the AWS-CLI.
```

### Running a command with your AWS SSO enabled profile<a name="sso-using-profile-running"></a>

You can use these temporary credentials to invoke an AWS CLI command with the associated named profile\. The following example shows that the command was run under an assumed role that is part of the specified account\.

```
$ aws sts get-caller-identity --profile my-dev-profile
{
    "UserId": "AROA12345678901234567:test-user@example.com",
    "Account": "123456789011",
    "Arn": "arn:aws:sts::123456789011:assumed-role/AWSPeregrine_readOnly_12321abc454d123/test-user@example.com"
}
```

As long as you signed in to AWS SSO and those cached credentials are not expired, the AWS CLI automatically renews expired AWS temporary credentials when needed\. However, if your AWS SSO credentials expire, you must explicitly renew them by logging in to your AWS SSO account again\.

```
$ aws s3 ls --profile my-sso-profile
Your short-term credentials have expired. Please sign-in to renew your credentials
SSO authorization page has automatically been opened in your default browser. 
Follow the instructions in the browser to complete this authorization request.
```

You can create multiple AWS SSO enabled named profiles that each point to a different AWS account or role\. You can also use the aws sso login command on more than one profile at a time\. If any of them share the same AWS SSO user account, you must log in to that AWS SSO user account only once and then they all share a single set of AWS SSO cached credentials\. 

```
# The following command retrieves temporary credentials for the AWS account and role 
# specified in one named profile. If you are not yet signed in to AWS SSO or your 
# cached credentials have expired, it opens your browser and prompts you for your 
# AWS SSO user name and password. It then retrieves AWS temporary credentials for
# the IAM role associated with this profile.
$ aws sso login --profile my-first-sso-profile

# The next command retrieves a different set of temporary credentials for the AWS 
# account and role specified in the second named profile. It does not overwrite or 
# in any way compromise the first profile's credentials. If this profile specifies the
# same AWS SSO portal, then it uses the SSO credentials that you retrieved in the 
# previoius command. The AWS CLI then retrieves AWS temporary credentials for the
# IAM role associated with the second profile. You don't have to sign in to 
# AWS SSO again.
$ aws sso login --profile my-second-sso-profile

# The following command lists the Amazon EC2 instances accessible to the role 
# identified in the first profile.
$ aws ec2 describe-instances --profile my-first-sso-profile

# The following command lists the Amazon EC2 instances accessible to the role 
# identified in the second profile.
$ aws ec2 describe-instances --profile my-second-sso-profile
```

### Signing out of your AWS SSO sessions<a name="sso-using-profile-sign-out"></a>

When you are done using your AWS SSO enabled profiles, you can choose to do nothing and let the AWS temporary credentials and your AWS SSO credentials expire\. However, you can also choose to run the following command to immediately delete all cached credentials in the SSO credential cache folder and all AWS temporary credentials that were based on the AWS SSO credentials\. This makes those credentials unavailable to be used for any future command\.

```
$ aws sso logout
Successfully signed out of all SSO profiles.
```

If you later want to run commands with one of your AWS SSO enabled profiles, you must again run the `aws sso login` command \(see the previous section\) and specify the profile to use\.