# Command line options<a name="cli-configure-options"></a>

You can use the following command line options to override the default configuration settings, any corresponding profile setting, or environment variable setting for that single command\. You can't use command line options to directly specify credentials, although you can specify which profile to use\. Each option that takes an argument requires a space or equals sign \(=\) separating the argument from the option name\. If the argument value is a string that contains a space, you must use quotation marks around the argument\.

The argument types \(for example, string, Boolean\) for each command line option are described in detail in [Specifying parameter values for the AWS CLICommon AWS CLI parameter types](cli-usage-parameters.md)\.

**\-\-ca\-bundle *<string>***  
Specifies the certificate authority \(CA\) certificate bundle to use when verifying SSL certificates\.

**\-\-cli\-auto\-prompt**  
**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing, updating, and uninstalling the AWS CLI version 2](install-cliv2.md)\.
Enables auto\-prompt mode for a single command\. As the following examples show, you can specify it at any point\.  

```
$ aws --cli-auto-prompt
$ aws dynamodb --cli-auto-prompt
$ aws dynamodb describe-table --cli-auto-prompt
```
This option overrides the `aws\_cli\_auto\_prompt` environment variable and the `cli\_auto\_prompt` profile setting\.  
For information on the AWS CLI version 2 auto\-prompt feature, see [Having the AWS CLI prompt you for commands](cli-usage-parameters-prompting.md)\.

**\-\-cli\-connect\-timeout *<integer>***  
Specifies the maximum socket connect time in seconds\. If the value is set to zero \(0\), the socket connect waits indefinitely \(is blocking\) and doesn't timeout\.

**\-\-cli\-read\-timeout *<integer>***  
Specifies the maximum socket read time in seconds\. If the value is set to zero \(0\) the socket read waits indefinitely \(is blocking\) and doesn't timeout\.

**\-\-color *<string>***  
Specifies support for color output\. Valid values are `on`, `off`, and `auto`\. The default value is `auto`\.

**\-\-debug**  
A Boolean switch that enables debug logging\. The AWS CLI by default provides cleaned up information regarding any successes or failures regarding command outcomes in the command output\. The `--debug` option provides the full Python logs\. This includes additional `stderr` diagnostic information about the operation of the command that can be useful when troubleshooting why a command provides unexpected results\. To easily view debug logs, we suggest sending the logs to a file to more easily search the information\. You can do this by using one of the following\.  
To send **only** the `stderr` diagnostic information, append `2> debug.txt` where `debug.txt` is the name you want to use for your debug file:  

```
$ aws servicename commandname options --debug 2> debug.txt
```
To send **both** the output and `stderr` diagnostic information, append `&> debug.txt` where `debug.txt` is the name you want to use for your debug file:  

```
$ aws servicename commandname options --debug &> debug.txt
```

**\-\-endpoint\-url *<string>***  
Specifies the URL to send the request to\. For most commands, the AWS CLI automatically determines the URL based on the selected service and the specified AWS Region\. However, some commands require that you specify an account\-specific URL\. You can also configure some AWS services to [host an endpoint directly within your private VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-privatelink), which might then need to be specified\.   
For a list of the standard service endpoints available in each Region, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

**\-\-no\-cli\-auto\-prompt**  
**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing, updating, and uninstalling the AWS CLI version 2](install-cliv2.md)\.
Disables auto\-prompt mode for a single command\.  

```
$ aws dynamodb describe-table --table-name Table1 --no-cli-auto-prompt
```
This option overrides the `aws\_cli\_auto\_prompt` environment variable and the `cli\_auto\_prompt` profile setting\.  
For information on the AWS CLI version 2 auto\-prompt feature, see [Having the AWS CLI prompt you for commands](cli-usage-parameters-prompting.md)\.

**\-\-no\-cli\-pager**  
A Boolean switch that disables using a pager for the output of the command\.

**\-\-no\-paginate**  
A Boolean switch that disables the multiple calls the automatically AWS CLI makes to receive all command results that creates pagination of the output\. This means only the first page of your output is displayed\.

**\-\-no\-sign\-request**  
A Boolean switch that disables signing the HTTP requests to the AWS service endpoint\. This prevents credentials from being loaded\.

**\-\-output *<string>***  
Specifies the output format to use for this command\. You can specify any of the following values:  
+ [**`json`**](cli-usage-output-format.md#json-output) – The output is formatted as a [JSON](https://json.org/) string\.
+ [**`yaml`**](cli-usage-output-format.md#yaml-output) – The output is formatted as a [YAML](https://yaml.org/) string\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`yaml-stream`**](cli-usage-output-format.md#yaml-stream-output) – The output is streamed and formatted as a [YAML](https://yaml.org/) string\. Streaming allows for faster handling of large data types\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`text`**](cli-usage-output-format.md#text-output) – The output is formatted as multiple lines of tab\-separated string values\. This can be useful to pass the output to a text processor, like `grep`, `sed`, or `awk`\.
+ [**`table`**](cli-usage-output-format.md#table-output) – The output is formatted as a table using the characters \+\|\- to form the cell borders\. It typically presents the information in a "human\-friendly" format that is much easier to read than the others, but not as programmatically useful\.

**\-\-profile *<string>***  
Specifies the [named profile](cli-configure-profiles.md) to use for this command\. To set up additional named profiles, you can use the `aws configure` command with the `--profile` option\.  

```
$ aws configure --profile <profilename>
```

**\-\-query *<string>***  
Specifies a [JMESPath query](http://jmespath.org/) to use in filtering the response data\. For more information, see [Filtering AWS CLI output](cli-usage-filter.md)\.

**\-\-region *<string>***  
Specifies which AWS Region to send this command's AWS request to\. For a list of all of the Regions that you can specify, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

**\-\-version**  
A Boolean switch that displays the current version of the AWS CLI program that is running\.

Common uses for command line options include checking your resources in multiple AWS Regions, and changing the output format for legibility or ease of use when scripting\. For example, if you're not sure which Region your instance is running in, you can run the **describe\-instances** command against each Region until you find it, as follows\. 

```
$ aws ec2 describe-instances --output table --region us-east-1
-------------------
|DescribeInstances|
+-----------------+
$ aws ec2 describe-instances --output table --region us-west-1
-------------------
|DescribeInstances|
+-----------------+
$ aws ec2 describe-instances --output table --region us-west-2
------------------------------------------------------------------------------
|                              DescribeInstances                             |
+----------------------------------------------------------------------------+
||                               Reservations                               ||
|+-------------------------------------+------------------------------------+|
||  OwnerId                            |  012345678901                      ||
||  ReservationId                      |  r-abcdefgh                        ||
|+-------------------------------------+------------------------------------+|
|||                                Instances                               |||
||+------------------------+-----------------------------------------------+||
|||  AmiLaunchIndex        |  0                                            |||
|||  Architecture          |  x86_64                                       |||
...
```

The argument types \(for example, string, Boolean\) for each command line option are described in detail in [Specifying parameter values for the AWS CLICommon AWS CLI parameter types](cli-usage-parameters.md)\.