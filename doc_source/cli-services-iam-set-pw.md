# Setting an initial password for an IAM user<a name="cli-services-iam-set-pw"></a>

This topic describes how to use AWS Command Line Interface \(AWS CLI\) commands to set an initial password for an AWS Identity and Access Management\( IAM\) user\. For more information on the IAM service, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

The following command uses [create\-login\-profile](https://docs.aws.amazon.com/cli/latest/reference/iam/create-login-profile.html) to set an initial password on the specified user\. When the user signs in for the first time, the user is required to change the password to something that only the user knows\.

```
$ aws iam create-login-profile --user-name MyUser --password My!User1Login8P@ssword --password-reset-required
{
    "LoginProfile": {
        "UserName": "MyUser",
        "CreateDate": "2018-12-14T17:27:18Z",
        "PasswordResetRequired": true
    }
}
```

You can use the `update-login-profile` command to *change* the password for an IAM user\.

```
$ aws iam update-login-profile --user-name MyUser --password My!User1ADifferentP@ssword
```