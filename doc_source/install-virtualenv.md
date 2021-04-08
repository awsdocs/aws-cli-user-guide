# Install and Update the AWS CLI version 1 in a virtual environment<a name="install-virtualenv"></a>

You can avoid requirement version conflicts with other `pip` packages by installing version 1 of the AWS Command Line Interface \(AWS CLI\) in a virtual environment\.

**Topics**
+ [Prerequisites](#install-virtualenv-prereqs)
+ [Install and update the AWS CLI version 1 in a virtual environment](#install-virtualenv-install)

## Prerequisites<a name="install-virtualenv-prereqs"></a>
+ Python 2 version 2\.7 or later, or Python 3 version 3\.6 or later\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.
**Warning**  
As of 2/1/2021 Python 3\.4 and 3\.5 is deprecated\.  
Python 2\.7 was deprecated by the [Python Software Foundation](https://www.python.org/psf-landing/) on January 1, 2020\. Going forward, customers using the AWS CLI version 1 should transition to using Python 3, with a minimum of Python 3\.6\. Python 2\.7 support is deprecated for new versions of the AWS CLI version 1 starting 7/19/2021\.  
In order to use the AWS CLI version 1 with an older version of Python, you need to install an earlier version of the AWS CLI version 1\.  
To view the AWS CLI version 1 Python version support matrix, see [About the AWS CLI versions](welcome-versions.md)\. 
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
   aws-cli/1.19.3 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

1. You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, you must reactivate the environment\.