# Identity and Access Management for the AWS CLI<a name="cli-security-iam"></a>

The AWS Command Line Interface \(AWS CLI\) uses the same users and roles to access your AWS resources and their services\. The policies that grant permissions are the same because the AWS CLI calls the same API operations that are used by the service console\. For more information, see the "Identity and Access Management" section in the "Security" chapter of the AWS service that you want to use\.

The only major difference is how you authenticate when using a standard IAM user and long\-term credentials\. Although an IAM user requires a password to access an AWS service's console, that same IAM user requires an access key pair to perform the same operations using the AWS CLI\. All other short\-term credentials are used in the same way they are used with the console\.

The credentials used by the AWS CLI are stored in plaintext files and are ***not*** encrypted\.
+ The `$HOME/.aws/credentials` file stores long\-term credentials required to access your AWS resources\. These include your access key ID and secret access key\. 
+ Short\-term credentials, such as those for roles that you assume, or that are for AWS Single Sign\-On services, are also stored in the `$HOME/.aws/cli/cache` and $HOME/\.aws/sso/cache folders, respectively\.

**Mitigation of Risk**
+ We strongly recommend that you configure your file system permissions on the `$HOME/.aws` folder and its child folders and files to restrict access to only authorized users\.
+ Use roles with temporary credentials wherever possible to reduce the opportunity for damage if the credentials are compromised\. Use long\-term credentials only to request and refresh short\-term role credentials\.