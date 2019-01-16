# AWS Command Line Interface User Guide

-----
*****Copyright &copy; 2019 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is the AWS Command Line Interface?](cli-chap-welcome.md)
+ [Installing the AWS CLI](cli-chap-install.md)
   + [Install the AWS CLI on Linux](install-linux.md)
      + [Installing Python on Linux](install-linux-python.md)
      + [Install the AWS CLI on Amazon Linux](install-linux-al2017.md)
   + [Install the AWS CLI on Windows](install-windows.md)
   + [Install the AWS CLIon macOS](install-macos.md)
   + [Install the AWS CLI in a Virtual Environment](install-virtualenv.md)
   + [Install the AWS CLI Using the Bundled Installer (Linux, macOS, or Unix)](install-bundle.md)
+ [Configuring the AWS CLI](cli-chap-configure.md)
   + [Configuration and Credential Files](cli-configure-files.md)
   + [Named Profiles](cli-configure-profiles.md)
   + [Environment Variables](cli-configure-envvars.md)
   + [Command Line Options](cli-configure-options.md)
   + [Instance Metadata](cli-configure-metadata.md)
   + [Using an HTTP Proxy](cli-configure-proxy.md)
   + [Assuming an IAM Role in the AWS CLI](cli-configure-role.md)
   + [Command Completion](cli-configure-completion.md)
+ [Tutorial: Using the AWS CLI to Deploy an Amazon EC2 Development Environment](cli-chap-tutorial.md)
+ [Using the AWS CLI](cli-chap-using.md)
   + [Getting Help with the AWS CLI](cli-usage-help.md)
   + [Command Structure in the AWS CLI](cli-usage-commandstructure.md)
   + [Specifying Parameter Values for the AWS CLI](cli-usage-parameters.md)
   + [Generate the CLI Skeleton and CLI Input JSON Parameters](cli-usage-skeleton.md)
   + [Controlling Command Output from the AWS CLI](cli-usage-output.md)
   + [Using Shorthand Syntax with the AWS Command Line Interface](cli-usage-shorthand.md)
   + [Using AWS CLI Pagination Options](cli-usage-pagination.md)
+ [Using the AWS CLI to Work with AWS Services](cli-chap-services.md)
   + [Using Amazon DynamoDB with the AWS CLI](cli-services-dynamodb.md)
   + [Using Amazon EC2 with the AWS CLI](cli-services-ec2.md)
      + [Create, Display, and Delete Amazon EC2 Key Pairs](cli-services-ec2-keypairs.md)
      + [Create, Configure, and Delete Security Groups for Amazon EC2](cli-services-ec2-sg.md)
      + [Launch, List, and Terminate Amazon EC2 Instances](cli-services-ec2-instances.md)
   + [Using Amazon S3 Glacier with the AWS CLI](cli-services-glacier.md)
   + [Using AWS Identity and Access Management from the AWS CLI](cli-services-iam.md)
      + [Creating IAM Users and Groups](cli-services-iam-new-user-group.md)
      + [Attach an IAM Managed Policy to an IAM User](cli-services-iam-policy.md)
      + [Set an Initial Password for an IAM User](cli-services-iam-set-pw.md)
      + [Create an Access Key for an IAM User](cli-services-iam-create-creds.md)
   + [Using Amazon S3 with the AWS CLI](cli-services-s3.md)
      + [Using High-Level (s3) Commands with the AWS CLI](using-s3-commands.md)
      + [Using API-Level (s3api) Commands with the AWS CLI](cli-services-s3-apicommands.md)
   + [Using Amazon SNS with the AWS CLI](cli-services-sns.md)
   + [Using Amazon SWF with the AWS CLI](cli-services-swf.md)
      + [List of Amazon SWF Commands by Category](cli-services-swf-commands.md)
      + [Working with Amazon SWF Domains Using the AWS CLI](cli-services-swf-domains.md)
+ [Troubleshooting AWS CLI Errors](cli-chap-troubleshooting.md)