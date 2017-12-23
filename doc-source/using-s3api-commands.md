# Using API\-Level \(s3api\) Commands with the AWS Command Line Interface<a name="using-s3api-commands"></a>

The API\-level commands \(contained in the `s3api` command set\) provide direct access to the Amazon S3 APIs and enable some operations not exposed in the high\-level commands\. This section describes the API\-level commands and provides a few examples\. For more Amazon S3 examples, see the [s3api command\-line reference](http://docs.aws.amazon.com/cli/latest/reference/s3api/) and choose an available command from the list\.

## Custom ACLs<a name="using-s3api-commands-acls"></a>

With high\-level commands, you can use the `--acl` option to apply pre\-defined access control lists \(ACLs\) on Amazon S3 objects, but you cannot set bucket\-wide ACLs\. You can do this with the API\-level command, `put-bucket-acl`\. The following example grants full control to two AWS users \(*user1@example\.com* and *user2@example\.com*\) and read permission to everyone\.

```
$ aws s3api put-bucket-acl --bucket MyBucket --grant-full-control 'emailaddress="user1@example.com",emailaddress="user2@example.com"' --grant-read 'uri="http://acs.amazonaws.com/groups/global/AllUsers"'
```

For details about custom ACLs, see [PUT Bucket acl](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTacl.html)\. The `s3api` ACL commands, such as `put-bucket-acl`, use the same shorthand argument notation\.

## Logging Policy<a name="using-s3api-commands-logpol"></a>

The API command `put-bucket-logging` configures bucket logging policy\. The following example sets the logging policy for *MyBucket*\. The AWS user *user@example\.com* will have full control over the log files, and all users will have access to them\. Note that the `put-bucket-acl` command is required to grant Amazon S3's log delivery system the necessary permissions \(write and read\-acp\)\.

```
$ aws s3api put-bucket-acl --bucket MyBucket --grant-write 'URI="http://acs.amazonaws.com/groups/s3/LogDelivery"' --grant-read-acp 'URI="http://acs.amazonaws.com/groups/s3/LogDelivery"'
$ aws s3api put-bucket-logging --bucket MyBucket --bucket-logging-status file://logging.json
```

**logging\.json**

```
{
  "LoggingEnabled": {
    "TargetBucket": "MyBucket",
    "TargetPrefix": "MyBucketLogs/",
    "TargetGrants": [
      {
        "Grantee": {
          "Type": "AmazonCustomerByEmail",
          "EmailAddress": "user@example.com"
        },
        "Permission": "FULL_CONTROL"
      },
      {
        "Grantee": {
          "Type": "Group",
          "URI": "http://acs.amazonaws.com/groups/global/AllUsers"
        },
        "Permission": "READ"
      }
    ]
  }
}
```