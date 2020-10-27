# Configuring the AWS CLI<a name="cli-chap-configure"></a>

This section explains how to configure the settings that the AWS Command Line Interface \(AWS CLI\) uses to interact with AWS\. These include your security credentials, the default output format, and the default AWS Region\.

**Note**  
AWS requires that all incoming requests are cryptographically signed\. The AWS CLI does this for you\. The "signature" includes a date/time stamp\. Therefore, you must ensure that your computer's date and time are set correctly\. If you don't, and the date/time in the signature is too far off of the date/time recognized by the AWS service, AWS rejects the request\.

**Topics**
+ [Configuration basics](cli-configure-quickstart.md)
+ [Configuration and credential file settings](cli-configure-files.md)
+ [Named profiles](cli-configure-profiles.md)
+ [Configuring the AWS CLI to use AWS Single Sign\-On](cli-configure-sso.md)
+ [Environment variables to configure the AWS CLI](cli-configure-envvars.md)
+ [Command line options](cli-configure-options.md)
+ [Sourcing credentials with an external process](cli-configure-sourcing-external.md)
+ [Getting credentials from EC2 instance metadata](cli-configure-metadata.md)
+ [Using an HTTP proxy](cli-configure-proxy.md)
+ [Using an IAM role in the AWS CLI](cli-configure-role.md)
+ [Command completion](cli-configure-completion.md)