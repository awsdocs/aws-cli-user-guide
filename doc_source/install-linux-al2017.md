# Install, Update, and Uninstall the AWS CLI version 1 on Amazon Linux<a name="install-linux-al2017"></a>

The AWS CLI version 1 is preinstalled on Amazon Linux and Amazon Linux 2\. Check the currently installed version by using the following command\.

```
$ aws --version
aws-cli/1.19.3 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
```

**Topics**
+ [Prerequisites](#install-amazon-linux-prereq)
+ [Install or update the AWS CLI version 1 on Amazon Linux using pip](#install-amazon-linux-pip)
+ [Uninstall the AWS CLI version 1 using pip](#install-amazon-linux-uninstall)

## Prerequisites<a name="install-amazon-linux-prereq"></a>

You must have Python 2 version 2\.7 or later, or Python 3 version 3\.6 or later installed\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.

**Warning**  
As of 2/1/2021 Python 3\.4 and 3\.5 is deprecated\.  
Python 2\.7 was deprecated by the [Python Software Foundation](https://www.python.org/psf-landing/) on January 1, 2020\. Going forward, customers using the AWS CLI version 1 should transition to using Python 3, with a minimum of Python 3\.6\. Python 2\.7 support is deprecated for new versions of the AWS CLI version 1 starting 7/19/2021\.  
In order to use the AWS CLI version 1 with an older version of Python, you need to install an earlier version of the AWS CLI version 1\.  
To view the AWS CLI version 1 Python version support matrix, see [About the AWS CLI versions](welcome-versions.md)\. 

## Install or update the AWS CLI version 1 on Amazon Linux using pip<a name="install-amazon-linux-pip"></a>

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
   aws-cli/1.19.3 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

## Uninstall the AWS CLI version 1 using pip<a name="install-amazon-linux-uninstall"></a>

If you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ pip3 uninstall awscli
```