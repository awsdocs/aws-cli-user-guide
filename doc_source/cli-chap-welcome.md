# What is the AWS Command Line Interface?<a name="cli-chap-welcome"></a>

The AWS Command Line Interface \(AWS CLI\) is an open source tool that enables you to interact with AWS services using commands in your command\-line shell\. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser\-based AWS Management Console from the command prompt in your terminal program:
+ **Linux shells** – Use common shell programs such as [https://www.gnu.org/software/bash/](https://www.gnu.org/software/bash/), [http://www.zsh.org/](http://www.zsh.org/), and [https://www.tcsh.org/](https://www.tcsh.org/) to run commands in Linux or macOS\.
+ **Windows command line** – On Windows, run commands at the Windows command prompt or in PowerShell\.
+ **Remotely** – Run commands on Amazon Elastic Compute Cloud \(Amazon EC2\) instances through a remote terminal program such as PuTTY or SSH, or with AWS Systems Manager\.

All IaaS \(infrastructure as a service\) AWS administration, management, and access functions in the AWS Management Console are available in the AWS API and CLI\. New AWS IaaS features and services provide full AWS Management Console functionality through the API and CLI at launch or within 180 days of launch\. 

The AWS CLI provides direct access to the public APIs of AWS services\. You can explore a service's capabilities with the AWS CLI, and develop shell scripts to manage your resources\. In addition to the low\-level, API\-equivalent commands, several AWS services provide customizations for the AWS CLI\. Customizations can include higher\-level commands that simplify using a service with a complex API\.

## AWS CLI versions<a name="about-versions"></a>

The AWS CLI is available in two versions and information in this guide applies to both versions unless stated otherwise\.
+ **Version 2\.x** – The current, generally available release of the AWS CLI that is intended for use in production environments\.
+ **Version 1\.x** – The previous version of the AWS CLI that is available for backwards compatibility\.

For more information on the different versions, see [About the AWS CLI versions](welcome-versions.md)

## Maintenance and support for SDK major versions<a name="sdks-major-versions-maintenance-support"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Shared Configuration and Credentials Reference Guide](https://docs.aws.amazon.com/credref/latest/refdocs/overview.html):
+ [AWS SDKs and Tools Maintenance Policy](https://docs.aws.amazon.com/credref/latest/refdocs/maint-policy.html)
+ [AWS SDKs and Tools Version Support Matrix](https://docs.aws.amazon.com/credref/latest/refdocs/version-support-matrix.html)

## About Amazon Web Services<a name="about-aws"></a>

Amazon Web Services \(AWS\) is a collection of digital infrastructure services that developers can leverage when developing their applications\. The services include computing, storage, database, and application synchronization \(messaging and queuing\)\. AWS uses a pay\-as\-you\-go service model\. You are charged only for the services that you—or your applications—use\. Also, to make AWS more approachable as a platform for prototyping and experimentation, AWS offers a free usage tier\. On this tier, services are free below a certain level of usage\. For more information about AWS costs and the Free Tier, see [Test\-Driving AWS in the Free Usage Tier](https://docs.aws.amazon.com/FeaturedArticles/latest/TestDriveFreeTier.html)\. To obtain an AWS account, open the [AWS home page](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) and then click **Sign Up**\.