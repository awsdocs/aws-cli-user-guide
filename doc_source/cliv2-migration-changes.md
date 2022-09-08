--------

--------

# New features and changes in AWS CLI version 2<a name="cliv2-migration-changes"></a>

This topic describes new features and changes in behavior between AWS CLI version 1 and AWS CLI version 2\. These changes might require you to update your scripts or commands to get the same behavior in version 2 as you did in version 1\.

**Topics**
+ [AWS CLI version 2 new features](#cliv2-migration-changes-features)
+ [Breaking changes between AWS CLI version 1 and AWS CLI version 2](#cliv2-migration-changes-breaking)

## AWS CLI version 2 new features<a name="cliv2-migration-changes-features"></a>

The AWS CLI version 2 is the most recent major version of the AWS CLI and supports all of the latest features\. Some features introduced in version 2 are not backported to version 1 and you must upgrade to access those features\. These features include the following:

**Python interpreter not needed**  
The AWS CLI version 2 doesn't need a separate install of Python\. It includes an embedded version\.

**[Wizards](cli-usage-wizard.md)**  
You can use a wizard with the AWS CLI version 2\. The wizard guides you through constructing certain commands\.

**[AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](cli-configure-sso.md)**  
If your organization uses AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\), your users can sign in to Active Directory, a built\-in IAM Identity Center directory, or [another IdP connected to IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-idp.html)\. Then, they are mapped to an AWS Identity and Access Management \(IAM\) role that allows you to run AWS CLI commands\.

**[Auto\-prompt](cli-usage-parameters-prompting.md)**  
When enabled, the AWS CLI version 2 can prompt you for commands, parameters, and resources when you run an `aws` command\. 

**[Docker](install-cliv2-docker.md)**  
The official Docker image for the AWS CLI provides isolation, portability, and security that AWS directly supports and maintains\. This way, you can use the AWS CLI version 2 in a container\-based environment without having to manage the installation yourself\. 

**[Client\-side pager](cli-usage-pagination.md#cli-usage-pagination-clientside)**  
The AWS CLI version 2 provides the use of a client\-side pager program for output\. By default, this feature is turned on and returns all output through your operating system’s default pager program\.

**[`aws configure import`](cli-configure-files.md#cli-config-aws_configure_import)**  
Import `.csv` credentials generated from the AWS Management Console\. A `.csv` file is imported with the profile name matching the IAM user name\. 

**[https://awscli.amazonaws.com/v2/documentation/api/latest/reference/configure/list-profiles.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/configure/list-profiles.html)**  
Lists the names of all profiles you have configured\. 

**[YAML stream output format](cli-usage-output-format.md#yaml-stream-output)**  
The `yaml` and `yaml-stream` format takes advantage of the [YAML](https://yaml.org) format while providing more responsive viewing of large datasets by streaming the data to you\. You can start viewing and using YAML data before the entire query downloads\. 

**[New high\-level `ddb` commands for DynamoDB](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ddb/index.html)**  
The AWS CLI version 2 has the high\-level Amazon DynamoDB commands [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ddb/put.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ddb/put.html) and [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ddb/select.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ddb/select.html)\. These commands provide a simplified interface for putting items in DynamoDB tables and searching in a DynamoDB table or index\. 

**[https://awscli.amazonaws.com/v2/documentation/api/latest/reference/logs/tail.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/logs/tail.html)**  
The AWS CLI version 2 has a custom `aws logs tail` command that tails the logs for an Amazon CloudWatch Logs group\. By default, the command returns logs from all associated CloudWatch Logs streams during the past ten minutes\.

**[Added metadata support for high\-level `s3` commands](cli-services-s3-commands.md#using-s3-commands-before-large)**  
The AWS CLI version 2 adds the `--copy-props` parameter to the high\-level `s3` commands\. With this parameter, you can configure additional metadata and tags for Amazon Simple Storage Service \(Amazon S3\)\.

**[`AWS_REGION`](cli-configure-envvars.md#envvars-list-AWS_REGION)**  
The AWS CLI version 2 has an AWS SDK\-compatible environment variable called `AWS_REGION`\. This variable specifies the AWS Region to send requests to\. It overrides the `AWS_DEFAULT_REGION` environment variable, which is only applicable in the AWS CLI\.

## Breaking changes between AWS CLI version 1 and AWS CLI version 2<a name="cliv2-migration-changes-breaking"></a>

This sections describes all of the changes in behavior between AWS CLI version 1 and AWS CLI version 2\. These changes might require you to update your scripts or commands to get the same behavior in version 2 as you did in version 1\.

**Topics**
+ [Environment variable added to set text file encoding](#cliv2-migration-encodingenvvar)
+ [Binary parameters are passed as base64\-encoded strings by default](#cliv2-migration-binaryparam)
+ [Improved Amazon S3 handling of file properties and tags for multipart copies](#cliv2-migration-s3-copy-metadata)
+ [No automatic retrieval of `http://` or `https://` URLs for parameters](#cliv2-migration-paramfile)
+ [Pager used for all output by default](#cliv2-migration-output-pager)
+ [Timestamp output values are standardized to ISO 8601 format](#cliv2-migration-timestamp)
+ [Improved handling of CloudFormation deployments that result in no changes](#cliv2-migration-cfn)
+ [Changed default behavior for Regional Amazon S3 endpoint for `us-east-1` Region](#cliv2-migration-s3-regional-endpoint)
+ [Changed default behavior for Regional AWS STS endpoints](#cliv2-migration-sts-regional-endpoint)
+ [`ecr get-login` removed and replaced with `ecr get-login-password`](#cliv2-migration-ecr-get-login)
+ [AWS CLI version 2 support for plugins is changing](#cliv2-migration-profile-plugins)
+ [Hidden alias support removed](#cliv2-migration-aliases)
+ [The `api_versions` configuration file setting is not supported](#cliv2-migration-api-versions)
+ [AWS CLI version 2 uses only Signature v4 to authenticate Amazon S3 requests](#cliv2-migration-sigv4)
+ [AWS CLI version 2 is more consistent with paging parameters](#cliv2-migration-skeleton-paging)
+ [AWS CLI version 2 provides more consistent return codes across all commands](#cliv2-migration-return-codes)

### Environment variable added to set text file encoding<a name="cliv2-migration-encodingenvvar"></a>

 By default, text files for [Binary / blob \(binary large object\) and streaming blob ](cli-usage-parameters-types.md#parameter-type-blob) use the same encoding as the installed locale\. Because the AWS CLI version 2 uses an embedded version of Python, the `PYTHONUTF8` and `PYTHONIOENCODING` environment variables are not supported\. To set encoding for text files to be different from the locale, use the `AWS_CLI_FILE_ENCODING` environment variable\. The following example sets the AWS CLI to open text files using `UTF-8` on Windows\.

```
AWS_CLI_FILE_ENCODING=UTF-8
```

For more information, see [Environment variables to configure the AWS CLI](cli-configure-envvars.md) \.

### Binary parameters are passed as base64\-encoded strings by default<a name="cliv2-migration-binaryparam"></a>

In the AWS CLI, some commands required [base64](https://wikipedia.org/wiki/Base64)\-encoded strings, while others required UTF\-8\-encoded byte strings\. In the AWS CLI version 1, passing data between two encoded string types often required some intermediate processing\. The AWS CLI version 2 makes handling binary parameters more consistent, which helps pass values from one command to another more reliably\. 

By default, the AWS CLI version 2 passes all binary input and binary output parameters as the base64\-encoded string `blobs` \(binary large object\)\. For more information, see [Binary / blob \(binary large object\) and streaming blob ](cli-usage-parameters-types.md#parameter-type-blob)\.

To revert to the AWS CLI version 1 behavior, use the `cli\_binary\_format` file configuration or the `\-\-cli\-binary\-format` parameter\.

### Improved Amazon S3 handling of file properties and tags for multipart copies<a name="cliv2-migration-s3-copy-metadata"></a>

When you use the AWS CLI version 1 commands in the `aws s3` namespace to copy a file from one S3 bucket location to another, and that operation uses [multipart copy](https://docs.aws.amazon.com/AmazonS3/latest/dev/CopyingObjctsMPUapi.html), no file properties from the source object are copied to the destination object\.

By default, the corresponding commands in the AWS CLI version 2 transfer all tags and some of the properties from the source to the destination copy\. Compared to the AWS CLI version 1, this can result in more AWS API calls being made to the Amazon S3 endpoint\. To change the default behavior for `s3` commands in AWS CLI version 2 , use the `--copy-props` parameter\.

For more information, see [File properties and tags in multipart copies](cli-services-s3-commands.md#using-s3-commands-before-tags)\.

### No automatic retrieval of `http://` or `https://` URLs for parameters<a name="cliv2-migration-paramfile"></a>

The AWS CLI version 2 does not perform a `GET` operation when a parameter value begins with `http://` or `https://`, and does not use the returned content as the parameter value\. As a result, the associated command line option `cli_follow_urlparam` is removed from the AWS CLI version 2\.

If you need to retrieve a URL and pass the URL contents into a parameter value, we recommend that you use `curl` or a similar tool to download the contents of the URL to a local file\. Then, use the `file://` syntax to read the contents of that file and use it as the parameter value\. 

For example, the following command no longer tries to retrieve the contents of the page found at `http://www.example.com` and pass those contents as the parameter\. Instead, it passes the literal text string `https://example.com` as the parameter\.

```
$ aws ssm put-parameter \
    --value http://www.example.com \
    --name prod.microservice1.db.secret \
    --type String 2
```

If you need to retrieve and use the contents of a web URL as a parameter, you can do the following in version 2\.

```
$ curl https://my.example.com/mypolicyfile.json -o mypolicyfile.json
$ aws iam put-role-policy \
    --policy-document file://./mypolicyfile.json \
    --role-name MyRole \
    --policy-name MyReadOnlyPolicy
```

In the preceding example, the `-o` parameter tells `curl` to save the file in the current folder with the same name as the source file\. The second command retrieves the content of that downloaded file and passes the content as the value of `--policy-document`\.

### Pager used for all output by default<a name="cliv2-migration-output-pager"></a>

By default, the AWS CLI version 2 returns all output through your operating system’s default pager program\. This program is the [https://ss64.com/bash/less.html](https://ss64.com/bash/less.html) program on Linux or macOS, and the [https://docs.microsoft.com/windows-server/administration/windows-commands/more](https://docs.microsoft.com/windows-server/administration/windows-commands/more) program on Windows\. This can help you navigate a large amount of output from a service by displaying that output one page at a time\. 

You can configure the AWS CLI version 2 to use a different paging program or none at all\. For more information, see [Client\-side pager](cli-usage-pagination.md#cli-usage-pagination-clientside)\.

### Timestamp output values are standardized to ISO 8601 format<a name="cliv2-migration-timestamp"></a>

By default, the AWS CLI version 2 returns all timestamp response values in the [ISO 8601 format](https://wikipedia.org/wiki/ISO_8601)\. In AWS CLI version 1, commands returned timestamp values in whatever format was returned by the HTTP API response, which could vary from service to service\. 

To see timestamps in the format returned by the HTTP API response, use the `wire` value in your `config` file\. For more information, see `cli\_timestamp\_format`\.

### Improved handling of CloudFormation deployments that result in no changes<a name="cliv2-migration-cfn"></a>

By default in the AWS CLI version 1, if you deploy a AWS CloudFormation template that results in no changes, the AWS CLI returns a failed error code\. This causes problems if you don't consider that to be an error and you want your script to continue\. You can work around this in the AWS CLI version 1 by adding the flag `-–no-fail-on-empty-changeset`, which returns `0`\.

Since this is a common use case, the AWS CLI version 2 defaults to returning a successful exit code of `0` when there is no change caused by a deployment and the operation returns an empty changeset\.

To revert to the original behavior, add the flag `--fail-on-empty-changeset`\.

### Changed default behavior for Regional Amazon S3 endpoint for `us-east-1` Region<a name="cliv2-migration-s3-regional-endpoint"></a>

When you configure theAWS CLI version 1 to use the `us-east-1` Region, the AWS CLI uses the global `s3.amazonaws.com` endpoint that is physically hosted in the `us-east-1` Region\. The AWS CLI version 2 uses the true Regional endpoint `s3.us-east-1.amazonaws.com` when that Region is specified\. To force the AWS CLI version 2 to use the global endpoint, you can set the Region for a command to `aws-global`\.

### Changed default behavior for Regional AWS STS endpoints<a name="cliv2-migration-sts-regional-endpoint"></a>

By default, the AWS CLI version 2 sends all AWS Security Token Service \(AWS STS\) API requests to the Regional endpoint for the currently configured AWS Region\. 

By default, the AWS CLI version 1 sends AWS STS requests to the global AWS STS endpoint\. You can control this default behavior in version 1 by using the [https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html#cli-config-sts_regional_endpoints](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-files.html#cli-config-sts_regional_endpoints) setting\.

### `ecr get-login` removed and replaced with `ecr get-login-password`<a name="cliv2-migration-ecr-get-login"></a>

The AWS CLI version 2 replaces the command `aws ecr get-login` with the `aws ecr get-login-password` command that improves automated integration with container authentication\. 

The `aws ecr get-login-password` command reduces the risk of exposing your credentials in the process list, shell history, or other log files\. It also improves compatibility with the `docker login` command for better automation\.

The `aws ecr get-login-password` command is available in the AWS CLI version 1\.17\.10 and later, and the AWS CLI version 2\. The earlier `aws ecr get-login` command is still available in the AWS CLI version 1 for backward compatibility\. 

With the `aws ecr get-login-password` command, you can replace the following code that retrieves a password\.

```
$ (aws ecr get-login --no-include-email)
```

To reduce the risk of exposing the password to the shell history or logs, use the following example command instead\. In this example, the password is piped directly to the `docker login` command, where it is assigned to the password parameter by the `--password-stdin` option\.

```
$ aws ecr get-login-password | docker login --username AWS --password-stdin MY-REGISTRY-URL
```

For more information, see [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecr/get-login-password.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ecr/get-login-password.html) in the *AWS CLI version 2 Reference Guide*\.

### AWS CLI version 2 support for plugins is changing<a name="cliv2-migration-profile-plugins"></a>

Plugin support in the AWS CLI version 2 is completely provisional and is intended to help users migrate from AWS CLI version 1 until a stable, updated plugin interface is released\. There are no guarantees that a particular plugin or even the AWS CLI plugin interface will be supported in future versions of the AWS CLI version 2\. If you rely on plugins, be sure to lock into a particular version of the AWS CLI and test the functionality of your plugin when you do upgrade\.

To enable plugin support, create a `[plugins]` section in your `~/.aws/config`\.

```
[plugins]
cli_legacy_plugin_path = <path-to-plugins>/python3.7/site-packages
<plugin-name> = <plugin-module>
```

In the `[plugins]` section, define the `cli_legacy_plugin_path` variable and set its value to the Python site packages path where your plugin module is\. Then, you can configure a plugin by providing a name for the plugin \(`plugin-name`\) and the file name of the Python module \(`plugin-module`\) that contains the source code for your plugin\. The AWS CLI loads each plugin by importing its `plugin-module` and calling its `awscli_initialize` function\.

### Hidden alias support removed<a name="cliv2-migration-aliases"></a>

AWS CLI version 2 no longer supports the following hidden aliases that were supported in version 1\. 

In the following table, the first column displays the service, command, and parameter that work in all versions, including the AWS CLI version 2\. The second column displays the alias that no longer works in the AWS CLI version 2\.


| Working service, command, and parameter | Obsolete alias | 
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

### The `api_versions` configuration file setting is not supported<a name="cliv2-migration-api-versions"></a>

The AWS CLI version 2 doesn't support calling earlier versions of AWS service APIs by using the `api_versions` configuration file setting\. All AWS CLI commands now call the latest version of the service APIs that are currently supported by the endpoint\.

### AWS CLI version 2 uses only Signature v4 to authenticate Amazon S3 requests<a name="cliv2-migration-sigv4"></a>

The AWS CLI version 2 doesn't support earlier signature algorithms to cryptographically authenticate service requests sent to Amazon S3 endpoints\. This signing happens automatically with every Amazon S3 request and only the [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) is supported\. You can't configure the signature version\. All Amazon S3 bucket presigned URLs now use only SigV4 and have a maximum expiration duration of one week\.

### AWS CLI version 2 is more consistent with paging parameters<a name="cliv2-migration-skeleton-paging"></a>

In the AWS CLI version 1, if you specify pagination parameters on the command line, then automatic pagination is turned off as expected\. However, when you specify pagination parameters by using a file with the `‐‐cli-input-json` parameter, automatic pagination was not turned off, which could result in unexpected output\. The AWS CLI version 2 turns off automatic pagination regardless of how you provide the parameters\.

### AWS CLI version 2 provides more consistent return codes across all commands<a name="cliv2-migration-return-codes"></a>

The AWS CLI version 2 is more consistent across all commands and properly returns an appropriate exit code compared to the AWS CLI version 1\. We also added exit codes 252, 253, and 254\. For more information on exit codes, see [Understanding return codes from the AWS CLI](cli-usage-returncodes.md)\.

If you have a dependency on how the AWS CLI version 1 uses return code values, we recommend checking the exit codes to make sure that you're getting the values you expect\. 