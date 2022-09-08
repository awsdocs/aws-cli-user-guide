--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# What is the AWS Command Line Interface version 1?<a name="cli-chap-welcome"></a>

**Note**  
The AWS CLI version 1 is not the latest version of the AWS CLI\. Some features introduced in AWS CLI version 2 are not backported to version 1 and you must upgrade to access those features\. There are some "breaking" changes from version 1 that might require you to change your scripts\. For a list of breaking changes in version 2, see [Breaking changes](https://docs.aws.amazon.com/cli/latest/userguide/cliv2-migration.html) in the *AWS CLI version 2 User Guide*\.

The AWS Command Line Interface \(AWS CLI\) is an open source tool that enables you to interact with AWS services using commands in your command\-line shell\. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser\-based AWS Management Console from the command prompt in your terminal program:
+ **Linux shells** – Use common shell programs such as [https://www.gnu.org/software/bash/](https://www.gnu.org/software/bash/), [http://www.zsh.org/](http://www.zsh.org/), and [https://www.tcsh.org/](https://www.tcsh.org/) to run commands in Linux or macOS\.
+ **Windows command line** – On Windows, run commands at the Windows command prompt or in PowerShell\.
+ **Remotely** – Run commands on Amazon Elastic Compute Cloud \(Amazon EC2\) instances through a remote terminal program such as PuTTY or SSH, or with AWS Systems Manager\.

All IaaS \(infrastructure as a service\) AWS administration, management, and access functions in the AWS Management Console are available in the AWS API and AWS CLI\. New AWS IaaS features and services provide full AWS Management Console functionality through the API and CLI at launch or within 180 days of launch\. 

The AWS CLI provides direct access to the public APIs of AWS services\. You can explore a service's capabilities with the AWS CLI, and develop shell scripts to manage your resources\. In addition to the low\-level, API\-equivalent commands, several AWS services provide customizations for the AWS CLI\. Customizations can include higher\-level commands that simplify using a service with a complex API\.

## About AWS CLI version 1<a name="welcome-versions-v1"></a>

The AWS CLI version 1 is the original AWS CLI, and we continue to support it\. However, major new features that are introduced in the AWS CLI version 2 might not be backported to the AWS CLI version 1\. To use those features, you must install the AWS CLI version 2\. The AWS CLI version 1 is built using the SDK for Python, and therefore requires you to install a compatible version of Python\.

To install the AWS CLI version 1, see [Installing, updating, and uninstalling the AWS CLI](cli-chap-install.md)\.

To check the currently installed version, use the following command:

```
$ aws --version
aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
```

For version history, see the [AWS CLI version 1 Changelog](https://github.com/aws/aws-cli/blob/develop/CHANGELOG.rst) on *GitHub*\.

## Maintenance and support for SDK major versions<a name="sdks-major-versions-maintenance-support"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Reference Guide](https://docs.aws.amazon.com/sdkref/latest/guide/overview.html):
+ [AWS SDKs and tools maintenance policy](https://docs.aws.amazon.com/sdkref/latest/guide/maint-policy.html)
+ [AWS SDKs and tools version support matrix](https://docs.aws.amazon.com/sdkref/latest/guide/version-support-matrix.html)

## About Amazon Web Services<a name="about-aws"></a>

Amazon Web Services \(AWS\) is a collection of digital infrastructure services that developers can leverage when developing their applications\. The services include computing, storage, database, and application synchronization \(messaging and queuing\)\. AWS uses a pay\-as\-you\-go service model\. You are charged only for the services that you—or your applications—use\. Also, to make AWS more approachable as a platform for prototyping and experimentation, AWS offers a free usage tier\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [AWS Free Tier](http://aws.amazon.com/free/)\. To obtain an AWS account, open the [AWS home page](http://aws.amazon.com/) and then choose **Create an AWS Account**\.