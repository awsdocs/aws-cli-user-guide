--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Install, Update, and Uninstall the AWS CLI version 1 on Amazon Linux<a name="install-linux-al2017"></a>

The AWS CLI version 1 is preinstalled on Amazon Linux and Amazon Linux 2\. Check the currently installed version by using the following command\.

```
$ aws --version
aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
```

Depending on when you created your Amazon Linux instance, the AWS CLI version 1 is preinstalled using one of the following package managers:
+ [pip](#install-amazon-linux-pip)
+ [yum](#install-amazon-linux-yum)

## Prerequisites<a name="install-amazon-linux-prereq"></a>

You must have Python 3\.6 or later installed\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.

## Install, update, or uninstall using pip<a name="install-amazon-linux-pip"></a>

Most Amazon Linux instances use pip to preinstall the AWS CLI version 1\.

### Install or update the AWS CLI version 1 on Amazon Linux using pip<a name="install-amazon-linux-pip-install"></a>

To install the latest version of the AWS CLI version 1 for the current user, use the following instructions\.

1. We recommend that if you have Python version 3 or later installed that you use `pip3`\. Use `pip3 install` to install or update to the latest version of the AWS CLI version 1\. If you run the command from within a [Python virtual environment \(venv\)](https://docs.python.org/3/library/venv.html), you don't need to use the `--user` option\. 

   ```
   $ pip3 install --upgrade --user awscli
   ```

1. Ensure the folder that contains `aws` is part of your `PATH` variable\.

   1. Find your shell's profile script in your user directory\. If you're not sure which shell you have, run `echo $SHELL`\.

      ```
      $ ls -a ~
      .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
      ```
      + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`
      + **Zsh** – `.zshrc`
      + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`

   1. Add an export command at the end of your profile script that's similar to the following example\.

      ```
      export PATH=$HOME/.local/bin:$PATH
      ```

      This command inserts the path, `$HOME/.local/bin` in this example, at the front of the existing `$PATH`variable\.

   1. Reload the profile into your current session to put those changes into effect\.

      ```
      $ source ~/.bash_profile
      ```

1. To verify that you're running the new version, use the `aws --version` command\.

   ```
   $ aws --version
   aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

### Uninstall the AWS CLI version 1 using pip<a name="install-amazon-linux-pip-uninstall"></a>

If you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ pip3 uninstall awscli
```

## Install, update, or uninstall using yum<a name="install-amazon-linux-yum"></a>

Most Amazon Linux 2 instances use yum to preinstall the AWS CLI version 1\.

### Install or update the AWS CLI version 1 on Amazon Linux using yum<a name="install-amazon-linux-yum-install"></a>

To install the latest version of the AWS CLI version 1 , run the following command\.

```
$ sudo yum install awscli
```

To update to the latest version of the AWS CLI version 1 , run the following command\.

```
$ sudo yum update awscli
```

To verify that you're running the new version, use the `aws --version` command\.

```
$ aws --version
aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
```

### Uninstall the AWS CLI version 1 using yum<a name="install-amazon-linux-yum-uninstall"></a>

Some newer images of Amazon Linux 2 use yum to install the Amazon Linux 2 you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ sudo yum remove awscli
```

## Troubleshooting AWS CLI install and uninstall errors<a name="install-amazon-linux-tshoot"></a>

If you come across issues after installing or uninstalling the AWS CLI, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md) for troubleshooting steps\. For the most relevant troubleshooting steps, see [Command not found errors](cli-chap-troubleshooting.md#tshoot-install-not-found), [The "`aws --version`" command returns a different version than you installed](cli-chap-troubleshooting.md#tshoot-install-wrong-version), and [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\.