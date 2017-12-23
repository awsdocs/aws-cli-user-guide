# Install the AWS Command Line Interface in a Virtual Environment<a name="awscli-install-virtualenv"></a>

You can avoid requirement version conflicts with other pip packages by installing the AWS CLI in a virtual environment\.

**To install the AWS CLI in a virtual environment**

1. Install `virtualenv` with pip\.

   ```
   $ pip install --user virtualenv
   ```

1. Create a virtual environment\.

   ```
   $ virtualenv ~/cli-ve
   ```

   You can use the `-p` option to use a Python executable other than the default\.

   ```
   $ virtualenv -p /usr/bin/python3.4 ~/cli-ve
   ```

1. Activate the virtual environment\.

   **Linux, macOS, or Unix**

   ```
   $ source ~/cli-ve/bin/activate
   ```

   **Windows**

   ```
   $ %USERPROFILE%\cli-ve\Scripts\activate
   ```

1. Install the AWS CLI\.

   ```
   (cli-ve)~$ pip install --upgrade awscli
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   aws-cli/1.11.84 Python/3.6.2 Linux/4.4.0-59-generic botocore/1.5.47
   ```

You can use the `deactivate` command to exit the virtual environment\. Whenever you start a new session, run the activation command again\.

To upgrade to the latest version, run the installation command again:

```
(cli-ve)~$ pip install --upgrade awscli
```