# Command Line Options<a name="cli-command-line"></a>

You can use the following command line options to override the default configuration settings for a single command\. You cannot use command line options to specify credentials\.

**\-\-profile**  
The name of a [named profile](cli-multiple-profiles.md) to use\.

**\-\-region**  
The AWS Region to call\.

**\-\-output**  
The output format\.

**\-\-endpoint\-url**  
The URL to make the call against\. For most commands, the AWS CLI automatically determines the URL based on the service and AWS Region\. However, some commands require that you specify an account\-specific URL\.

When you provide one of these options at the command line, it overrides the default configuration and corresponding profile setting for a single command\. Each option takes a string argument with a space or equals sign \(=\) separating the argument from the option name\. When the argument string contains a space, use quotation marks around the argument\.

**Tip**  
To set up additional profiles, you can use the **\-\-profile** option with **aws configure**\.  

```
$ aws configure --profile <profilename>
```

Common uses for command line options include checking your resources in multiple AWS Regions, and changing the output format for legibility or ease of use when scripting\. For example, if you're not sure which region your instance is running in, you can run the **describe\-instances** command against each region until you find it, as follows\. 

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

The argument types \(string, Boolean, etc\.\) for each command line option are described in detail in [Specifying Parameter Values](cli-using-param.md)\.