--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Install and Update the AWS CLI version 1 in a virtual environment<a name="install-virtualenv"></a>

You can avoid requirement version conflicts with other `pip` packages by installing version 1 of the AWS Command Line Interface \(AWS CLI\) in a virtual environment\.

**Topics**
+ [Prerequisites](#install-virtualenv-prereqs)
+ [Install and update the AWS CLI version 1 in a virtual environment](#install-virtualenv-install)
+ [Troubleshooting AWS CLI install and uninstall errors](#install-virtualenv-tshoot)

## Prerequisites<a name="install-virtualenv-prereqs"></a>
+ Python 3\.6 or later\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.
**Warning**  
Python 2\.7 was deprecated by the [Python Software Foundation](https://www.python.org/psf-landing/) on January 1, 2020\. Starting with AWS CLI version 1\.20\.0, a minimum version of Python 3\.6 is required\.  
In order to use the AWS CLI version 1 with an older version of Python, you need to install an earlier version of the AWS CLI version 1\. To view the AWS CLI version 1 Python version support matrix, see [Python version requirements](cli-chap-install.md#cli-chap-install-python)\. 
+ `pip` or `pip3` is installed\.

## Install and update the AWS CLI version 1 in a virtual environment<a name="install-virtualenv-install"></a>

1. Install `virtualenv` using `pip`\.

   ```
   $ pip install --user virtualenv
   ```

1. Create a virtual environment and name it\.

   ```
   $ virtualenv ~/cli-ve
   ```

   Alternatively, you can use the `-p` option to specify a version of Python other than the default\.

   ```
   $ virtualenv -p /usr/bin/python37 ~/cli-ve
   ```

1. Activate your new virtual environment\.

   **Linux or macOS**

   ```
   $ source ~/cli-ve/bin/activate
   ```

   **Windows**

   ```
   $ %USERPROFILE%\cli-ve\Scripts\activate
   ```

   The prompt changes to show that your virtual environment is active\.

   ```
   (cli-ve)~$
   ```

1. Install or update the AWS CLI version 1 into your virtual environment\.

   ```
   (cli-ve)~$ pip install --upgrade awscli
   ```

1. Verify that the AWS CLI version 1 is installed correctly\.

   ```
   $ aws --version
   aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

1. You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, you must reactivate the environment\.

## Troubleshooting AWS CLI install and uninstall errors<a name="install-virtualenv-tshoot"></a>

If you come across issues after installing or uninstalling the AWS CLI, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md) for troubleshooting steps\. For the most relevant troubleshooting steps, see [Command not found errors](cli-chap-troubleshooting.md#tshoot-install-not-found), [The "`aws --version`" command returns a different version than you installed](cli-chap-troubleshooting.md#tshoot-install-wrong-version), and [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\.