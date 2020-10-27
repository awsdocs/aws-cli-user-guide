# Using the AWS CLI version 1 with earlier versions of Python<a name="deprecate-old-python-versions"></a>

On January 10th, 2020, AWS CLI version 1, which requires a separate installation of Python to operate, dropped support for Python versions 2\.6 and 3\.3\. All builds of AWS CLI version 1 released after January 10th, 2020, starting with version 1\.17, require Python 2\.7, Python 3\.4, or a later version to successfully use the AWS CLI\.

This change does not affect the following versions of the AWS CLI:
+ **Windows MSI installer version of the AWS CLI version 1\.** The Windows MSI installer of the AWS CLI version 1 installation includes and uses its own embedded copy of Python, independent of any other Python version that you might have installed\. If you're using an MSI installer\-based AWS CLI, no changes are required\.
+ **AWS CLI version 2\.** The installers for AWS CLI version 2 all include and use an embedded copy of Python, independent of any other Python version that you might have installed\. If you're using AWS CLI version 2, no changes are required\.

For more information, see the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

To use an earlier, unsupported version of Python, such as Python 2\.6 or Python 3\.3, with the AWS CLI version 1, you must use a copy of AWS CLI version 1 that was released before January 10th, 2020, and prevent it from being updated to a later version\. Using an earlier version of the AWS CLI version 1 prevents you from accessing new services or features that were added to the AWS CLI after the date that your earlier version was initially released\. We recommend that whenever possible, you upgrade your Python version to a supported version, and use a later version of the AWS CLI version 1\.

**pip**

You can force `pip` to download an AWS CLI version 1 version that is compatible with Python 2\.6 or Python 3\.3 by using a command that specifies `awscli<1.17`, similar to the following example\. 

```
$ pip3 install --upgrade --user awscli<1.17
```

If you install the AWS CLI version 1 using a [pip Requirements file](https://pip.pypa.io/en/stable/user_guide/#requirements-files), include a line similar to the following\.

```
awscli<1.17
```

**Bundled installer on Linux or macOS**

Download and save a copy of the bundled installer that includes a version of the AWS CLI version 1 that is compatible with the version of Python that you want to use\. You can use the following URL format to download the file, replacing *\{VERSION\}* with the version number that you want to use, as shown\. Version numbers less than 1\.17 support the earlier Python releases\.

```
https://s3.amazonaws.com/aws-cli/awscli-bundle-{VERSION}.zip
```

For example, the following command downloads the AWS CLI version 1\.16\.312\. 

```
$ curl https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip -o awscli-bundle.zip 
```

From here, you can continue to follow the installation instructions, after the step to download the installer\.
+ [Install, update and uninstall the AWS CLI version 1 on macOS using the bundled installer](install-macos.md#install-macosos-bundled)
+ [Install and uninstall the AWS CLI version 1 on Linux using the bundled installer](install-linux.md#install-linux-bundled)