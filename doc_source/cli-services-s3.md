# Using Amazon S3 with the AWS CLI<a name="cli-services-s3"></a>

You can access the features of Amazon Simple Storage Service \(Amazon S3\) using the AWS Command Line Interface \(AWS CLI\)\. 

Before you run any commands, set your default credentials\. For more information, see [Configuration basics](cli-configure-quickstart.md)\.

The AWS CLI provides two tiers of commands for accessing Amazon S3:
+ The *s3* tier consists of high\-level commands that simplify performing common tasks, such as creating, manipulating, and deleting objects and buckets\.
+ The *s3api* tier behaves identically to other AWS services by exposing direct access to all Amazon S3 API operations\. It enables you to carry out advanced operations that might not be possible with the following tier's high\-level commands alone\.

To get a list of all of the commands available in each tier, use the `help` argument with the `aws s3api` or `aws s3` commands\.

```
$ aws s3 help
```

```
$ aws s3api help
```

**Note**  
The AWS CLI supports copying, moving, and syncing from Amazon S3 to Amazon S3 using the *server\-side* `COPY` operation provided by Amazon S3\. This means that your files are kept in the cloud, and are *not* downloaded to the client machine, then back up to Amazon S3\.  
When operations such as these can be performed completely in the cloud, only the bandwidth necessary for the HTTP request and response is used\.

**Topics**
+ [Using high\-level \(s3\) commands with the AWS CLI](cli-services-s3-commands.md)
+ [Using API\-Level \(s3api\) commands with the AWS CLI](cli-services-s3-apicommands.md)