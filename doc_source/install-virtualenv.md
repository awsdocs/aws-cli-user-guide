# Install the AWS CLI in a Virtual Environment<a name="install-virtualenv"></a>

You can avoid requirement version conflicts with other `pip` packages by installing the AWS Command Line Interface \(AWS CLI\) in a virtual environment\.

**To install the AWS CLI in a virtual environment**

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
   $ virtualenv -p /usr/bin/python3.4 ~/cli-ve
   ```

1. Activate your new virtual environment\.

   **Linux, macOS, or Unix**

   ```
   $ source ~/cli-ve/bin/activate
   ```

   **Windows**

   ```
   $ %USERPROFILE%\cli-ve\Scripts\activate
   ```

1. Install the AWS CLI into your virtual environment\.

   ```
   (cli-ve)~$ pip install --upgrade awscli
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.61
   ```

You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, you must reactivate the environment\.

To upgrade to the latest version, run the installation command again\.

```
(cli-ve)~$ pip install --upgrade awscli
```