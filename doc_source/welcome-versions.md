# About the AWS CLI versions<a name="welcome-versions"></a>

The AWS CLI is available in two versions and information in this guide applies to both versions unless stated otherwise\. To check which version you may have currently installed, run the `aws --version` command in your shell\. The returned value provides the current version you have installed\. The following example shows that the version running is 2\.1\.29\.

```
$ aws --version
aws-cli/2.1.29 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.0.0
```

## AWS CLI version 2<a name="welcome-versions-v2"></a>

The AWS CLI version 2 is the most recent major version of the AWS CLI and supports all of the latest features\. Some features introduced in version 2 are not backported to version 1 and you must upgrade to access those features\. There are some "breaking" changes from version 1 that might require you to change your scripts\. For a list of breaking changes in version 2, see [Breaking changes – Migrating from AWS CLI version 1 to version 2](cliv2-migration.md)\.

The AWS CLI version 2 is available to install only as a bundled installer\. While you may find it in package managers, these are unsupported and unofficial packages that are not produced or managed by AWS\. We recommend that you install the AWS CLI from only the official AWS distribution points, as documented in this guide\. 

To install the AWS CLI version 2, see [Installing, updating, and uninstalling the AWS CLI version 2](install-cliv2.md)\.

For version history, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

## AWS CLI version 1<a name="welcome-versions-v1"></a>

**Warning**  
Python 2\.7 was deprecated by the Python Software Foundation on January 1, 2020\. Going forward, customers using the AWS CLI version 1 should transition to using Python 3, with a minimum of Python 3\.6\. Python 2\.7 support is deprecated for new versions of the AWS CLI version 1 starting 7/19/2021\. Python 3\.4 and 3\.5 is deprecated starting 2/1/2021\.  
To continue using AWS CLI version 1 with older versions of Python, see the Python version support matrix below\.  
For Python installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.

The AWS CLI version 1 is the original AWS CLI, and we continue to support it\. However, major new features that are introduced in the AWS CLI version 2 might not be backported to the AWS CLI version 1\. To use those features, you must install the AWS CLI version 2\.

The AWS CLI version 1 is built using the Python SDK, and therefore requires you to install a compatible version of Python\.


**Python version support matrix**  

| AWS CLI version | Supported Python version | 
| --- | --- | 
| Versions starting 7/19/2021 | Python 3\.6\+ | 
| 1\.19\.0 – current | Python 2\.7\+, Python 3\.6\+ | 
| 1\.17 – 1\.18\.x | Python 2\.7\+, Python 3\.4\+ | 
| 1\.0 – 1\.16\.x | Python 2\.6 and older, Python 3\.3 and older | 

To install the AWS CLI version 1, see [Installing, updating, and uninstalling the AWS CLI version 1](install-cliv1.md)\.

For version history, see the [AWS CLI version 1 Changelog](https://github.com/aws/aws-cli/blob/develop/CHANGELOG.rst) on *GitHub*\.