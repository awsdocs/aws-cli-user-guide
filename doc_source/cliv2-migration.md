# Breaking changes – Migrating from AWS CLI version 1 to version 2<a name="cliv2-migration"></a>

This topic describes the changes in behavior between AWS CLI version 1 and AWS CLI version 2 that might require you to make changes to scripts or commands to get the same behavior in version 2 as you did in version 1\.

**Topics**
+ [AWS CLI version 2 now uses environment variable to set text file encoding](#cliv2-migration-encodingenvvar)
+ [AWS CLI version 2 now passes binary parameters as base64\-encoded strings by default](#cliv2-migration-binaryparam)
+ [AWS CLI version 2 improves Amazon S3 handling of file properties and tags when performing multipart copies](#cliv2-migration-s3-copy-metadata)
+ [AWS CLI version 2 no longer automatically retrieves `http://` or `https://` URLs for parameters](#cliv2-migration-paramfile)
+ [AWS CLI version 2 uses a paging program for all output by default\.](#cliv2-migration-output-pager)
+ [AWS CLI version 2 now returns all timestamp output values in ISO 8601 format](#cliv2-migration-timestamp)
+ [AWS CLI version 2 improves handling of AWS CloudFormation deployments that result in no changes](#cliv2-migration-cfn)
+ [AWS CLI version 2 uses Amazon S3 keys more consistently](#cliv2-migration-s3-keys)
+ [AWS CLI version 2 uses the correct Amazon S3 regional endpoint for `us-east-1` Region](#cliv2-migration-s3-regional-endpoint)
+ [AWS CLI version 2 uses regional AWS STS endpoints by default](#cliv2-migration-sts-regional-endpoint)
+ [AWS CLI version 2 replaces `ecr get-login` with `ecr get-login-password`](#cliv2-migration-ecr-get-login)
+ [AWS CLI version 2 support for plugins is changing](#cliv2-migration-profile-plugins)
+ [AWS CLI version 2 no longer supports "hidden" aliases](#cliv2-migration-aliases)

## AWS CLI version 2 now uses environment variable to set text file encoding<a name="cliv2-migration-encodingenvvar"></a>

By default, text files use the same encoding as the installed locale\. To set encoding for text files to be different from the locale, use the `AWS_CLI_FILE_ENCODING` environment variable\. The below example sets the CLI to open text files using `UTF-8` on windows\.

```
AWS_CLI_FILE_ENCODING=UTF-8
```

For more information, see [Environment variables to configure the AWS CLI](cli-configure-envvars.md) \.

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

## AWS CLI version 2 improves Amazon S3 handling of file properties and tags when performing multipart copies<a name="cliv2-migration-s3-copy-metadata"></a>

When you use the AWS CLI version 1 version of commands in the `aws s3` namespace to copy a file from one Amazon S3 bucket location to another Amazon S3 bucket location, and that operation uses [multipart copy](https://docs.aws.amazon.com/AmazonS3/latest/dev/CopyingObjctsMPUapi.html), no file properties from the source object are copied to the destination object\.

By default, the AWS CLI version 2 commands in the `s3` namespace that perform multipart copies now transfer all tags and the following set of properties from the source to the destination copy: `content-type`, `content-language`, `content-encoding`, `content-disposition`, `cache-control`, `expires`, and `metadata`\.

This can result in additional AWS API calls to the Amazon S3 endpoint that would not have been made if you used AWS CLI version 1\. These can include: `HeadObject`, `GetObjectTagging`, and `PutObjectTagging`\.

If you need to change this default behavior in AWS CLI version 2 commands, use the `--copy-props` parameter to specify one of the following options:
+ **default** – The default value\. Specifies that the copy includes all tags attached to the source object and the properties encompassed by the `--metadata-directive` parameter used for non\-multipart copies: `content-type`, `content-language`, `content-encoding`, `content-disposition`, `cache-control`, `expires`, and `metadata`\.
+ **metadata\-directive** – Specifies that the copy includes only the properties that are encompassed by the `--metadata-directive` parameter used for non\-multipart copies\. It doesn't copy any tags\.
+ **none** – Specifies that the copy includes none of the properties from the source object\.

## AWS CLI version 2 no longer automatically retrieves `http://` or `https://` URLs for parameters<a name="cliv2-migration-paramfile"></a>

The AWS CLI version 2 no longer performs a GET operation when a parameter value begins with `http://` or `https://`, and then using the returned content as the value of the parameter\. If you need to retrieve a URL and pass the contents read from that URL as the value of a parameter, we recommend that you use `curl` or a similar tool to download the contents of the URL to a local file\. Then use the `file://` syntax to read the contents of that file and use it as the parameter's value\. 

For example, the following command no longer tries to retrieve the contents of the page found at `http://www.google.com` and pass those contents as the parameter\. Instead, it passes the literal text string `https://google.com` as the parameter\.

```
$ aws ssm put-parameter --value http://www.google.com --name prod.microservice1.db.secret --type String 2
```

If you really do want to retrieve and use the contents of a web URL as a parameter, you can do the following in version 2\.

```
$ curl https://my.example.com/mypolicyfile.json -o mypolicyfile.json
$ aws iam put-role-policy --policy-document file://./mypolicyfile.json --role-name MyRole --policy-name MyReadOnlyPolicy
```

In the previous example, the `-o` parameter tells `curl` to save the file in the current folder with the same name as the source file\. The second command retrieves the content of that downloaded file and passes the content as the value of `--policy-document`\.

## AWS CLI version 2 uses a paging program for all output by default\.<a name="cliv2-migration-output-pager"></a>

By default, AWS CLI version 2 returns all output through your operating system’s default pager program\. By default this program is the [https://ss64.com/bash/less.html](https://ss64.com/bash/less.html) program on Linux and macOS, and the [https://docs.microsoft.com/windows-server/administration/windows-commands/more](https://docs.microsoft.com/windows-server/administration/windows-commands/more) program on Windows\. This can make it easier for you to navigate a large amount of output from a service by displaying that output one page at a time\. However, you sometimes want all the output without needing to press a key to get each page, such as when you are running scripts\. To do this, you can configure the AWS CLI version 2 to use a different paging program or none at all\. To do this, configure either the `AWS_PAGER` environment variable or the `cli_pager` setting in your `~/.aws/config` file and specify the command you want to use\. You can specify a command that is in your search path, or specify the full path and file name for any command available on your computer\.

You can completely disable all use of an external paging program by setting the variable to an empty string as shown in the following examples\.

**By setting an option in the `~/.aws/config` file**

The following example shows setting it for the `default` profile, but you can add the setting to any profile in your `~/.aws/config` file\.

```
[default]
cli_pager=
```

**By setting an environment variable**

*Linux or macOS:*

```
$ export AWS_PAGER=""
```

*Windows:*

```
C:\> setx AWS_PAGER ""
```

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

## AWS CLI version 2 replaces `ecr get-login` with `ecr get-login-password`<a name="cliv2-migration-ecr-get-login"></a>

The AWS CLI version 2 replaces the command `aws ecr get-login` with the new `aws ecr get-login-password` command that improves automated integration with container authentication\. 

The `aws ecr get-login-password` command reduces the risk of exposing your credentials in the process list, shell history, or other log files\. It also improves compatibility with the `docker login` command, allowing better automation\.

The `aws ecr get-login-password` command is available in the AWS CLI version 1\.17\.10 and later, and the AWS CLI version 2\. The older `aws ecr get-login` command is still available in the AWS CLI version 1 for backward compatibility\. 

The `aws ecr get-login-password` command enables you to replace the following code that retrieves a password\.

```
$(aws ecr get-login -no-include-email)
```

To reduce the risk of exposing the password to the shell history or logs, use the following example command instead\. In this example, the password is piped directly to the `docker login` command, where it is assigned to the password parameter by the `--password-stdin` option\.

```
aws ecr get-login-password | docker login --username AWS --password-stdin MY-REGISTRY-URL
```

## AWS CLI version 2 support for plugins is changing<a name="cliv2-migration-profile-plugins"></a>

Plugin support in the AWS CLI version 2 is completely provisional and intended to help users migrate from AWS CLI version 1 until a stable, updated, plugin interface is released\. There are no guarantees that a particular plugin or even the CLI plugin interface will be supported in future versions of the AWS CLI version 2\. If you rely on plugins, be sure to lock to a particular version of the CLI and test the functionality of your plugin when you do upgrade\.

To enable plugin support, create a `[plugins]` section in your `~/.aws/config`\.

```
[plugins]
cli_legacy_plugin_path = <path-to-plugins>/python3.7/site-packages
<plugin-name> = <plugin-module>
```

In the `[plugins]` section, begin by defining the `cli_legacy_plugin_path` variable and setting its value to the Python site packages path that your plugin module lives in\. Then you can configure a plugin by providing a name for the plugin \(`plugin-name`\), and the file name of the Python module, \(`plugin-module`\), that contains the source code for your plugin\. The CLI loads each plugin by importing its `plugin-module` and calling its `awscli_initialize` function\.

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