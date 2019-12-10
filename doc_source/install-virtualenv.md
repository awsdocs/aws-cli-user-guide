# Install the AWS CLI version 1 in a Virtual Environment<a name="install-virtualenv"></a>

You can avoid requirement version conflicts with other `pip` packages by installing version 1 of the AWS Command Line Interface \(AWS CLI\) in a virtual environment\.

**Important**  
On January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI version 1 with Python 2\.6 or Python 3\.3](deprecate-python-26-33.md) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

**To install the AWS CLI version 1 in a virtual environment**

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

1. Install the AWS CLI version 1 into your virtual environment\.

   ```
   (cli-ve)~$ pip install --upgrade awscli
   ```

1. Verify that the AWS CLI version 1 is installed correctly\.

   ```
   $ aws --version
   aws-cli/1.16.273 Python/3.7.3 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13.0
   ```

You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, you must reactivate the environment\.

To upgrade to the latest version, run the installation command again\.

```
(cli-ve)~$ pip install --upgrade awscli
```