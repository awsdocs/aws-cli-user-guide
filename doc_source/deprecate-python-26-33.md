# Using the AWS CLI with Python 2\.6 or Python 3\.3<a name="deprecate-python-26-33"></a>

**Important**  
On January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI with Python 2\.6 or Python 3\.3](#deprecate-python-26-33) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

AWS CLI version 1 requires that you have a version of Python installed\. This Python installation can be any supported version of Python\. Because Python 2\.6 and Python 3\.3 are no longer supported and are no longer receiving security updates, we are deprecating support for Python 2\.6 and Python 3\.3 with the AWS CLI\. We strongly recommend that you to upgrade to a later version of Python\. 

Starting on January 10th, 2020, AWS CLI releases version 1\.17 and later will work only with later versions of Python\.

To continue using Python 2\.6 or Python 3\.3 with the AWS CLI, you must "pin" your current installation to an older version of the AWS CLI, as described in the following sections\. Pinning your current version prevents it from being updated to a later version\. 

**Note**  
Using an earlier version of the AWS CLI prevents you from accessing new services or features that were added to the AWS CLI after the date that your earlier version was initially released\. We recommend that whenever possible, you upgrade your Python version to a supported version, and use a later version of the AWS CLI\.

**Pip**

You can force `pip` to download an AWS CLI version that is compatible with Python 2\.6 or Python 3\.3 by using a command that specifies `awscli<1.17`, similar to the following example\. 

```
$ pip3 install --upgrade --user awscli<1.17
```

If you install the AWS CLI using a [pip Requirements file](https://pip.pypa.io/en/stable/user_guide/#requirements-files), include a line similar to the following\.

```
awscli<1.17
```

**Bundled installer on Linux or macOS**

You can pin your installation to a specific earlier version with the bundled installer\. To do this, download and save a copy of the bundled installer that includes a version of the AWS CLI that is compatible with the version of Python that you want to use\. You can use the following URL format to download the file, replacing *\{VERSION\}* with the version number that you want to use, as shown\. Version numbers less than 1\.17 support the older Python releases\.

```
https://s3.amazonaws.com/aws-cli/awscli-bundle-{VERSION}.zip
```

For example, the following command downloads the AWS CLI version 1\.16\.188\. 

```
$ curl [https://s3\.amazonaws\.com/aws\-cli/awscli\-bundle\-1\.16\.188\.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.188.zip) -o awscli-bundle.zip 
```

From here, you can continue to follow the installation instructions in [Install the AWS CLI Using the Bundled Installer \(Linux, macOS, or Unix\)](install-bundle.md), starting with step 2\.

**MSI installer on Windows** 

The MSI installer version of the AWS CLI for Windows is not affected by this deprecation\. This version of the AWS CLI installation includes and uses its own embedded copy of Python, independent of any other Python version you might have installed\. If you're using an MSI installer\-based AWS CLI, no changes are required\.