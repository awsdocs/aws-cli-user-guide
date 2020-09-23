# Configuration basics<a name="cli-configure-quickstart"></a>

This section explains how to quickly configure basic settings that the AWS Command Line Interface \(AWS CLI\) uses to interact with AWS\. These include your security credentials, the default output format, and the default AWS Region\.

**Note**  
AWS requires that all incoming requests are cryptographically signed\. The AWS CLI does this for you\. The "signature" includes a date/time stamp\. Therefore, you must ensure that your computer's date and time are set correctly\. If you don't, and the date/time in the signature is too far off of the date/time recognized by the AWS service, AWS rejects the request\.

**Topics**
+ [Quick configuration with `aws configure`](#cli-configure-quickstart-config)
+ [Access key ID and secret access key](#cli-configure-quickstart-creds)
+ [Region](#cli-configure-quickstart-region)
+ [Output format](#cli-configure-quickstart-format)
+ [Profiles](#cli-configure-quickstart-profiles)
+ [Configuration settings and precedence](#cli-configure-quickstart-precedence)

## Quick configuration with `aws configure`<a name="cli-configure-quickstart-config"></a>

For general use, the `aws configure` command is the fastest way to set up your AWS CLI installation\. When you enter this command, the AWS CLI prompts you for four pieces of information:
+ [Access key ID](#cli-configure-quickstart-creds)
+ [Secret access key](#cli-configure-quickstart-creds)
+ [AWS Region](#cli-configure-quickstart-region)
+ [Output format](#cli-configure-quickstart-format)

The AWS CLI stores this information in a *profile* \(a collection of settings\) named `default` in the `credentials` file\. By default, the information in this profile is used when you run an AWS CLI command that doesn't explicitly specify a profile to use\. For more information on the `credentials` file, see [Configuration and credential file settings](cli-configure-files.md)

The following example shows sample values\. Replace them with your own values as described in the following sections\.

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

## Access key ID and secret access key<a name="cli-configure-quickstart-creds"></a>

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. As a best practice, do not use the AWS account root user access keys for any task where it's not required\. Instead, [create a new administrator IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) with access keys for yourself\.

The only time that you can view or download the secret access key is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

**To create access keys for an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Choose the name of the user whose access keys you want to create, and then choose the **Security credentials** tab\.

1. In the **Access keys** section, choose **Create access key**\.

1. To view the new access key pair, choose **Show**\. You will not have access to the secret access key again after this dialog box closes\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\. You will not have access to the secret access key again after this dialog box closes\.

   Keep the keys confidential in order to protect your AWS account and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

1. After you download the `.csv` file, choose **Close**\. When you create an access key, the key pair is active by default, and you can use the pair right away\.

**Related topics**
+ [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 

## Region<a name="cli-configure-quickstart-region"></a>

The `Default region name` identifies the AWS Region whose servers you want to send your requests to by default\. This is typically the Region closest to you, but it can be any Region\. For example, you can type `us-west-2` to use US West \(Oregon\)\. This is the Region that all later requests are sent to, unless you specify otherwise in an individual command\.

**Note**  
You must specify an AWS Region when using the AWS CLI, either explicitly or by setting a default Region\. For a list of the available Regions, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. The Region designators used by the AWS CLI are the same names that you see in AWS Management Console URLs and service endpoints\.

## Output format<a name="cli-configure-quickstart-format"></a>

The `Default output format` specifies how the results are formatted\. The value can be any of the values in the following list\. If you don't specify an output format, `json` is used as the default\.
+ [**`json`**](cli-usage-output.md#json-output) – The output is formatted as a [JSON](https://json.org/) string\.
+ [**`yaml`**](cli-usage-output.md#yaml-output) – The output is formatted as a [YAML](https://yaml.org/) string\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`yaml-stream`**](cli-usage-output.md#yaml-stream-output) – The output is streamed and formatted as a [YAML](https://yaml.org/) string\. Streaming allows for faster handling of large data types\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`text`**](cli-usage-output.md#text-output) – The output is formatted as multiple lines of tab\-separated string values\. This can be useful to pass the output to a text processor, like `grep`, `sed`, or `awk`\.
+ [**`table`**](cli-usage-output.md#table-output) – The output is formatted as a table using the characters \+\|\- to form the cell borders\. It typically presents the information in a "human\-friendly" format that is much easier to read than the others, but not as programmatically useful\.

## Profiles<a name="cli-configure-quickstart-profiles"></a>

A collection of settings is called a profile\. By default, the AWS CLI uses the `default` profile\. You can create and use additional named profiles with varying credentials and settings by specifying the `--profile` option and assigning a name\. 

The following example creates a profile named `produser`\.

```
$ aws configure --profile produser
AWS Access Key ID [None]: AKIAI44QH8DHBEXAMPLE
AWS Secret Access Key [None]: je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: text
```

You can then specify a `--profile profilename` and use the credentials and settings stored under that name\.

```
$ aws s3 ls --profile produser
```

To update these settings, run `aws configure` again \(with or without the `--profile` parameter, depending on which profile you want to update\) and enter new values as appropriate\. The next sections contain more information about the files that `aws configure` creates, additional settings, and named profiles\.

For more information on named profiles, see [Named profiles](cli-configure-profiles.md)\.

## Configuration settings and precedence<a name="cli-configure-quickstart-precedence"></a>

The AWS CLI uses credentials and configuration settings located in multiple places, such as the system or user environment variables, local AWS configuration files, or explicitly declared on the command line as a parameter\. Certain locations take precedence over others\. The AWS CLI credentials and configuration settings take precedence in the following order:

1. **[Command line options](cli-configure-options.md)** – Overrides settings in any other location\. You can specify `--region`, `--output`, and `--profile` as parameters on the command line\.

1. **[Environment variables](cli-configure-envvars.md)** – You can store values in your system's environment variables\.

1. **[CLI credentials file](cli-configure-files.md)** – The `credentials` and `config` file are updated when you run the command `aws configure`\. The `credentials` file is located at `~/.aws/credentials` on Linux or macOS, or at `C:\Users\USERNAME\.aws\credentials` on Windows\. This file can contain the credential details for the `default` profile and any named profiles\.

1. **[CLI configuration file](cli-configure-files.md)** – The `credentials` and `config` file are updated when you run the command `aws configure`\. The `config` file is located at `~/.aws/config` on Linux or macOS, or at `C:\Users\USERNAME\.aws\config` on Windows\. This file contains the configuration settings for the default profile and any named profiles\. 

1. **[Container credentials](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)** – You can associate an IAM role with each of your Amazon Elastic Container Service \(Amazon ECS\) task definitions\. Temporary credentials for that role are then available to that task's containers\. For more information, see [IAM Roles for Tasks](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html) in the *Amazon Elastic Container Service Developer Guide*\.

1. **[Instance profile credentials](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)** – You can associate an IAM role with each of your Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. Temporary credentials for that role are then available to code running in the instance\. The credentials are delivered through the Amazon EC2 metadata service\. For more information, see [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the *Amazon EC2 User Guide for Linux Instances* and [Using Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) in the *IAM User Guide*\.