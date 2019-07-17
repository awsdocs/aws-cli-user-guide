# Install the AWS CLI on Amazon Linux<a name="install-linux-al2017"></a>

The AWS Command Line Interface \(AWS CLI\) comes preinstalled on Amazon Linux and Amazon Linux 2\. Check the currently installed version by using the following command\.

```
$ aws --version
aws-cli/1.16.116 Python/3.6.8 Linux/4.14.77-81.59.amzn2.x86_64 botocore/1.12.106
```

**Important**  
Using `sudo` to complete a command grants the command full access to your system\. We recommend using that command only when no option that is more secure exists\. For commands like `pip`, we recommend that you avoid using `sudo` by using a [Python virtual environment \(venv\)](https://docs.python.org/3/library/venv.html) or by specifying the `--user` option to install in the user's folders instead of the system's folders\.

You can use `sudo yum update` to get the latest version available in the `yum` repository, but this might not be the latest version\. Instead, we recommend that you use `pip` to get the latest version\.

**Prerequisites**  
Verify that Python and `pip` are already installed\. For more information, see [Install the AWS CLI on Linux](install-linux.md)\.

**To install or upgrade the AWS CLI on Amazon Linux \(user\)**

1. Use `pip3 install` to install the latest version of the AWS CLI\. We recommend that if you have Python version 3\+ installed that you use `pip3`\. If you run the command from within a [Python virtual environment \(venv\)](https://docs.python.org/3/library/venv.html), then you don't need to use the `--user` option\.

   ```
   $ pip3 install --upgrade --user awscli
   ```

1. Add the install location to the beginning of your `PATH` variable\.

   ```
   $ export PATH=/home/ec2-user/.local/bin:$PATH
   ```

   Add this command to the end of your profile's startup script \(for example, `~/.bashrc`\) to persist the change between command line sessions\.

1. Verify that you're running new version with `aws --version`\.

   ```
   $ aws --version
   aws-cli/1.16.116 Python/3.6.8 Linux/4.14.77-81.59.amzn2.x86_64 botocore/1.12.106
   ```