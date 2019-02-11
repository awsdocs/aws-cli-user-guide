# Install the AWS CLI on Amazon Linux<a name="install-linux-al2017"></a>

The AWS Command Line Interface \(AWS CLI\) comes preinstalled on Amazon Linux and Amazon Linux 2\. Check the currently installed version by using the following command\.

```
$ aws --version
aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59.amzn2.x86_64 botocore/1.12.61
```

You can use `sudo yum update` to get the latest version available in the `yum` repository, but this might not be the latest version\. Instead, we recommend that you use `pip` to get the latest version\.

**Prerequisites**  
Verify that Python and `pip` are already installed\. For more information, see [Install the AWS CLI on Linux](install-linux.md)\.

**To upgrade the AWS CLI on Amazon Linux \(root\)**

1. Use `pip3 install` to install the latest version of the AWS CLI\. We recommend that if you have Python version 3\+ installed that you use `pip3`\.

   ```
   $ sudo pip3 install --upgrade awscli
   ```

1. Verify the new version with `aws --version`\.

   ```
   $ aws --version
   aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59.amzn2.x86_64 botocore/1.12.61
   ```

If you don't have root privileges, install the AWS CLI in user mode\.

**To upgrade the AWS CLI on Amazon Linux \(user\)**

1. Use `pip3 install` to install the latest version of the AWS CLI\. We recommend that if you have Python version 3\+ installed that you use `pip3`\.

   ```
   $ sudo pip3 install --upgrade --user awscli
   ```

1. Add the install location to the beginning of your `PATH` variable\.

   ```
   $ export PATH=/home/ec2-user/.local/bin:$PATH
   ```

   Add this command to the end of `~/.bashrc` to maintain the change between sessions\.

1. Verify the new version with `aws --version`\.

   ```
   $ aws --version
   aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59.amzn2.x86_64 botocore/1.12.61
   ```