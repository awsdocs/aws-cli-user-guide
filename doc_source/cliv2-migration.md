# Breaking Changes – Migrating from AWS CLI version 1 to version 2<a name="cliv2-migration"></a>

This topic describes the changes in behavior between AWS CLI version 1 and AWS CLI version 2 that might require you to make changes to scripts or commands to get the same behavior in version 2 as you did in version 1\.

**Topics**
+ [AWS CLI version 2 now passes binary parameters as base64\-encoded strings by default](#cliv2-migration-binaryparam)
+ [AWS CLI version 2 no longer automatically retrieves `http://` or `https://` URLs for parameters](#cliv2-migration-paramfile)
+ [AWS CLI version 2 now returns all timestamp output values in ISO 8601 format](#cliv2-migration-timestamp)
+ [AWS CLI version 2 improves handling of AWS CloudFormation deployments that result in no changes](#cliv2-migration-cfn)
+ [AWS CLI version 2 uses Amazon S3 keys more consistently](#cliv2-migration-s3-keys)
+ [AWS CLI version 2 uses the correct Amazon S3 regional endpoint for `us-east-1` Region](#cliv2-migration-s3-regional-endpoint)
+ [AWS CLI version 2 uses regional AWS STS endpoints by default](#cliv2-migration-sts-regional-endpoint)
+ [AWS CLI version 2 currently does not support the `[plugins]` section in the AWS `config` file](#cliv2-migration-profile-plugins)
+ [AWS CLI version 2 no longer supports "hidden" aliases](#cliv2-migration-aliases)

## AWS CLI version 2 now passes binary parameters as base64\-encoded strings by default<a name="cliv2-migration-binaryparam"></a>

AWS CLI version 1 didn't always make it easy to pass binary parameters from the output of one command to the input of another command without requiring some intermediate processing\. Some commands required [base64](https://wikipedia.org/wiki/Base64)\-encoded strings, others required UTF8\-encoded byte strings\. AWS CLI version 2 makes handling binary parameters more consistent to enable more reliable passing of values from one command to another\. 

By default, the AWS CLI version 2 now passes all binary input and binary output parameters as base64\-encoded strings\. A parameter that requires binary input has its type specified as `blob` \(binary large object\) in the documentation\. To pass binary data as a file to a CLI parameter, the AWS CLI version 2 enables you to specify the file using the following prefixes:
+ `file://` – The AWS CLI treats the file content as base64\-encoded text\. For example: `--some-param file://~/my/path/file-with-base64.txt`
+ `fileb://` – The AWS CLI treats the file content as unencoded binary\. For example: `--some-param fileb://~/my/path/file-with-raw-binary.bin`

You can tell the AWS CLI version 2 to revert to the AWS CLI version 1 behavior by specifying the following line in the `~/.aws/config` file for a profile\.

```
cli_binary_format=raw-in-base64-out
```

You can also revert the setting for an individual command, overriding the active profile setting, by including the parameter `--cli-binary-format raw-in-base64-out` on the command\-line\.

If you revert to the AWS CLI version 1 behavior and specify a file for a binary parameter using either `file://` or `fileb://`, the AWS CLI treats the file content as unencoded raw binary\.

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

## AWS CLI version 2 improves handling of AWS CloudFormation deployments that result in no changes<a name="cliv2-migration-cfn"></a>

In AWS CLI version 1, if you deployed a AWS CloudFormation template that resulted in no changes, by default, the AWS CLI failed with an error code\. This could be a problem if you didn't consider that to be an error and wanted your script to continue\. You could work around this in AWS CLI version 1, by adding the flag `-–no-fail-on-empty-changeset` which returns `0` and doesn't cause an error in your script\.

Because this is the common case scenario, the AWS CLI version 2 now defaults to returning a successful exit code of `0` when there is no change caused by the deployment and the operation returns an empty changeset\.

In AWS CLI version 2, to revert to the original behavior, you must add the new flag `--fail-on-empty-changeset`\.

## AWS CLI version 2 uses Amazon S3 keys more consistently<a name="cliv2-migration-s3-keys"></a>

For the Amazon S3 customization commands in the `s3` namespace, we improved the consistency of how paths are shown\. In the AWS CLI version 2, paths are always displayed relative to the relevant key\. The AWS CLI version 1 sometimes showed paths in absolute form and sometimes in relative form\. 

## AWS CLI version 2 uses the correct Amazon S3 regional endpoint for `us-east-1` Region<a name="cliv2-migration-s3-regional-endpoint"></a>

When you configure AWS CLI version 1 to use the `us-east-1` region, the AWS CLI used the global `s3.amazonaws.com` endpoint which was physically hosted in the `us-east-1` region\. AWS CLI version 2 now uses the true regional endpoint `s3.us-east-1.amazonaws.com` when that region is specified\. To force the AWS CLI version 2 to use the global endpoint, you can set the Region for a command to `aws-global`\.

## AWS CLI version 2 uses regional AWS STS endpoints by default<a name="cliv2-migration-sts-regional-endpoint"></a>

By default, AWS CLI version 2 sends all AWS STS API requests to the regional endpoint for the currently configured AWS Region\. 

By default, AWS CLI version 1 sends AWS STS requests to the global AWS STS endpoint\. You can control this default behavior in V1 by using the [sts\_regional\_endpoints](cli-configure-files.md#cli-config-sts_regional_endpoints) setting\.

## AWS CLI version 2 currently does not support the `[plugins]` section in the AWS `config` file<a name="cliv2-migration-profile-plugins"></a>

AWS CLI version 2 does not support the `[plugins]` section of the `~/.aws/config` file\. Version 1 plugins do not work in version 2, and can cause failures\. We recommend that you either remove the section or rename it\.

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