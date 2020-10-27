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
      + [Using the official AWS CLI version 2 Docker image](install-cliv2-docker.md)
      + [Installing the AWS CLI version 2 on Linux](install-cliv2-linux.md)
      + [Installing the AWS CLI version 2 on macOS](install-cliv2-mac.md)
      + [Installing the AWS CLI version 2 on Windows](install-cliv2-windows.md)
   + [Installing the AWS CLI version 1](install-cliv1.md)
      + [Install, Update, and Uninstall the AWS CLI version 1 on Amazon Linux](install-linux-al2017.md)
      + [Install, Update, and Uninstall the AWS CLI version 1 on Linux](install-linux.md)
      + [Install, Update, and Uninstall the AWS CLI version 1 on macOS](install-macos.md)
      + [Install, Update, and Uninstall the AWS CLI version 1 on Windows](install-windows.md)
      + [Install and Update the AWS CLI version 1 in a virtual environment](install-virtualenv.md)
      + [Using the AWS CLI version 1 with earlier versions of Python](deprecate-old-python-versions.md)
+ [Configuring the AWS CLI](cli-chap-configure.md)
   + [Configuration basics](cli-configure-quickstart.md)
   + [Configuration and credential file settings](cli-configure-files.md)
   + [Named profiles](cli-configure-profiles.md)
   + [Configuring the AWS CLI to use AWS Single Sign-On](cli-configure-sso.md)
   + [Environment variables to configure the AWS CLI](cli-configure-envvars.md)
   + [Command line options](cli-configure-options.md)
   + [Sourcing credentials with an external process](cli-configure-sourcing-external.md)
   + [Getting credentials from EC2 instance metadata](cli-configure-metadata.md)
   + [Using an HTTP proxy](cli-configure-proxy.md)
   + [Using an IAM role in the AWS CLI](cli-configure-role.md)
   + [Command completion](cli-configure-completion.md)
+ [Using the AWS CLI](cli-chap-using.md)
   + [Getting help with the AWS CLI](cli-usage-help.md)
   + [Command structure in the AWS CLI](cli-usage-commandstructure.md)
   + [Specifying parameter values for the AWS CLI](cli-usage-parameters.md)
      + [Common AWS CLI parameter types](cli-usage-parameters-types.md)
      + [Using quotation marks with strings in the AWS CLI](cli-usage-parameters-quoting-strings.md)
      + [Having the AWS CLI prompt you for parameters](cli-usage-parameters-prompting.md)
      + [Loading AWS CLI parameters from a file](cli-usage-parameters-file.md)
      + [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](cli-usage-skeleton.md)
      + [Using shorthand syntax with the AWS CLI](cli-usage-shorthand.md)
   + [Controlling command output from the AWS CLI](cli-usage-output.md)
   + [Using AWS CLI pagination options](cli-usage-pagination.md)
   + [Understanding return codes from the AWS CLI](cli-usage-returncodes.md)
+ [Using the AWS CLI to work with AWS Services](cli-chap-services.md)
   + [Using Amazon DynamoDB with the AWS CLI](cli-services-dynamodb.md)
   + [Using Amazon EC2 with the AWS CLI](cli-services-ec2.md)
      + [Creating, displaying, and deleting Amazon EC2 key pairs](cli-services-ec2-keypairs.md)
      + [Creating, configuring, and deleting security groups for Amazon EC2](cli-services-ec2-sg.md)
      + [Launching, listing, and terminating Amazon EC2 instances](cli-services-ec2-instances.md)
   + [Using Amazon S3 Glacier with the AWS CLI](cli-services-glacier.md)
   + [Using AWS Identity and Access Management from the AWS CLI](cli-services-iam.md)
      + [Creating IAM users and groups](cli-services-iam-new-user-group.md)
      + [Attaching an IAM managed policy to an IAM user](cli-services-iam-policy.md)
      + [Setting an initial password for an IAM user](cli-services-iam-set-pw.md)
      + [Create an access key for an IAM user](cli-services-iam-create-creds.md)
   + [Using Amazon S3 with the AWS CLI](cli-services-s3.md)
      + [Using high-level (s3) commands with the AWS CLI](cli-services-s3-commands.md)
      + [Using API-Level (s3api) commands with the AWS CLI](cli-services-s3-apicommands.md)
   + [Using Amazon SNS with the AWS CLI](cli-services-sns.md)
   + [Using Amazon SWF with the AWS CLI](cli-services-swf.md)
      + [List of Amazon SWF commands by category](cli-services-swf-commands.md)
      + [Working with Amazon SWF domains using the AWS CLI](cli-services-swf-domains.md)
+ [Security in the AWS Command Line Interface](security.md)
   + [Data protection in the AWS CLI](data-protection.md)
   + [Identity and Access Management for the AWS CLI](cli-security-iam.md)
   + [Compliance validation for the AWS CLI](cli-security-compliance-validation.md)
   + [Enforcing a minimum version of TLS 1.2](cli-security-enforcing-tls.md)
+ [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)
+ [Breaking changes – Migrating from AWS CLI version 1 to version 2](cliv2-migration.md)
+ [AWS CLI user guide document history](document-history.md)