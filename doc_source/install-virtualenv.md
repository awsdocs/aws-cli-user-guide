# Install and Update the AWS CLI version 1 in a virtual environment<a name="install-virtualenv"></a>

You can avoid requirement version conflicts with other `pip` packages by installing version 1 of the AWS Command Line Interface \(AWS CLI\) in a virtual environment\.

**Topics**
+ [Prerequisites](#install-virtualenv-prereqs)
+ [Install and update the AWS CLI version 1 in a virtual environment](#install-virtualenv-install)

## Prerequisites<a name="install-virtualenv-prereqs"></a>
+ Python 2 version 2\.7 or later, or Python 3 version 3\.4 or later\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.
**Important**  
AWS CLI version 1 no longer supports Python versions 2\.6 and 3\.3\. All versions of the AWS CLI version 1 released after January 10th, 2020, starting with 1\.17, require Python 2\.7, Python 3\.4, or a later version\.  
This change does not affect the Windows MSI installer version of the AWS CLI version 1 and the AWS CLI version 2\.  
For more information, see [Using the AWS CLI version 1 with earlier versions of Python](deprecate-old-python-versions.md) in this guide, and the [deprecation announcement](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/) blog post\.
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
   aws-cli/1.18.134 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

1. You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, you must reactivate the environment\.