--------

--------

# Quick setup<a name="getting-started-quickstart"></a>

This topic explains how to quickly configure basic settings that the AWS Command Line Interface \(AWS CLI\) uses to interact with AWS\. These include your security credentials, the default output format, and the default AWS Region\.

**Topics**
+ [New configuration quick setup](#getting-started-quickstart-new)
+ [Using existing configuration and credentials files](#getting-started-quickstart-existing)

## New configuration quick setup<a name="getting-started-quickstart-new"></a>

For general use, the `aws configure` command in your preferred terminal is the fastest way to set up your AWS CLI installation\. When you enter this command, the AWS CLI prompts you for four pieces of information:
+ [Access key ID](cli-configure-quickstart.md#cli-configure-quickstart-creds)
+ [Secret access key](cli-configure-quickstart.md#cli-configure-quickstart-creds)
+ [AWS Region](cli-configure-quickstart.md#cli-configure-quickstart-region)
+ [Output format](cli-configure-quickstart.md#cli-configure-quickstart-format)

The AWS CLI stores this information in a *profile* \(a collection of settings\) named `default` in the `credentials` file\. By default, the information in this profile is used when you run an AWS CLI command that doesn't explicitly specify a profile to use\. For more information on the `credentials` file, see [Configuration and credential file settings](cli-configure-files.md)

The following example shows sample values\. Replace them with your own values as described in the following sections\.

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

For more detailed information on configuration see [Configuration basics](cli-configure-quickstart.md)\.

## Using existing configuration and credentials files<a name="getting-started-quickstart-existing"></a>

If you have existing configuration and credentials files, these can be used for the AWS CLI\. 

To use the `config` and `credentials` files, move them to the folder named `.aws` in your home directory\. Where you find your home directory location varies based on the operating system, but is referred to using the environment variables `%UserProfile%` in Windows and `$HOME` or `~` \(tilde\) in Unix\-based systems\. 

You can specify a non\-default location for the `config` and `credentials` files by setting the `AWS_CONFIG_FILE` and `AWS_SHARED_CREDENTIALS_FILE` environment variables to another local path\. See [Environment variables to configure the AWS CLI](cli-configure-envvars.md) for details\. 

For more detailed information on configuration and credentials files, see [Configuration and credential file settings](cli-configure-files.md)\.