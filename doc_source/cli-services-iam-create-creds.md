# Create an access key for an IAM user<a name="cli-services-iam-create-creds"></a>

This topic describes how to use AWS Command Line Interface \(AWS CLI\) commands to create a set of access keys for an AWS Identity and Access Management \(IAM\) user\. For more information on the IAM service, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

You can use the [https://docs.aws.amazon.com/cli/latest/reference/iam/create-access-key.html](https://docs.aws.amazon.com/cli/latest/reference/iam/create-access-key.html) command to create an access key for an IAM user\. An access key is a set of security credentials that consists of an access key ID and a secret key\. 

An IAM user can create only two access keys at one time\. If you try to create a third set, the command returns a `LimitExceeded` error\.

```
$ aws iam create-access-key --user-name MyUser
{
    "AccessKey": {
        "UserName": "MyUser",
        "AccessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "Status": "Active",
        "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
        "CreateDate": "2018-12-14T17:34:16Z"
    }
}
```

Use the [https://docs.aws.amazon.com/cli/latest/reference/iam/delete-access-key.html](https://docs.aws.amazon.com/cli/latest/reference/iam/delete-access-key.html) command to delete an access key for an IAM user\. Specify which access key to delete by using the access key ID\.

```
$ aws iam delete-access-key --user-name MyUser --access-key-id AKIAIOSFODNN7EXAMPLE
```