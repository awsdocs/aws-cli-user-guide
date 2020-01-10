# AWS Command Line Interface User Guide

-----
*****Copyright &copy; 2020 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

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
   + [Installing the AWS CLI version 2](install-cliv2.md)
      + [Installing the AWS CLI version 2 on Linux or macOS](install-cliv2-linux-mac.md)
      + [Installing AWS CLI version 2 on Windows](install-cliv2-windows.md)
   + [Installing the AWS CLI version 1](install-cliv1.md)
      + [Install the AWS CLI version 1 on Linux](install-linux.md)
         + [Installing Python on Linux](install-linux-python.md)
         + [Install the AWS CLI version 1 on Amazon Linux](install-linux-al2017.md)
      + [Install the AWS CLI version 1 on macOS](install-macos.md)
      + [Install the AWS CLI version 1 on Windows](install-windows.md)
      + [Install the AWS CLI version 1 in a Virtual Environment](install-virtualenv.md)
      + [Install the AWS CLI version 1 Using the Bundled Installer (Linux or macOS)](install-bundle.md)
      + [Using the AWS CLI version 1 with Earlier Versions of Python](deprecate-old-python-versions.md)
   + [Migrating from AWS CLI version 1 to version 2](cliv2-migration.md)
+ [Configuring the AWS CLI](cli-chap-configure.md)
   + [Configuration and Credential File Settings](cli-configure-files.md)
   + [Named Profiles](cli-configure-profiles.md)
   + [Configuring the AWS CLI to use AWS Single Sign-On](cli-configure-sso.md)
   + [Environment Variables To Configure the AWS CLI](cli-configure-envvars.md)
   + [Command Line Options](cli-configure-options.md)
   + [Sourcing Credentials with an External Process](cli-configure-sourcing-external.md)
   + [Getting Credentials from EC2 Instance Metadata](cli-configure-metadata.md)
   + [Using an HTTP Proxy](cli-configure-proxy.md)
   + [Using an IAM Role in the AWS CLI](cli-configure-role.md)
   + [Command Completion](cli-configure-completion.md)
+ [Using the AWS CLI](cli-chap-using.md)
   + [Getting Help with the AWS CLI](cli-usage-help.md)
   + [Command Structure in the AWS CLI](cli-usage-commandstructure.md)
   + [Specifying Parameter Values for the AWS CLI](cli-usage-parameters.md)
      + [Common AWS CLI Parameter Types](cli-usage-parameters-types.md)
      + [Using Quotation Marks with Strings in the AWS CLI](cli-usage-parameters-quoting-strings.md)
      + [Having the AWS CLI Prompt You for Parameters](cli-usage-parameters-prompting.md)
      + [Loading AWS CLI Parameters from a File](cli-usage-parameters-file.md)
      + [Generating AWS CLI Skeleton and Input Parameters from a JSON or YAML Input File](cli-usage-skeleton.md)
      + [Using Shorthand Syntax with the AWS CLI](cli-usage-shorthand.md)
   + [Controlling Command Output from the AWS CLI](cli-usage-output.md)
   + [Using AWS CLI Pagination Options](cli-usage-pagination.md)
   + [Understanding Return Codes from the AWS CLI](cli-usage-returncodes.md)
+ [Using the AWS CLI to Work with AWS Services](cli-chap-services.md)
   + [Using Amazon DynamoDB with the AWS CLI](cli-services-dynamodb.md)
   + [Using Amazon EC2 with the AWS CLI](cli-services-ec2.md)
      + [Creating, Displaying, and Deleting Amazon EC2 Key Pairs](cli-services-ec2-keypairs.md)
      + [Creating, Configuring, and Deleting Security Groups for Amazon EC2](cli-services-ec2-sg.md)
      + [Launching, Listing, and Terminating Amazon EC2 Instances](cli-services-ec2-instances.md)
   + [Using Amazon S3 Glacier with the AWS CLI](cli-services-glacier.md)
   + [Using AWS Identity and Access Management from the AWS CLI](cli-services-iam.md)
      + [Creating IAM Users and Groups](cli-services-iam-new-user-group.md)
      + [Attaching an IAM Managed Policy to an IAM User](cli-services-iam-policy.md)
      + [Setting an Initial Password for an IAM User](cli-services-iam-set-pw.md)
      + [Create an Access Key for an IAM User](cli-services-iam-create-creds.md)
   + [Using Amazon S3 with the AWS CLI](cli-services-s3.md)
      + [Using High-Level (s3) Commands with the AWS CLI](cli-services-s3-commands.md)
      + [Using API-Level (s3api) Commands with the AWS CLI](cli-services-s3-apicommands.md)
   + [Using Amazon SNS with the AWS CLI](cli-services-sns.md)
   + [Using Amazon SWF with the AWS CLI](cli-services-swf.md)
      + [List of Amazon SWF Commands by Category](cli-services-swf-commands.md)
      + [Working with Amazon SWF Domains Using the AWS CLI](cli-services-swf-domains.md)
+ [Security in the AWS Command Line Interface](security.md)
   + [Data Protection in the AWS CLI](data-protection.md)
   + [Identity and Access Management for the AWS CLI](cli-security-iam.md)
   + [Compliance Validation for the AWS CLI](cli-security-compliance-validation.md)
+ [Troubleshooting AWS CLI Errors](cli-chap-troubleshooting.md)
+ [AWS CLI User Guide Document History](document-history.md)