# Installing the AWS CLI version 2 on Windows<a name="install-cliv2-windows"></a>

This section describes how to install, update, and remove the AWS CLI version 2 on Windows\.

**Important**  
AWS CLI versions 1 and 2 use the same `aws` command name\. If you have both versions installed, your computer uses the first one found in your search path\. If you previously installed AWS CLI version 1, we recommend that you do one of the following to use AWS CLI version 2:  
** Recommended** – Uninstall AWS CLI version 1 and use only AWS CLI version 2\. For uninstall instructions, determine the method you used to install AWS CLI version 1 and follow the appropriate uninstall instructions for your operating system in [Installing the AWS CLI version 1](install-cliv1.md)
Use your operating system's ability to create a symbolic link \(symlink\) or alias with a different name for one of the two `aws` commands\. For example, you can use a [symbolic link](https://www.linux.com/tutorials/understanding-linux-links/) or [alias](https://www.linux.com/tutorials/aliases-diy-shell-commands/) on Linux and macOS, or [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) on Windows\.

**Topics**
+ [Prerequisites](#cliv2-windows-prereq)
+ [Install or update the AWS CLI version 2 on Windows using the MSI installer](#cliv2-windows-install)
+ [Uninstall the AWS CLI version 2 from Windows](#cliv2-windows-remove)

## Prerequisites<a name="cliv2-windows-prereq"></a>

Before you can install or update the AWS CLI version 2 on Windows, be sure you have the following:
+ A 64\-bit version of Windows XP or later\.
+ Admin rights to install software

## Install or update the AWS CLI version 2 on Windows using the MSI installer<a name="cliv2-windows-install"></a>

1. Download the AWS CLI MSI installer for Windows \(64\-bit\) at[ https://awscli\.amazonaws\.com/AWSCLIV2\.msi](https://awscli.amazonaws.com/AWSCLIV2.msi)\. 

   To update your current installation of AWS CLI version 2 on Windows, download a new installer each time you update to overwrite previous versions\. AWS CLI is updated regularly, so check the [Releases page on GitHub](https://github.com/aws/aws-cli/releases) to see when the latest version was released\. 

1. Run the downloaded MSI installer and follow the on\-screen instructions\. By default, the AWS CLI installs to `C:\Program Files\Amazon\AWSCLIV2`\.

   In this example the latest version of the CLI is downloaded\. A version can be specified by appending it just before the file extension: `https://awscli.amazonaws.com/AWSCLIV2-2.x.y.zip`

1. To confirm the installation, open the **Start** menu, search for `cmd` to open a command prompt window, and at the command prompt use the `aws --version` command\. 

   Don't include the prompt symbol \(`C:\>`\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the AWS CLI\. The rest of this guide uses the generic prompt symbol \(`$`\), except in cases where a command is Windows\-specific\. For more information about how we format code examples, see [Using the examples](cli-chap-welcome.md#cli-using-examples)\.

   ```
   C:\> aws --version
   aws-cli/2.0.36 Python/3.7.4 Windows/10 botocore/2.0.0
   ```

   If Windows is unable to find the program, you might need to close and reopen the command prompt window to refresh the path, or [add the installation directory to your PATH](install-windows.md#awscli-install-windows-path) environment variable manually\.

## Uninstall the AWS CLI version 2 from Windows<a name="cliv2-windows-remove"></a>

1. Open **Programs and Features** by doing one of the following:
   + Open the **Control Panel**, and then choose **Programs and Features**\.
   + Open a command prompt, and then enter the following command\.

     ```
     C:\> appwiz.cpl
     ```

1. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\.


1. Confirm that you want to uninstall the AWS CLI\.
