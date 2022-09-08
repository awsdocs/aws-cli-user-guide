--------

--------

# Having the AWS CLI prompt you for commands<a name="cli-usage-parameters-prompting"></a>

You can have the AWS CLI version 2 prompt you commands, parameters, and resources when you run an `aws` command\.

**Topics**
+ [How it works](#cli-usage-auto-prompt-about)
+ [Auto\-prompt features](#cli-usage-auto-prompt-features)
+ [Auto\-prompt modes](#cli-usage-auto-prompt-modes)
+ [Configure auto\-prompt](#cli-usage-auto-prompt-configure)

## How it works<a name="cli-usage-auto-prompt-about"></a>

If enabled, the auto\-prompt enables you to use the **ENTER** key to complete a partially entered command\. After pressing the **ENTER** key, commands, parameters, and resources are suggested based on what you continue to type\. The suggestions list the name of the command, parameter, or resource on the left and a description of it on the right\. To select and use a suggestion, use the arrows keys to highlight a row, and then press the **SPACE** key\. When you've finished entering in your command, press **ENTER** to use the command\. The following example demonstrates what a suggested list from auto\-prompt looks like\.

```
$ aws
> aws a
       accessanalyzer                Access Analyzer
       acm                           AWS Certificate Manager
       acm-pca                       AWS Certificate Manager Private Certificate Authority
       alexaforbusiness              Alexa For Business
       amplify                       AWS Amplify
```

## Auto\-prompt features<a name="cli-usage-auto-prompt-features"></a>

The auto\-prompt contains the following useful features:

**Documentation panel**  
Provides the help documentation for the current command\. To open the documentation, press the **F3** key\.

**Command completion**  
Suggests `aws` commands to use\. To see a list, partially enter the command\. The following example is searching for a service starting with the letter `a`\.  

```
$ aws
> aws a
       accessanalyzer                Access Analyzer
       acm                           AWS Certificate Manager
       acm-pca                       AWS Certificate Manager Private Certificate Authority
       alexaforbusiness              Alexa For Business
       amplify                       AWS Amplify
```

**Parameter completion**  
After a command is typed, auto\-prompt starts to suggest parameters\. The descriptions for the parameters include the value type, and a description of what the parameter is\. Required parameters are listed first, and are labeled as required\. The following example shows the auto\-prompt list of parameters for `aws dynamodb describe-table`\.  

```
$ aws dynamodb describe-table
> aws dynamodb describe-table 
                              --table-name (required)  [string] The name of the table to describe.
                               --cli-input-json         [string] Reads arguments from the JSON string provided. The JSON string follows the format provide...
                               --cli-input-yaml         [string] Reads arguments from the YAML string provided. The YAML string follows the format provide...
                               --generate-cli-skeleton  [string] Prints a JSON skeleton to standard output without sending an API request. If provided wit...
```

**Resource completion**  
The auto\-prompt makes AWS API calls using available AWS resource properties to suggest resource values\. This allows for auto\-prompt to suggest possible resources you own when entering in parameters\. In the following example auto\-prompt lists your table names when filling in the `--table-name` parameter for the `aws dynamodb describe-table` command\.  

```
$ aws dynamodb describe-table
> aws dynamodb describe-table --table-name 
                                            Table1
                                            Table2
                                            Table3
```

**Shorthand completion**  
For parameters that use shorthand syntax, auto\-prompt suggests values to use\. In the following example, auto\-prompt lists shorthand syntax values for the `--placement` parameter in the `aws ec2 run-instances` command\.  

```
$ aws ec2 run-instances
> aws ec2 run-instances --placement 
 AvailabilityZone=      [string] The Availability Zone of the instance. If not specified, an Availability Zone wil...
 Affinity=              [string] The affinity setting for the instance on the Dedicated Host. This parameter is no...
 GroupName=             [string] The name of the placement group the instance is in.
 PartitionNumber=       [integer] The number of the partition the instance is in. Valid only if the placement grou...
```

**File completion**  
When filling out parameters in `aws` commands, auto\-complete suggests local filenames after using the prefix `file://` or `fileb://`\. In the following example, auto\-prompt suggests local files after entering in `--item file://` for the `aws ec2 run-instances` command\.  

```
$ aws ec2 run-instances
> aws ec2 run-instances --item file:// 
                         item1.txt
                         file1.json
                         file2.json
```

**Region completion**  
When using the global parameter `--region`, auto\-prompt lists possible Regions to select from\. In the following example, auto\-prompt suggests Regions in alphabetical order after entering in `--region` for the `aws dynamodb list-tables` command\.  

```
$ aws dynamodb list-tables
> aws dynamodb list-tables --region 
                                     af-south-1
                                     ap-east-1
                                     ap-northeast-1
                                     ap-northeast-2
```

**Profile completion**  
When using the global parameter `--profile`, auto\-prompt lists your profiles\. In the following example, auto\-prompt suggests your profiles after entering in `--profile` for the `aws dynamodb list-tables` command\.  

```
$ aws dynamodb list-tables
> aws dynamodb list-tables --profile 
                                     profile1
                                     profile2
                                     profile3
```

**Fuzzy searching**  
Complete commands and values that contain a specific set of characters\. In the following example, auto\-prompt suggests Regions that contain `eu` after entering in `--region eu` for the `aws dynamodb list-tables` command\.  

```
$ aws dynamodb list-tables
> aws dynamodb list-tables --region west
                                         eu-west-1
                                         eu-west-2
                                         eu-west-3
                                         us-west-1
```

**History**  
To view and run previously used commands in auto\-prompt mode, press **CTRL \+ R**\. History lists previous commands that you can select by using the arrow keys\. In the following example, the auto\-prompt mode history is displayed\.  

```
$ aws
> aws 
        dynamodb list-tables
        s3 ls
```

## Auto\-prompt modes<a name="cli-usage-auto-prompt-modes"></a>

Auto\-prompt for the AWS CLI version 2 has 2 modes that can be configured:
+ **Full mode:** Uses auto\-prompt each time you attempt to run an `aws` command, whether you manually call it using the `--cli-auto-prompt` parameter or permanently enabled it\. This includes pressing **ENTER** after both a complete command or incomplete command\.
+ **Partial mode:** Uses auto\-prompt if a command is incomplete or cannot be run due to client\-side validation errors\. This mode is particular useful if you have pre\-existing scripts, runbooks, or you only want to be auto\-prompted for commands you are unfamiliar with rather than prompted on every command\.

## Configure auto\-prompt<a name="cli-usage-auto-prompt-configure"></a>

To configure auto\-prompt you can use the following methods in order of precedence: 
+ **Command line options** enable or disable auto\-prompt for a single command\. Use `\-\-cli\-auto\-prompt` to call auto\-prompt and `\-\-no\-cli\-auto\-prompt` to disable auto\-prompt\.
+ **Environment variables** use the `aws\_cli\_auto\_prompt` variable\.
+ **Shared config files** use the `cli\_auto\_prompt` setting\.