# Configuring the AWS CLI<a name="cli-chap-getting-started"></a>

This section explains how to configure settings that the AWS Command Line Interface uses when interacting with AWS, such as your security credentials and the default region\.

**Note**  
The AWS CLI signs requests on your behalf, and includes a date in the signature\. Ensure that your computer's date and time are set correctly; if not, the date in the signature may not match the date of the request, and AWS rejects the request\.

**Topics**
+ [Quick Configuration](#cli-quick-configuration)
+ [Configuration Settings and Precedence](#config-settings-and-precedence)
+ [Configuration and Credential Files](cli-config-files.md)
+ [Named Profiles](cli-multiple-profiles.md)
+ [Environment Variables](cli-environment.md)
+ [Command Line Options](cli-command-line.md)
+ [Instance Metadata](cli-metadata.md)
+ [Using an HTTP Proxy](cli-http-proxy.md)
+ [Assuming a Role](cli-roles.md)
+ [Command Completion](cli-command-completion.md)

## Quick Configuration<a name="cli-quick-configuration"></a>

 For general use, the `aws configure` command is the fastest way to set up your AWS CLI installation\. 

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

 The AWS CLI will prompt you for four pieces of information\. AWS Access Key ID and AWS Secret Access Key are your account credentials\.

**To get the access key ID and secret access key for an IAM user**

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. We recommend that you use IAM access keys instead of AWS account root user access keys\. IAM lets you securely control access to AWS services and resources in your AWS account\.

The only time that you can view or download the secret access keys is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/home?#home)\.

1. In the navigation pane of the console, choose **Users**\.

1. Choose your IAM user name \(not the check box\)\.

1. Choose the **Security credentials** tab and then choose **Create access key**\.

1. To see the new access key, choose **Show**\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\.

   Keep the keys confidential in order to protect your AWS account, and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

**Related topics**
+ [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 

Default region is the name of the region you want to make calls against by default\. This is usually the region closest to you, but it can be any region\. For example, type `us-west-2` to use US West \(Oregon\)\.

**Note**  
You must specify an AWS region when using the AWS CLI\. For a list of services and available regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. The region designators used by the AWS CLI are the same names that you see in AWS Management Console URLs and service endpoints\. 

Default output format can be either `json`, `text`, or `table`\. If you don't specify an output format, `json` is used\.

If you have multiple profiles, you can configure additional, named profiles by using the `--profile` option\. 

```
$ aws configure --profile user2
AWS Access Key ID [None]: AKIAI44QH8DHBEXAMPLE
AWS Secret Access Key [None]: je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: text
```

To update any of your settings, simply run `aws configure` again and enter new values as appropriate\. The next sections contain more information on the files that `aws configure` creates, additional settings, and named profiles\.

## Configuration Settings and Precedence<a name="config-settings-and-precedence"></a>

The AWS CLI uses a *provider chain* to look for AWS credentials in a number of different places, including system or user environment variables and local AWS configuration files\.

The AWS CLI looks for credentials and configuration settings in the following order:

1. **Command line options** – region, output format and profile can be specified as command options to override default settings\.

1. **[Environment variables](cli-environment.md)** – `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN`\.

1. **The AWS credentials file** – located at `~/.aws/credentials` on Linux, macOS, or Unix, or at `C:\Users\USERNAME \.aws\credentials` on Windows\. This file can contain multiple named profiles in addition to a default profile\.

1. **The CLI configuration file** – typically located at `~/.aws/config` on Linux, macOS, or Unix, or at `C:\Users\USERNAME \.aws\config` on Windows\. This file can contain a default profile, named profiles, and CLI specific configuration parameters for each\. 

1. **Container credentials** – provided by Amazon Elastic Container Service on container instances when you [assign a role to your task](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)\.

1. **Instance profile credentials** – these credentials can be used on EC2 instances with an assigned instance role, and are delivered through the Amazon EC2 metadata service\.