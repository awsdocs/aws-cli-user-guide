# Troubleshooting AWS CLI Errors<a name="troubleshooting"></a>

After installing with `pip`, you may need to add the `aws` executable to your OS's `PATH` environment variable, or change its mode to make it executable\.

**Error:** *aws: command not found*

You may need to add the `aws` executable to your OS's `PATH` environment variable\.
+ **Windows** – [Adding the AWS CLI Executable to your Command Line Path](install-windows.md#awscli-install-windows-path)
+ **macOS** – [Adding the AWS CLI Executable to your Command Line Path](install-macos.md#awscli-install-osx-path)
+ **Linux** – [Adding the AWS CLI Executable to your Command Line Path](install-linux.md#install-linux-path)

If `aws` is in your `PATH` and you still see this error, it may not have the right file mode\. Try running it directly\.

```
$ ~/.local/bin/aws --version
```

**Error:** *permission denied*

Make sure that the `aws` script has a file mode that is executable\. For example, `755`\.

Run `chmod +x` to make the file executable\.

```
$ chmod +x ~/.local/bin/aws
```

**Error:** *AWS was not able to validate the provided credentials*

The AWS CLI may be reading credentials from a different location than you expect\. Run `aws configure list` to confirm that the correct credentials are used\.

```
$ aws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None    None
access_key     ****************XYVA shared-credentials-file
secret_key     ****************ZAGY shared-credentials-file
    region                us-west-2      config-file    ~/.aws/config
```

If the correct credentials are in use, your clock may be out of sync\. On Linux, macOS, or Unix, run `data` to check the time\.

```
date
```

If your system clock is off, use `ntpd` to sync it\.

```
sudo service ntpd stop
sudo ntpdate time.nist.gov
sudo service ntpd start
ntpstat
```

On Windows, use the date and time options in the control panel to configure your system clock\.

**Error:** *An error occurred \(UnauthorizedOperation\) when calling the *CreateKeyPair* operation: You are not authorized to perform this operation\.*

Your IAM user or role needs permission to call the API actions that correspond to the commands that you run with the AWS CLI\. Most commands call a single action with a name that matches the command name; however, custom commands like `aws s3 sync` call multiple APIs\. You can see which APIs a command calls by using the `--debug` option\.