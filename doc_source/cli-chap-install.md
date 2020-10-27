# Installing the AWS CLI<a name="cli-chap-install"></a>

The AWS Command Line Interface \(AWS CLI\) is available in two versions\.

To check which version you have installed, run the `aws --version`\. The returned value provides the current version you have installed\. The following example shows that the version running is 2\.0\.47\.

```
$ aws --version
aws-cli/2.0.47 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.0.0
```

## AWS CLI version 2<a name="cli-chap-install-v2"></a>

The AWS CLI version 2 is the most recent major version of the AWS CLI and supports all of the latest features\. Some features introduced in version 2 are not backported to version 1 and you must upgrade to access those features\.

The AWS CLI version 2 is available to install only as a bundled installer\. Although you might find it in some package managers, these are not produced or managed by AWS, and therefore are not official packages and not supported by AWS\. We recommend that you install the AWS CLI from only the official AWS distribution points, as documented in this guide\.

For more information, see [Installing the AWS CLI version 2](install-cliv2.md)\.

## AWS CLI version 1<a name="cli-chap-install-v1"></a>

The AWS CLI version 1 is the original AWS CLI, and we continue to support it\. However, major new features that are introduced in the AWS CLI version 2 might not be backported to the AWS CLI version 1\. To use those features, you must install the AWS CLI version 2\.

For more information, see [Installing the AWS CLI version 1](install-cliv1.md)\.

## Migrating from the AWS CLI version 1 to version 2<a name="migrating"></a>

If you ran commands or scripts with the AWS CLI version 1 and are considering migrating to the AWS CLI version 2, see [Breaking changes – Migrating from AWS CLI version 1 to version 2](cliv2-migration.md) for a description of changes to be aware of\.