# Create Security Credentials for an IAM User<a name="cli-iam-create-creds"></a>

 The following example uses the `create-access-key` command to create security credentials for an IAM user\. A set of security credentials comprises an access key ID and a secret key\. Note that an IAM user can have no more than two sets of credentials at any given time\. If you attempt to create a third set, the `create-access-key` command will return a "LimitExceeded" error\.

```
$ aws iam create-access-key --user-name MyUser
{
    "AccessKey": {
        "SecretAccessKey": "je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY",
        "Status": "Active",
        "CreateDate": "2013-01-02T22:44:12.897Z",
        "UserName": "MyUser",
        "AccessKeyId": "AKIAI44QH8DHBEXAMPLE"
    }
}
```

Use the `delete-access-key` command to delete a set of credentials for an IAM user\. Specify which credentials to delete by using the access key ID\.

```
$ aws iam delete-access-key --user-name MyUser --access-key-id AKIAI44QH8DHBEXAMPLE
```