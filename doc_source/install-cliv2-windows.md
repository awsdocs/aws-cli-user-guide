# Installing AWS CLI version 2 on Windows<a name="install-cliv2-windows"></a>

This section describes how to install, upgrade, and remove AWS CLI version 2 on Windows\.

**Preview Evaluation Software**  
AWS CLI version 2 is provided as a preview for testing and evaluation\. At this time, we do not recommend using it in a production environment\. For production environments, we recommend that you use the generally available version 1\.x\.  
You can provide feedback for this developer preview version in the [AWS CLI version 2 GitHub repo](https://github.com/aws/aws-cli/issues?q=is%3Aopen+is\%3Aissue+label%3Av2)\. Be sure to attach the "V2" label to your issue\.

**Topics**
+ [Prerequisites for Windows](#cliv2-windows-prereq)
+ [Installing on Windows](#cliv2-windows-install)
+ [Upgrading on Windows](#cliv2-windows-upgrade)
+ [Removing from Windows](#cliv2-windows-remove)

## Prerequisites for Windows<a name="cliv2-windows-prereq"></a>
+ The AWS CLI version 2 is supported on Windows XP or later\.
+ The AWS CLI version 2 supports only 64\-bit versions of Windows\.

## Installing on Windows<a name="cliv2-windows-install"></a>

For Windows users, the MSI installation package offers a familiar and convenient way to install the AWS CLI version 2 without installing any other prerequisites\.

**To install the AWS CLI version 2 using the MSI installer**

1. [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://d1vvhvl2y92vvt.cloudfront.net/AWSCLIV2.msi)

1. Run the downloaded MSI installer and follow the onscreen instructions\. By default, the AWS CLI installs to `C:\Program Files\Amazon\AWSCLI2`\.
**Note**  
The preview release of AWS CLI version 2 names the program `aws2` to enable AWS CLI version 1 and version 2 to coexist side\-by\-side\. Future releases of AWS CLI version 2 might change this command name\.

1. To confirm the installation, use the `aws2 --version` command at a command prompt \(open the **Start** menu and search for `cmd` to start a command prompt\)\.

   ```
   C:\> aws2 --version
   aws-cli/2.0.0dev0 Python/3.7.3 Windows/10 botocore/2.0.0dev0
   ```

Don't include the prompt symbol \(`C:\>`, shown above\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the AWS CLI\. The rest of this guide uses the generic prompt symbol, `$` , except in cases where a command is Windows\-specific\.

If Windows is unable to find the program, you might need to close and reopen the command prompt to refresh the path, or [add the installation directory to your PATH](install-windows.md#awscli-install-windows-path) environment variable manually\.

## Upgrading on Windows<a name="cliv2-windows-upgrade"></a>

AWS CLI is updated regularly\. Check the [Releases page on GitHub](https://github.com/aws/aws-cli/releases) to see when the latest version was released\. 

To update to the latest version, download the latest version of the MSI installer and run it, as described previously\. It automatically overwrites the previous version\.

## Removing from Windows<a name="cliv2-windows-remove"></a>

To uninstall the AWS CLI, open the **Control Panel**, and then choose **Programs and Features**\. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\. Confirm that you want to uninstall the AWS CLI when you're prompted\.

You can also launch the **Programs and Features** program from the command line with the following command\.

```
C:\> appwiz.cpl
```