# AWS Command Line Interface User Guide

-----
*****Copyright &copy; 2017 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

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
+ [Installing the AWS Command Line Interface](installing.md)
   + [Install the AWS Command Line Interface on Linux](awscli-install-linux.md)
      + [Installing Python on Linux](awscli-install-linux-python.md)
      + [Installing the AWS Command Line Interface on Amazon Linux 2017](awscli-install-linux-al2017.md)
   + [Install the AWS Command Line Interface on Microsoft Windows](awscli-install-windows.md)
   + [Install the AWS Command Line Interface on macOS](cli-install-macos.md)
   + [Install the AWS Command Line Interface in a Virtual Environment](awscli-install-virtualenv.md)
   + [Install the AWS CLI Using the Bundled Installer (Linux, macOS, or Unix)](awscli-install-bundle.md)
+ [Configuring the AWS CLI](cli-chap-getting-started.md)
   + [Configuration and Credential Files](cli-config-files.md)
   + [Named Profiles](cli-multiple-profiles.md)
   + [Environment Variables](cli-environment.md)
   + [Command Line Options](cli-command-line.md)
   + [Instance Metadata](cli-metadata.md)
   + [Using an HTTP Proxy](cli-http-proxy.md)
   + [Assuming a Role](cli-roles.md)
   + [Command Completion](cli-command-completion.md)
+ [Deploying a Development Environment in Amazon EC2 Using the AWS Command Line Interface](tutorial-ec2-ubuntu.md)
+ [Using the AWS Command Line Interface](cli-chap-using.md)
   + [Getting Help with the AWS Command Line Interface](getting-help.md)
   + [Command Structure in the AWS Command Line Interface](command-structure.md)
   + [Specifying Parameter Values for the AWS Command Line Interface](cli-using-param.md)
   + [Generate CLI Skeleton and CLI Input JSON Parameters](generate-cli-skeleton.md)
   + [Controlling Command Output from the AWS Command Line Interface](controlling-output.md)
   + [Using Shorthand Syntax with the AWS Command Line Interface](shorthand-syntax.md)
   + [Using the AWS Command Line Interface's Pagination Options](pagination.md)
+ [Working with Amazon Web Services](chap-working-with-services.md)
   + [Using Amazon DynamoDB with the AWS Command Line Interface](cli-dynamodb.md)
   + [Using Amazon EC2 through the AWS Command Line Interface](cli-using-ec2.md)
      + [Using Key Pairs](cli-ec2-keypairs.md)
      + [Using Security Groups](cli-ec2-sg.md)
      + [Using Amazon EC2 Instances](cli-ec2-launch.md)
   + [Using Amazon Glacier with the AWS Command Line Interface](cli-using-glacier.md)
   + [AWS Identity and Access Management from the AWS Command Line Interface](cli-iam.md)
      + [Create New IAM Users and Groups](cli-iam-new-user-group.md)
      + [Set an IAM Policy for an IAM User](cli-iam-policy.md)
      + [Set an Initial Password for an IAM User](cli-iam-set-pw.md)
      + [Create Security Credentials for an IAM User](cli-iam-create-creds.md)
   + [Using Amazon S3 with the AWS Command Line Interface](cli-s3.md)
      + [Using High-Level s3 Commands with the AWS Command Line Interface](using-s3-commands.md)
      + [Using API-Level (s3api) Commands with the AWS Command Line Interface](using-s3api-commands.md)
   + [Using the AWS Command Line Interface with Amazon SNS](cli-sqs-queue-sns-topic.md)
   + [Using Amazon Simple Workflow Service with the AWS Command Line Interface](cli-using-swf.md)
      + [List of Amazon SWF Commands by Category](swf-commands-by-category.md)
      + [Working with Amazon SWF Domains Using the AWS Command Line Interface](cli-using-swf-domains.md)
+ [Troubleshooting AWS CLI Errors](troubleshooting.md)