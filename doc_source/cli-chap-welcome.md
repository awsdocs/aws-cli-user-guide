--------

--------

# What is the AWS Command Line Interface?<a name="cli-chap-welcome"></a>

The AWS Command Line Interface \(AWS CLI\) is an open source tool that enables you to interact with AWS services using commands in your command\-line shell\. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser\-based AWS Management Console from the command prompt in your terminal program:
+ **Linux shells** – Use common shell programs such as [https://www.gnu.org/software/bash/](https://www.gnu.org/software/bash/), [http://www.zsh.org/](http://www.zsh.org/), and [https://www.tcsh.org/](https://www.tcsh.org/) to run commands in Linux or macOS\.
+ **Windows command line** – On Windows, run commands at the Windows command prompt or in PowerShell\.
+ **Remotely** – Run commands on Amazon Elastic Compute Cloud \(Amazon EC2\) instances through a remote terminal program such as PuTTY or SSH, or with AWS Systems Manager\.

All IaaS \(infrastructure as a service\) AWS administration, management, and access functions in the AWS Management Console are available in the AWS API and AWS CLI\. New AWS IaaS features and services provide full AWS Management Console functionality through the API and CLI at launch or within 180 days of launch\. 

The AWS CLI provides direct access to the public APIs of AWS services\. You can explore a service's capabilities with the AWS CLI, and develop shell scripts to manage your resources\. In addition to the low\-level, API\-equivalent commands, several AWS services provide customizations for the AWS CLI\. Customizations can include higher\-level commands that simplify using a service with a complex API\.

## About AWS CLI version 2<a name="welcome-versions-v2"></a>

The AWS CLI version 2 is the most recent major version of the AWS CLI and supports all of the latest features\. Some features introduced in version 2 are not backported to version 1 and you must upgrade to access those features\. There are some "breaking" changes from version 1 that might require you to change your scripts\. For a list of breaking changes in version 2, see [Migrating from AWS CLI version 1 to version 2](cliv2-migration.md)\.

The AWS CLI version 2 is available to install only as a bundled installer\. While you might find it in package managers, these are unsupported and unofficial packages that are not produced or managed by AWS\. We recommend that you install the AWS CLI from only the official AWS distribution points, as documented in this guide\. 

To install the AWS CLI version 2, see [Installing or updating the latest version of the AWS CLI](getting-started-install.md)\.

To check the currently installed version, use the following command:

```
$ aws --version
aws-cli/2.7.24 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
```

For version history, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

## Maintenance and support for SDK major versions<a name="sdks-major-versions-maintenance-support"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Reference Guide](https://docs.aws.amazon.com/sdkref/latest/guide/overview.html):
+ [AWS SDKs and tools maintenance policy](https://docs.aws.amazon.com/sdkref/latest/guide/maint-policy.html)
+ [AWS SDKs and tools version support matrix](https://docs.aws.amazon.com/sdkref/latest/guide/version-support-matrix.html)

## About Amazon Web Services<a name="about-aws"></a>

Amazon Web Services \(AWS\) is a collection of digital infrastructure services that developers can leverage when developing their applications\. The services include computing, storage, database, and application synchronization \(messaging and queuing\)\. AWS uses a pay\-as\-you\-go service model\. You are charged only for the services that you—or your applications—use\. Also, to make AWS more approachable as a platform for prototyping and experimentation, AWS offers a free usage tier\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [AWS Free Tier](http://aws.amazon.com/free/)\. To obtain an AWS account, open the [AWS home page](http://aws.amazon.com/) and then choose **Create an AWS Account**\.