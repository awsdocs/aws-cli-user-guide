# Installing the AWS Command Line Interface on Amazon Linux 2017<a name="awscli-install-linux-al2017"></a>

The AWS CLI comes pre\-installed on the [ Amazon Linux AMI](http://aws.amazon.com/amazon-linux-ami/)\. Check the currently installed version with `aws --version`\.

```
$ aws --version
aws-cli/1.11.83 Python/2.7.12 Linux/4.9.20-11.31.amzn1.x86_64 botocore/1.5.46
```

You can use `sudo yum update` to get the latest version available in the yum repository, but this may not be the latest version\. Use pip to get the latest version\.

**To upgrade the AWS CLI on Amazon Linux \(root\)**

1. Use `pip install` to install the latest version of the AWS CLI\.

   ```
   $ sudo pip install --upgrade awscli
   ```

1. Verify the new version with `aws --version`\.

   ```
   $ aws --version
   aws-cli/1.11.85 Python/2.7.12 Linux/4.9.20-11.31.amzn1.x86_64 botocore/1.5.48
   ```

If you don't have root privileges, install the AWS CLI in user mode\.

**To upgrade the AWS CLI on Amazon Linux \(user\)**

1. Use `pip install` to install the latest version of the AWS CLI\.

   ```
   $ sudo pip install --upgrade --user awscli
   ```

1. Add the install location to the beginning of your `PATH` variable\.

   ```
   $ export PATH=/home/ec2-user/.local/bin:$PATH
   ```

   Add this command to the end of `~/.bashrc` to maintain the change between sessions\.

1. Verify the new version with `aws --version`\.

   ```
   $ aws --version
   aws-cli/1.11.85 Python/2.7.12 Linux/4.9.20-11.31.amzn1.x86_64 botocore/1.5.48
   ```