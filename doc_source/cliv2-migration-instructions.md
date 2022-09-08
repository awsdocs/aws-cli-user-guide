--------

--------

# AWS CLI version 2 migration instructions<a name="cliv2-migration-instructions"></a>

This topic provides instructions for migrating from AWS CLI version 1 to AWS CLI version 2\.

AWS CLI versions 1 and 2 use the same `aws` command name\. If you have both versions installed, your computer uses the first one found in your search path\. If you previously installed AWS CLI version 1, we recommend that you do one of the following to use AWS CLI version 2:
+ ** Recommended** – [Uninstall AWS CLI version 1 and use only AWS CLI version 2](#cliv2-migration-instructions-migrate)\.
+ [To have both version installed](#cliv2-migration-instructions-side-by-side), use your operating system's ability to create a symbolic link \(symlink\) or alias with a different name for one of the two `aws` commands\.

For information on breaking changes between version 1 and version 2, see [New features and changes in AWS CLI version 2](cliv2-migration-changes.md)\.

## Replacing version 1 with version 2<a name="cliv2-migration-instructions-migrate"></a>

Perform the following steps to replace AWS CLI version 1 with AWS CLI version 2\. 

**To replace AWS CLI version 1 with AWS CLI version 2**

1. Prepare any existing scripts you have for the migration by confirming any breaking changes between version 1 and version 2 in [New features and changes in AWS CLI version 2](cliv2-migration-changes.md)\.

1. Uninstall the AWS CLI version 1 by following the uninstall instructions for your operating system in [Installing, updating, and uninstalling the AWS CLI version 1](https://docs.aws.amazon.com/cli/v1/userguide/cli-chap-install.html)\.

1. Confirm that the AWS CLI is completely uninstalled by using the following command\.

   ```
   $ aws --version
   ```

   Complete one of the following based on the output:
   + **No version returned:** You've successfully uninstalled the AWS CLI version 1 and can proceed to the next step\.
   + **A version is returned:** You still have an install of the AWS CLI version 1\. For troubleshooting steps, see [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\. Perform troubleshooting steps until no version output is received\.

1. Install the AWS CLI version 2 by following the appropriate install instructions for your operating system in [Installing or updating the latest version of the AWS CLI](getting-started-install.md)\.

## Side\-by\-side install<a name="cliv2-migration-instructions-side-by-side"></a>

To have both versions installed, use your operating system's ability to create a symbolic link \(symlink\) or alias with a different name for one of the two `aws` commands\. 

1. Install the AWS CLI version 2 by following the appropriate install instructions for your operating system in [Installing or updating the latest version of the AWS CLI](getting-started-install.md)\.

1. Use your operating system's ability to create a symlink or alias with a different name for one of the two `aws` commands, such as using *`aws2`* for AWS CLI version 2\. The following are symlink examples for AWS CLI version 2\. Replace the *PATH* with your install location\.

------
#### [ Linux and macOS ]

   You can use a [symbolic link](https://www.linux.com/tutorials/understanding-linux-links/) or [alias](https://www.linux.com/tutorials/aliases-diy-shell-commands/) on Linux and macOS\.

   ```
   $ alias aws2='PATH'
   ```

------
#### [ Windows command prompt ]

   [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) on Windows\.

   ```
   C:\> doskey aws2=PATH
   ```

------