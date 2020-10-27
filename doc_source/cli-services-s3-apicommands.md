# Using API\-Level \(s3api\) commands with the AWS CLI<a name="cli-services-s3-apicommands"></a>

The API\-level commands \(contained in the `s3api` command set\) provide direct access to the Amazon Simple Storage Service \(Amazon S3\) APIs, and enable some operations that are not exposed in the high\-level `s3` commands\. These commands are the equivalent of the other AWS services that provide API\-level access to the services' functionality\.

This topic provides examples that demonstrate how to use the lower\-level commands that map to the Amazon S3 APIs\. In addition, you can find examples for each S3 API command in the [s3api section of the CLI Reference Guide](https://docs.aws.amazon.com/cli/latest/reference/s3api/)\.

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

## Apply a custom ACL<a name="cli-services-s3-apicommands-acls"></a>

With high\-level commands, you can use the `--acl` option to apply predefined access control lists \(ACLs\) to Amazon S3 objects\. But you can't use that command to set bucket\-wide ACLs\. However, you can do this by using the [put\-bucket\-acl](https://docs.aws.amazon.com/cli/latest/reference/s3api/put-bucket-acl.html) API\-level command\. 

The following example shows how to grant full control to two AWS users \(*user1@example\.com* and *user2@example\.com*\) and read permission to everyone\. The identifier for "everyone" comes from a special URI that you pass as a parameter\.

```
$ aws s3api put-bucket-acl --bucket MyBucket --grant-full-control 'emailaddress="user1@example.com",emailaddress="user2@example.com"' --grant-read 'uri="http://acs.amazonaws.com/groups/global/AllUsers"'
```

For details about how to construct the ACLs, see [PUT Bucket acl](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTacl.html) in the *Amazon Simple Storage Service API Reference*\. The `s3api` ACL commands in the CLI, such as `put-bucket-acl`, use the same [shorthand argument notation](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-shorthand.html)\.

## Configure a logging policy<a name="cli-services-s3-apicommands-logpol"></a>

The API command `put-bucket-logging` configures a bucket logging policy\. 

In the following example, the AWS user *user@example\.com* is granted full control over the log files, and all users have read access to them\. Notice that the `put-bucket-acl` command is also required to grant the Amazon S3 log delivery system \(specified by a URI\) the permissions needed to read and write the logs to the bucket\.

```
$ aws s3api put-bucket-acl --bucket MyBucket --grant-read-acp 'URI="http://acs.amazonaws.com/groups/s3/LogDelivery"' --grant-write 'URI="http://acs.amazonaws.com/groups/s3/LogDelivery"'
$ aws s3api put-bucket-logging --bucket MyBucket --bucket-logging-status file://logging.json
```

The `logging.json` file in the previous command has the following content\.

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