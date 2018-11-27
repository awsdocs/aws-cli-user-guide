# Using Amazon S3 with the AWS Command Line Interface<a name="cli-s3"></a>

The AWS CLI provides two tiers of commands for accessing Amazon S3\.
+ The first tier, named *s3*, consists of high\-level commands for frequently used operations, such as creating, manipulating, and deleting objects and buckets\.
+ The second tier, named *s3api*, exposes all Amazon S3 operations, including modifying a bucket access control list \(ACL\), using cross\-origin resource sharing \(CORS\), or logging policies\. It allows you to carry out advanced operations that may not be possible with the high\-level commands alone\.

To get a list of all commands available in each tier, use the `help` argument with the `aws s3` or `aws s3api` commands:

```
$ aws s3 help
```

or

```
$ aws s3api help
```

**Note**  
The AWS CLI supports copying, moving, and syncing from Amazon S3 to Amazon S3\. These operations use the *service\-side* COPY operation provided by Amazon S3: Your files are kept in the cloud, and are *not* downloaded to the client machine, then back up to Amazon S3\.  
When operations such as these can be performed completely in the cloud, only the bandwidth necessary for the HTTP request and response is used\.

For examples of Amazon S3 usage, see the following topics in this section\.

**Topics**
+ [Using High\-Level s3 Commands with the AWS Command Line Interface](using-s3-commands.md)
+ [Using API\-Level \(s3api\) Commands with the AWS Command Line Interface](using-s3api-commands.md)