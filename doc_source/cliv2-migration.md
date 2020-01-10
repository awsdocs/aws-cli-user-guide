# Migrating from AWS CLI version 1 to version 2<a name="cliv2-migration"></a>

This topic describes the changes in behavior between AWS CLI version 1 and AWS CLI version 2 that might require you to make changes to scripts or commands to get the same behavior in version 2 as you did in version 1\.

## AWS CLI version 2 no longer automatically retrieves `http://` or `https://` URLs for parameters<a name="cliv2-migration-paramfile"></a>

The AWS CLI version 2 no longer performs a GET operation when a parameter value begins with `http://` or `https://`, and then using the returned content as the value of the parameter\. If you need to retrieve a URL and pass the contents read from that URL as the value of a parameter, we recommend that you use `curl` or a similar tool to download the contents of the URL to a local file\. Then use the `file://` syntax to read the contents of that file and use it as the parameter's value\. 

For example, the following command no longer tries to retrieve the contents of the page found at `http://www.google.com` and pass those contents as the parameter\. Instead, it passes the literal text string `https://google.com` as the parameter\.

```
$ aws2 ssm put-parameter --value http://www.google.com --name prod.microservice1.db.secret --type String 2
```

If you really do want to retrieve and use the contents of a web URL as a parameter, you can do the following in version 2\.

```
$ curl https://my.example.com/mypolicyfile.json -o mypolicyfile.json
$ aws iam put-role-policy --policy-document file://./mypolicyfile.json --role-name MyRole --policy-name MyReadOnlyPolicy
```

In the previous example, the `-o` parameter tells `curl` to save the file in the current folder with the same name as the source file\. The second command retrieves the content of that downloaded file and passes the content as the value of `--policy-document`\.

## AWS CLI version 2 now returns all timestamp output values in ISO 8601 format<a name="cliv2-migration-timestamp"></a>

By default, AWS CLI version 2 returns all timestamp response values in the [ISO 8601 format](https://wikipedia.org/wiki/ISO_8601)\. In AWS CLI version 1, commands returned timestamp values in whatever format was returned by the HTTP API response, which could vary from service to service\. 

ISO 8601 formatted timestamps look like the following examples\. The first example shows the time in [Coordinated Universal Time \(UTC\)](https://wikipedia.org/wiki/Coordinated_Universal_Time) by including a `Z` after the time\. The date and the time are separated by a `T`\.

```
2019-10-31T22:21:41Z
```

To specify a different time zone, instead of the `Z`, specify a `+` or `-` and the number of hours the desired time zone is ahead of or behind UTC, as a two\-digit value\. The following example shows the same time as the previous example but adjusted to Pacific Standard time, which is eight hours behind UTC\.

```
2019-10-31T14:21:41-08
```

To see timestamps in the format returned by the HTTP API response, add the following line to your `.aws/config` profile\.

```
cli_timestamp_format = wire
```

## AWS CLI version 2 no longer supports "hidden" aliases<a name="cliv2-migration-aliases"></a>

AWS CLI version 2 no longer supports the following hidden aliases that were supported in version 1\. 

In the following table, the first column displays the service, command, and parameter that work in all versions, including AWS CLI version 2\. The second column displays the alias that no longer works in AWS CLI version 2


| Working Service, Command, and Parameter | Obsolete Alias | 
| --- | --- | 
| cognito\-identity create\-identity\-pool open\-id\-connect\-provider\-arns | open\-id\-connect\-provider\-ar\-ns | 
| storagegateway describe\-tapes tape\-arns | tape\-ar\-ns | 
| storagegateway\.describe\-tape\-archives\.tape\-arns | tape\-ar\-ns | 
| storagegateway\.describe\-vtl\-devices\.vtl\-device\-arns | vtl\-device\-ar\-ns | 
| storagegateway\.describe\-cached\-iscsi\-volumes\.volume\-arns | volume\-ar\-ns | 
| storagegateway\.describe\-stored\-iscsi\-volumes\.volume\-arns | volume\-ar\-ns | 
| route53domains\.view\-billing\.start\-time | start | 
| deploy\.create\-deployment\-group\.ec2\-tag\-set | ec\-2\-tag\-set | 
| deploy\.list\-application\-revisions\.s3\-bucket | s\-3\-bucket | 
| deploy\.list\-application\-revisions\.s3\-key\-prefix | s\-3\-key\-prefix | 
| deploy\.update\-deployment\-group\.ec2\-tag\-set | ec\-2\-tag\-set | 
| iam\.enable\-mfa\-device\.authentication\-code1 | authentication\-code\-1 | 
| iam\.enable\-mfa\-device\.authentication\-code2 | authentication\-code\-2 | 
| iam\.resync\-mfa\-device\.authentication\-code1 | authentication\-code\-1 | 
| iam\.resync\-mfa\-device\.authentication\-code2 | authentication\-code\-2 | 
| importexport\.get\-shipping\-label\.street1 | street\-1 | 
| importexport\.get\-shipping\-label\.street2 | street\-2 | 
| importexport\.get\-shipping\-label\.street3 | street\-3 | 
| lambda\.publish\-version\.code\-sha256 | code\-sha\-256 | 
| lightsail\.import\-key\-pair\.public\-key\-base64 | public\-key\-base\-64 | 
| opsworks\.register\-volume\.ec2\-volume\-id | ec\-2\-volume\-id | 

## AWS CLI version 2 uses Amazon S3 keys more consistently<a name="cliv2-migration-s3-keys"></a>

For the Amazon S3 customization commands in the `s3` namespace, we improved the consistency of how paths are shown\. In the AWS CLI version 2, paths are always displayed relative to the relevant key\. The AWS CLI version 1 sometimes showed paths in absolute form and sometimes in relative form\. 

## AWS CLI version 2 currently does not support the `[plugins]` section in the AWS `config` file<a name="cliv2-migration-profile-plugins"></a>

AWS CLI version 2 does not support the `[plugins]` section of the `~/.aws/config` file\. Version 1 plugins do not work in version 2, and can cause failures\. We recommend that you either remove the section or rename it\.