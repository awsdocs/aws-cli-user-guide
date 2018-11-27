# Set an Initial Password for an IAM User<a name="cli-iam-set-pw"></a>

The following example demonstrates how to use the `create-login-profile` command to set an initial password for an IAM user\.

```
$ aws iam create-login-profile --user-name MyUser --password My!User1Login8P@ssword
{
    "LoginProfile": {
        "UserName": "MyUser",
        "CreateDate": "2013-01-02T21:10:54.339Z",
        "MustChangePassword": "false"
    }
}
```

Use the `update-login-profile` command to update the password for an IAM user\.