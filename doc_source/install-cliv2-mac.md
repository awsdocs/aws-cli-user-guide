# Installing the AWS CLI version 2 on macOS<a name="install-cliv2-mac"></a>

This topic describes how to install, update, and remove the AWS CLI version 2 on macOS\.

**Important**  
AWS CLI versions 1 and 2 use the same `aws` command name\. If you have both versions installed, your computer uses the first one found in your search path\. If you previously installed AWS CLI version 1, we recommend that you do one of the following to use AWS CLI version 2:  
** Recommended** – Uninstall AWS CLI version 1 and use only AWS CLI version 2\. For uninstall instructions, determine the method you used to install AWS CLI version 1 and follow the appropriate uninstall instructions for your operating system in [Installing the AWS CLI version 1](install-cliv1.md)
Use your operating system's ability to create a symbolic link \(symlink\) or alias with a different name for one of the two `aws` commands\. For example, you can use a [symbolic link](https://www.linux.com/tutorials/understanding-linux-links/) or [alias](https://www.linux.com/tutorials/aliases-diy-shell-commands/) on Linux and macOS, or [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) on Windows\.

**Topics**
+ [Prerequisites](#cliv2-mac-prereq)
+ [Install and update the AWS CLI version 2 using the macOS user interface](#cliv2-mac-install-gui)
+ [Install and update the AWS CLI version 2 using the macOS command line](#cliv2-mac-install-cmd)
+ [Verify the installation](#cliv2-mac-install-confirm)
+ [Uninstall the AWS CLI version 2](#cliv2-mac-remove)

## Prerequisites<a name="cliv2-mac-prereq"></a>
+ We support the AWS CLI version 2 on Apple\-supported versions of 64\-bit macOS\.
+ Because AWS doesn't maintain third\-party repositories, we can’t guarantee that they contain the latest version of the AWS CLI\.

## Install and update the AWS CLI version 2 using the macOS user interface<a name="cliv2-mac-install-gui"></a>

The following steps show how to install or update to the latest version of the AWS CLI version 2 by using the standard macOS user interface and your browser\. If you are updating to the latest version, use the same installation method that you used for your current version\.

1. In your browser, download the macOS `pkg` file: 
   + **For the latest version of the AWS CLI:** [https://awscli.amazonaws.com/AWSCLIV2.pkg](https://awscli.amazonaws.com/AWSCLIV2.pkg)
   + **For a specific version of the AWS CLI:** Append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following link [https://awscli.amazonaws.com/https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg](https://awscli.amazonaws.com/https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg)\. For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

1. Double\-click the downloaded file to launch the installer\.

1. Follow the on\-screen instructions\. You can choose to install the AWS CLI version 2 in the following ways:
   + **For all users on the computer \(requires `sudo`\)**
     + You can install to any folder, or choose the recommended default folder of `/usr/local/aws-cli`\.
     + The installer automatically creates a symlink at `/usr/local/bin/aws` that links to the main program in the installation folder you chose\.
   + **For only the current user \(doesn't require `sudo`\)**
     + You can install to any folder to which you have write permission\.
     + Due to standard user permissions, after the installer finishes, you must manually create a symlink file in your `$PATH` that points to the `aws` and `aws_completer` programs by using the following commands at the command prompt\. If your `$PATH` includes a folder you can write to, you can run the following command without `sudo` if you specify that folder as the target's path\. If you don't have a writable folder in your `$PATH`, you must use `sudo` in the commands to get permissions to write to the specified target folder\. The default location for a symlink is `/usr/local/bin/`\.

       ```
       $ sudo ln -s /folder/installed/aws-cli/aws /usr/local/bin/aws
       $ sudo ln -s /folder/installed/aws-cli/aws_completer /usr/local/bin/aws/aws_completer
       ```
**Note**  
You can view debug logs for the installation by pressing **Cmd\+L** anywhere in the installer\. This opens a log pane that enables you to filter and save the log\. The log file is also automatically saved to `/var/log/install.log`\.

1. To verify the AWS CLI version 2 is installed, follow the steps in [Verify the installation](#cliv2-mac-install-confirm)\.

## Install and update the AWS CLI version 2 using the macOS command line<a name="cliv2-mac-install-cmd"></a>

You can download, install, and update from the command line\. If you are updating to the latest version, use the same installation method that you used in your current version\. You can install the AWS CLI version 2 in one of the following ways:
+ [For all users](#cliv2-mac-install-cmd-all-users) – Requires `sudo`\.
+ [For only the current user](#cliv2-mac-install-cmd-current-user) – Might require `sudo` to create a symlink in a folder in your `$PATH` variable\.<a name="cliv2-mac-install-cmd-all-users"></a>

## To install and update for all users using the macOS command line<a name="cliv2-mac-install-cmd-all-users"></a>

If you have `sudo` permissions, you can install the AWS CLI version 2 for all users on the computer\.

We provide the steps in one easy to copy and paste group\. See the descriptions of each line in the following steps\. 

**For the latest version of the AWS CLI,** use the following command block:

```
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

**For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

 For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

1. Download the file using the `curl` command\. The `-o` option specifies the file name that the downloaded package is written to\. In this example, the file is written to `AWSCLIV2.pkg` in the current folder\.

   **For the latest version of the AWS CLI,** use the following command block:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   ```

   **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

1. Run the standard macOS `installer` program, specifying the downloaded `.pkg` file as the source\. Use the `-pkg` parameter to specify the name of the package to install, and the `-target /` parameter for which drive to install the package to\. The files are installed to `/usr/local/aws-cli`, and a symlink is automatically created in `/usr/local/bin`\. You must include `sudo` on the command to grant write permissions to those folders\. 

   ```
   $ sudo installer -pkg ./AWSCLIV2.pkg -target /
   ```

   After installation is complete, debug logs are written to `/var/log/install.log`\.

1. To verify the AWS CLI version 2 installed, follow the steps in [Verify the installation](#cliv2-mac-install-confirm)\.<a name="cliv2-mac-install-cmd-current-user"></a>

## To install and update for only the current user using the macOS command line<a name="cliv2-mac-install-cmd-current-user"></a>

1. To specify which folder the AWS CLI is installed to, you must create an XML file\. This file is an XML\-formatted file that looks like the following example\. Leave all values as shown, except you must replace the path */Users/myusername* in line 9 with the path to the folder you want the AWS CLI version 2 installed to\. *The folder must already exist, or the command fails\.* This XML example specifies that the installer installs the AWS CLI in the folder `/Users/myusername`, where it creates a folder named `aws-cli`\.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
     <array>
       <dict>
         <key>choiceAttribute</key>
         <string>customLocation</string>
         <key>attributeSetting</key>
         <string>/Users/myusername</string>
         <key>choiceIdentifier</key>
         <string>default</string>
       </dict>
     </array>
   </plist>
   ```

1. Download the `pkg` installer using the `curl` command\. The `-o` option specifies the file name that the downloaded package is written to\. In this example, the file is written to `AWSCLIV2.pkg` in the current folder\.

1. **For the latest version of the AWS CLI,** use the following command block:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   ```

   **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

1. Run the standard macOS `installer` program with the following options:
   + Specify the name of the package to install by using the `-pkg` parameter\.
   + Specify a *current user only* installation by setting the parameter `--target CurrentUserHomeDirectory`\.
   + Specify the path \(relative to the current folder\) and name of the XML file that you created in the `--applyChoiceChangesXML` parameter\.

   The following example installs the AWS CLI in the folder `/Users/myusername/aws-cli`\.

   ```
   $ installer -pkg AWSCLIV2.pkg \
               -target CurrentUserHomeDirectory \
               -applyChoiceChangesXML choices.xml
   ```

1. Because standard user permissions typically don't allow writing to folders in your `$PATH`, the installer in this mode doesn't try to add the symlinks to the `aws` and `aws_completer` programs\. For the AWS CLI to run correctly, you must manually create the symlinks after the installer finishes\. If your `$PATH` includes a folder you can write to and you specify the folder as the target's path, you can run the following command without `sudo`\. If you don't have a writable folder in your `$PATH`, you must use `sudo` for permissions to write to the specified target folder\.

   ```
   $ sudo ln -s /folder/installed/aws-cli/aws /folder/in/your/PATH/aws
   $ sudo ln -s /folder/installed/aws-cli/aws_completer /folder/in/your/PATH/aws_completer
   ```

   After installation is complete, debug logs are written to `/var/log/install.log`\.

1. To verify the AWS CLI version 2 installed, follow the steps in [Verify the installation](#cliv2-mac-install-confirm)\.

## Verify the installation<a name="cliv2-mac-install-confirm"></a>

To verify that the shell can find and run the `aws` command in your `$PATH`, use the following commands\.

```
$ which aws
/usr/local/bin/aws 
$ aws --version
aws-cli/2.0.47 Python/3.7.4 Darwin/18.7.0 botocore/2.0.0
```

## Uninstall the AWS CLI version 2<a name="cliv2-mac-remove"></a>

To uninstall the AWS CLI version 2, run the following commands, substituting the paths you used to install\.

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