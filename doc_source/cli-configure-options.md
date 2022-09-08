--------

--------

# Command line options<a name="cli-configure-options"></a>

In the AWS CLI, command line options are global parameters you can use to override the default configuration settings, any corresponding profile setting, or environment variable setting for that single command\. You can't use command line options to directly specify credentials, although you can specify which profile to use\. 

**Topics**
+ [How to use command line options](#cli-configure-options-how)
+ [AWS CLI supported global command line options](#cli-configure-options-list)
+ [Common uses of command line options](#cli-configure-options-common)

## How to use command line options<a name="cli-configure-options-how"></a>

Most command line options are simple strings, such as the profile name `profile1` in the following example:

```
$ aws s3 ls --profile profile1
example-bucket-1
example-bucket-2
...
```

Each option that takes an argument requires a space or equals sign \(=\) separating the argument from the option name\. If the argument value is a string that contains a space, you must use quotation marks around the argument\. For details on argument types and formatting for parameters, see [Specifying parameter values for the AWS CLICommon AWS CLI parameter types](cli-usage-parameters.md)\.

## AWS CLI supported global command line options<a name="cli-configure-options-list"></a>

In the AWS CLI you can use the following command line options to override the default configuration settings, any corresponding profile setting, or environment variable setting for that single command\. 

**\-\-ca\-bundle *<string>***  
Specifies the certificate authority \(CA\) certificate bundle to use when verifying SSL certificates\.   
If defined, this option overrides the value for the profile setting `ca\_bundle` and the `AWS\_CA\_BUNDLE` environment variable\.

**\-\-cli\-auto\-prompt**  
Enables auto\-prompt mode for a single command\. As the following examples show, you can specify it at any point\.  

```
$ aws --cli-auto-prompt
$ aws dynamodb --cli-auto-prompt
$ aws dynamodb describe-table --cli-auto-prompt
```
This option overrides the `aws\_cli\_auto\_prompt` environment variable and the `cli\_auto\_prompt` profile setting\.  
For information on the AWS CLI version 2 auto\-prompt feature, see [Having the AWS CLI prompt you for commands](cli-usage-parameters-prompting.md)\.

**\-\-cli\-binary\-format**  
Specifies how the AWS CLI version 2 interprets binary input parameters\. It can be one of the following values:  
+ **base64** – This is the default value\. An input parameter that is typed as a binary large object \(BLOB\) accepts a base64\-encoded string\. To pass true binary content, put the content in a file and provide the file's path and name with the `fileb://` prefix as the parameter's value\. To pass base64\-encoded text contained in a file, provide the file's path and name with the `file://` prefix as the parameter's value\.
+ **raw\-in\-base64\-out** – Provides backward compatibility with the AWS CLI version 1 behavior where binary values must be passed literally\. 
This overrides the `cli\_binary\_format` file configuration setting\.  

```
$ aws lambda invoke \
    --cli-binary-format raw-in-base64-out \
    --function-name my-function \
    --invocation-type Event \
    --payload '{ "name": "Bob" }' \
    response.json
```
If you reference a binary value in a file using the `fileb://` prefix notation, the AWS CLI *always* expects the file to contain raw binary content and does not attempt to convert the value\.   
If you reference a binary value in a file using the `file://` prefix notation, the AWS CLI handles the file according to the current `cli_binary_format` setting\. If that setting's value is `base64` \(the default when not explicitly set\), the AWS CLI expects the file to contain base64\-encoded text\. If that setting's value is `raw-in-base64-out`, the AWS CLI expects the file to contain raw binary content\.

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
+ **[`json`](cli-usage-output-format.md#json-output)** – The output is formatted as a [JSON](https://json.org/) string\.
+  **[`yaml`](cli-usage-output-format.md#yaml-output)** – The output is formatted as a [YAML](https://yaml.org/) string\.
+ **[`yaml-stream`](cli-usage-output-format.md#yaml-stream-output)** – The output is streamed and formatted as a [YAML](https://yaml.org/) string\. Streaming allows for faster handling of large data types\.
+ **[`text`](cli-usage-output-format.md#text-output)** – The output is formatted as multiple lines of tab\-separated string values\. This can be useful to pass the output to a text processor, like `grep`, `sed`, or `awk`\.
+ **[`table`](cli-usage-output-format.md#table-output)** – The output is formatted as a table using the characters \+\|\- to form the cell borders\. It typically presents the information in a "human\-friendly" format that is much easier to read than the others, but not as programmatically useful\.

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

## Common uses of command line options<a name="cli-configure-options-common"></a>

Common uses for command line options include checking your resources in multiple AWS Regions, and changing the output format for legibility or ease of use when scripting\. In the following examples, we run the **describe\-instances** command against each Region until we find which Region our instance is in\. 

```
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