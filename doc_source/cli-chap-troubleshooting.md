# Troubleshooting AWS CLI Errors<a name="cli-chap-troubleshooting"></a>

## Installation Issues<a name="troubleshooting-install"></a>

If you use `pip` to install the AWS Command Line Interface \(AWS CLI\), you might need to add the folder that contains the `aws` program to your operating system's `PATH` environment variable, or change its mode to make it executable\.

**Error:** *aws: command not found*

You might need to add the `aws` executable to your operating system's `PATH` environment variable\.
+ **Windows** – [Add the AWS CLI Executable to Your Command Line Path](install-windows.md#awscli-install-windows-path)
+ **macOS** – [Add the AWS CLI Executable to Your Command Line Path](install-macos.md#awscli-install-osx-path)
+ **Linux** – [Add the AWS CLI Executable to Your Command Line Path](install-linux.md#install-linux-path)

If `aws` is in your `PATH` and you still see this error, it might not have the right file mode\. Try running it directly\.

```
$ ~/.local/bin/aws --version
```

## Permissions Issues<a name="troubleshooting-permissions"></a>

### Main CLI program must have 'run' permission<a name="troubleshooting-permissions-filemode"></a>

**Error:** *permission denied*

Make sure that the `aws` program has run permissions for the calling user\. Typically, you would use `755`\.

Run `chmod +x` to add run permissions to the file\.

```
$ chmod +x ~/.local/bin/aws
```

### You must use valid credentials<a name="troubleshooting-permissions-wrongcreds"></a>

**Error:** *AWS was not able to validate the provided credentials*

The AWS CLI might be reading credentials from a different location than you expect\. You can run `aws configure list` to confirm which credentials are used\. 

The following example shows how to check the credentials used for the default profile\.

```
$ aws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************XYVA shared-credentials-file
secret_key     ****************ZAGY shared-credentials-file
    region                us-west-2      config-file    ~/.aws/config
```

The following example shows how to check the credentials of a named profile\.

```
$ aws configure list --profile saanvi
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                    saanvi           manual    --profile
access_key         **************** shared-credentials-file
secret_key         **************** shared-credentials-file
    region                us-west-2      config-file    ~/.aws/config
```

If you are using valid credentials, your clock may be out of sync\. On Linux, macOS, or Unix, run `date` to check the time\.

```
$ date
```

If your system clock is not correct within a few minutes, use `ntpd` to sync it\.

```
$ sudo service ntpd stop
$ sudo ntpdate time.nist.gov
$ sudo service ntpd start
$ ntpstat
```

On Windows, use the date and time options in the Control Panel to configure your system clock\.

### Your IAM user must be able to run the command<a name="troubleshooting-permissions-userpolicy"></a>

**Error:** *An error occurred \(UnauthorizedOperation\) when calling the *CreateKeyPair* operation: You are not authorized to perform this operation\.*

Your IAM user or role must have permission to call the API actions that correspond to the commands that you run with the AWS CLI\. 

Most commands call a single action with a name that matches the command name; however, custom commands like `aws s3 sync` call multiple APIs\. You can see which APIs a command calls by using the `--debug` option\.

For information about assigning permissions to IAM users and roles, see [Overview of Access Management: Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html) in the *IAM User Guide*\.