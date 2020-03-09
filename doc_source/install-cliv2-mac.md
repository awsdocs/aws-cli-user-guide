# Installing the AWS CLI version 2 on macOS<a name="install-cliv2-mac"></a>

This section describes how to install, upgrade, and remove the AWS CLI version 2 on macOS\.

**Important**  
Because AWS CLI versions 1 and 2 use the same `aws` name for the command, your computer will find only the first one found in your search path if you have both installed\. If you previously had installed AWS CLI version 1, then we recommend that you do one of the following to use AWS CLI version 2:  
***Recommended:*** Uninstall AWS CLI version 1 and use only AWS CLI version 2\.
Use your operating system's ability to create an alias or sym link with a different name for one of the two `aws` commands\. For example, you could use [https://www.linux.com/tutorials/understanding-linux-links/](https://www.linux.com/tutorials/understanding-linux-links/) or [https://www.linux.com/tutorials/aliases-diy-shell-commands/](https://www.linux.com/tutorials/aliases-diy-shell-commands/) in Linux and macOS, or []() in Windows\.

**Topics**
+ [Prerequisites](#cliv2-mac-prereq)
+ [Installing using the macOS graphical interface](#cliv2-mac-install-gui)
+ [Installing using the macOS command line](#cliv2-mac-install-cmd)
+ [Confirming the installation](#cliv2-mac-install-confirm)
+ [Upgrading](#cliv2-mac-upgrade)
+ [Uninstalling](#cliv2-mac-remove)

## Prerequisites<a name="cliv2-mac-prereq"></a>
+ The AWS CLI version 2 has no dependencies on other software packages\. It has a self\-contained, embedded copy of all dependencies included in the installer\. You no longer need to install and maintain Python to use the AWS CLI\.
+ We support the AWS CLI version 2 on versions of macOS that are supported by Apple, including High Sierra \(10\.13\), Mojave \(10\.14\), and Catalina \(10\.15\)\.

You can install using either the graphical interface or the command line\.

## Installing using the macOS graphical interface<a name="cliv2-mac-install-gui"></a>

To install using the standard macOS graphical interface and your browser, follow these steps:

1. Using your browser, download this file: [https://awscli.amazonaws.com/AWSCLIV2.pkg](https://awscli.amazonaws.com/AWSCLIV2.pkg)\.

1. Double\-click the downloaded file to launch the installer\.

1. Follow the on\-screen instructions\. You can choose to install the AWS CLI version 2 in the following ways:
   + **For all users on the computer \(requires `sudo`\)**
     + You can install to any folder, or choose the recommended default folder of `/usr/local/aws-cli`\.
     + The installer automatically creates a symbolic link \(symlink\) at `/usr/local/bin/aws` that links to main program in the installation folder you chose\.
   + **For only the current user \(doesn't require `sudo`\)**
     + You can install to any folder to which you have write permission\.
     + You must manually create a symlink file in your `$PATH` that points to the actual `aws` and `aws_completer` programs\. You must run these commands at the command prompt\. Because standard user permissions typically don't allow writing to folders in the path, the installer in this mode doesn't try to add the symlinks\. You must manually create the symlinks after the installer finishes\. If your `$PATH`includes a folder you can write to, you can run the following command without `sudo` if you specify that folder as the target's path\. If you don't have a writable folder in your `$PATH`, then you must use `sudo` in the commands to get permissions to write to the specified target folder\.

       ```
       $ sudo ln -s /folder/installed/aws-cli/aws /folder/in/path/aws
       $ sudo ln -s /folder/installed/aws-cli/aws_completer /folder/in/path/aws_completer
       ```

1. You can view debug logs for the installation by pressing **CMD\+L** anywhere in the installer\. This opens up a log pane that enables you to filter and save the log\. The log file is also automatically saved to `/var/log/install.log`\.

1. Follow the steps in the section [Confirming the installation](#cliv2-mac-install-confirm) below\.

## Installing using the macOS command line<a name="cliv2-mac-install-cmd"></a>

You can also download and install from the command line\. You can choose to install the AWS CLI version 2 in the following ways:
+ [For all users](#cliv2-mac-install-cmd-all-users) \- requires `sudo`
+ [For only the current user](#cliv2-mac-install-cmd-current-user) \- might require `sudo` to create symlink in folder in $PATH<a name="cliv2-mac-install-cmd-all-users"></a>

## To install for all users using the macOS command line<a name="cliv2-mac-install-cmd-all-users"></a>

If you have sudo permissions, you can install the AWS CLI version 2 for all users on the computer\.

We provide the steps in one easy to copy and paste group\. See the descriptions of each line in the steps that follow\. 

```
$ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

1. Download the file using the `curl` command\. The options on the following example command cause the downloaded file to be written to the current directory with the local name \.

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   ```

   In this example, the `-o` option specifies the file name that the downloaded package is written to\. In the previous example, the file is written to `AWSCLIV2.pkg` in the current folder\.

1. Run the standard macOS `installer` program, specifying the downloaded \.pkg file as the source\.

   ```
   $ sudo installer -pkg ./AWSCLIV2.pkg -target /
   ```

   You must specify the name of the package to install by using the `-pkg` parameter, and the drive to which to install the package by using the `-target /` parameter\. The files are installed to `/usr/local/aws-cli`, and a symlink is automatically created in `/usr/local/bin`\. You must include `sudo` on the command to grant write permissions to those folders\. 

1. You can view debug logs after installation is complete\. The logs are written to `/var/log/install.log`\.

1. Follow the steps in the section [Confirming the installation](#cliv2-mac-install-confirm) below\.<a name="cliv2-mac-install-cmd-current-user"></a>

## To install for only the current user using the macOS command line<a name="cliv2-mac-install-cmd-current-user"></a>

If you have sudo permissions, you can install the AWS CLI version 2 for all users on the computer\.

1. Download the file using the `curl` command\. The options on the following example command cause the downloaded file to be written to the current directory with the local name \.

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   ```

   In this example, the `-o` option specifies the file name that the downloaded package is written to\. In the previous example, the file is written to `AWSCLIV2.pkg` in the current folder\.

1. Before you can run the installer, you must create a file that specifies the folder to which the AWS CLI is installed\. This file is an XML formatted file that looks like the following example\. Leave all value as shown, expect you must replace the path */Users/myusername* in line 9 with the path to the folder you want the AWS CLI version 2 installed to\. *The folder must already exist, or the command fails\.* This XML example specifies that the installer is install the AWS CLI in the folder `/Users/myusername`, where it creates a folder named `aws-cli`\.

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

1. Now you can run the standard macOS `installer` program with the following options:
   + Specify the name of the package to install by using the `-pkg` parameter\.
   + To specify a *current user only* installation, you must set the parameter `--target CurrentUserHomeDirectory`\.
   + Specify the path \(relative to the current folder\) and name of the XML file that you created in the previous step in the `--applyChoiceChangesXML` parameter\.

   ```
   $ installer -pkg AWSCLIV2.pkg \
               -target CurrentUserHomeDirectory \
               -applyChoiceChangesXML choices.xml
   ```

   This installs the AWS CLI in the folder `/Users/myusername/aws-cli`\.

1. Finally, you must create a symlink file in your `$PATH` that points to the actual `aws` and `aws_completer` programs\. Because standard user permissions typically don't allow writing to folders in the path, the installer in this mode doesn't try to add the symlinks\. You must manually create the symlinks after the installer finishes\. If your `$PATH`includes a folder you can write to, you can run the following command without `sudo` if you specify that folder as the target's path\. If you don't have a writable folder in your `$PATH`, then you must use `sudo` in the commands to get permissions to write to the specified target folder\.

   ```
   $ sudo ln -s /folder/installed/aws-cli/aws /folder/in/path/aws
   $ sudo ln -s /folder/installed/aws-cli/aws_completer /folder/in/path/aws_completer
   ```

1. You can view debug logs after installation is complete\. The logs are written to `/var/log/install.log`\.

1. Follow the steps in the section [Confirming the installation](#cliv2-mac-install-confirm) below\.

## Confirming the installation<a name="cliv2-mac-install-confirm"></a>

Confirm the installation\. The following commands verify that the shell can find the `aws` command in your path, and that the command can actually run\.

```
$ which aws
/usr/local/bin/aws 
$ aws --version
aws-cli/2.0.0 Python/3.7.4 Darwin/18.7.0 botocore/2.0.0
```

## Upgrading<a name="cliv2-mac-upgrade"></a>

To upgrade your copy of the AWS CLI version 2, run the same steps that you used to install it in the previous section\. Download and run the package\. If it finds that the AWS CLI is already installed, then it automatically overwrites and upgrades the existing installation\.

## Uninstalling<a name="cliv2-mac-remove"></a>

To uninstall the AWS CLI version 2, run the following commands, substituting the paths you used to install\.

Find the folder that contains the symlinks to the main program and the completer\.

```
$ which aws
/usr/local/bin/aws
```

Use that information to find the installation folder that the symlinks point to\.

```
$ ls -l /usr/local/bin/aws
lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/aws
```

Now delete the two symlinks in the first folder\. If your user account already has write permission to these folders, you don't need to use `sudo`\.

```
$ sudo rm /usr/local/bin/aws
$ sudo rm /usr/local/bin/aws_completer
```

Finally, you can delete the main installation folder\. Use `sudo` to gain write access to the `/usr/local` folder\.

```
$ sudo rm -rf /usr/local/aws-cli
```