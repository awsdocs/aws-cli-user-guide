--------

--------

# Uninstalling the AWS CLI version 2<a name="uninstall"></a>

This topic describes how to uninstall the AWS Command Line Interface version 2 \(AWS CLI version 2\)\. 

AWS CLI version 2 uninstallation instructions:

## Linux<a name="uninstall-linux"></a>

To uninstall the AWS CLI version 2, run the following commands\.

1. Locate the symlink and install paths\.
   + Use the `which` command to find the symlink\. This shows the path you used with the `--bin-dir` parameter\.

     ```
     $ which aws
     /usr/local/bin/aws
     ```
   + Use the `ls` command to find the directory that the symlink points to\. This gives you the path you used with the `--install-dir` parameter\.

     ```
     $ ls -l /usr/local/bin/aws
     lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
     ```

1. Delete the two symlinks in the `--bin-dir` directory\. If your user account has write permission to these directories, you don't need to use `sudo`\.

   ```
   $ sudo rm /usr/local/bin/aws
   $ sudo rm /usr/local/bin/aws_completer
   ```

1. Delete the `--install-dir` directory\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm -rf /usr/local/aws-cli
   ```

1. **\(Optional\)** Remove the shared AWS SDK and AWS CLI settings information in the `.aws` folder\.
**Warning**  
These configuration and credentials settings are shared across all AWS SDKs and the AWS CLI\. If you remove this folder, they cannot be accessed by any AWS SDKs that are still on your system\.

   The default location of the `.aws` folder differs between platforms, by default the folder is located in *\~/\.aws/*\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm -rf ~/.aws/
   ```

## macOS<a name="uninstall-macos"></a>

To uninstall the AWS CLI version 2, run the following commands, substituting the paths you used to install\. The example commands use the default installation paths\.

1. Find the folder that contains the symlinks to the main program and the completer\.

   ```
   $ which aws
   /usr/local/bin/aws
   ```

1. Using that information, run the following command to find the installation folder that the symlinks point to\.

   ```
   $ ls -l /usr/local/bin/aws
   lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/aws
   ```

1. Delete the two symlinks in the first folder\. If your user account already has write permission to these folders, you don't need to use `sudo`\.

   ```
   $ sudo rm /usr/local/bin/aws
   $ sudo rm /usr/local/bin/aws_completer
   ```

1. Delete the main installation folder\. Use `sudo` to gain write access to the `/usr/local` folder\.

   ```
   $ sudo rm -rf /usr/local/aws-cli
   ```

1. **\(Optional\)** Remove the shared AWS SDK and AWS CLI settings information in the `.aws` folder\.
**Warning**  
These configuration and credentials settings are shared across all AWS SDKs and the AWS CLI\. If you remove this folder, they cannot be accessed by any AWS SDKs that are still on your system\.

   The default location of the `.aws` folder differs between platforms, by default the folder is located in *\~/\.aws/*\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm -rf ~/.aws/
   ```

## Windows<a name="uninstall-windows"></a>

1. Open **Programs and Features** by doing one of the following:
   + Open the **Control Panel**, and then choose **Programs and Features**\.
   + Open a command prompt, and then enter the following command\.

     ```
     C:\> appwiz.cpl
     ```

1. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\.

1. Confirm that you want to uninstall the AWS CLI\.

1. **\(Optional\)** Remove the shared AWS SDK and AWS CLI settings information in the `.aws` folder\.
**Warning**  
These configuration and credentials settings are shared across all AWS SDKs and the AWS CLI\. If you remove this folder, they cannot be accessed by any AWS SDKs that are still on your system\.

   The default location of the `.aws` folder differs between platforms, by default the folder is located in *%UserProfile%\\\.aws*\.

   ```
   $ rmdir %UserProfile%\.aws
   ```

## Troubleshooting AWS CLI install and uninstall errors<a name="uninstall-tshoot"></a>

If you come across issues after installing or uninstalling the AWS CLI, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md) for troubleshooting steps\. For the most relevant troubleshooting steps, see [Command not found errors](cli-chap-troubleshooting.md#tshoot-install-not-found), [The "`aws --version`" command returns a different version than you installed](cli-chap-troubleshooting.md#tshoot-install-wrong-version), and [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\.