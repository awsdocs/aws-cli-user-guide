# Controlling Command Output from the AWS Command Line Interface<a name="controlling-output"></a>

This section describes the different ways that you can control the output from the AWS CLI\.

**Topics**
+ [How to Select the Output Format](#controlling-output-format)
+ [How to Filter the Output with the `--query` Option](#controlling-output-filter)
+ [JSON Output Format](#controlling-output-json)
+ [Text Output Format](#text-output)
+ [Table Output Format](#table-output)

## How to Select the Output Format<a name="controlling-output-format"></a>

The AWS CLI supports three different output formats:
+ JSON \(json\)
+ Tab\-delimited text \(text\)
+ ASCII\-formatted table \(table\)

As explained in the [configuration](cli-chap-configure.md) topic, the output format can be specified in three different ways:
+ Using the `output` option in the configuration file\. The following example sets the output to `text`:

  ```
  [default]
  output=text
  ```
+ Using the `AWS_DEFAULT_OUTPUT` environment variable\. For example:

  ```
  $ export AWS_DEFAULT_OUTPUT="table"
  ```
+ Using the `--output` option on the command line\. For example:

  ```
  $ aws swf list-domains --registration-status REGISTERED --output text
  ```

**Note**  
If the output format is specified in multiple ways, the usual [AWS CLI precedence rules](cli-chap-configure.md#config-settings-and-precedence) apply\. For example, using the `AWS_DEFAULT_OUTPUT` environment variable overrides any value set in the config file with `output`, and a value passed to an AWS CLI command with `--output` overrides any value set in the environment or in the config file\.

JSON is best for handling the output programmatically via various languages or `jq` \(a command\-line JSON processor\)\. The table format is easy for humans to read, and text format works well with traditional Unix text processing tools, such as `sed`, `grep`, and `awk`, as well as Windows PowerShell scripts\.

## How to Filter the Output with the `--query` Option<a name="controlling-output-filter"></a>

The AWS CLI provides built\-in output filtering capabilities with the `--query` option\. To demonstrate how it works, we'll first start with the default JSON output below, which describes two EBS \(Elastic Block Storage\) volumes attached to separate EC2 instances\.

```
$ aws ec2 describe-volumes
{
    "Volumes": [
        {
            "AvailabilityZone": "us-west-2a",
            "Attachments": [
                {
                    "AttachTime": "2013-09-17T00:55:03.000Z",
                    "InstanceId": "i-a071c394",
                    "VolumeId": "vol-e11a5288",
                    "State": "attached",
                    "DeleteOnTermination": true,
                    "Device": "/dev/sda1"
                }
            ],
            "VolumeType": "standard",
            "VolumeId": "vol-e11a5288",
            "State": "in-use",
            "SnapshotId": "snap-f23ec1c8",
            "CreateTime": "2013-09-17T00:55:03.000Z",
            "Size": 30
        },
        {
            "AvailabilityZone": "us-west-2a",
            "Attachments": [
                {
                    "AttachTime": "2013-09-18T20:26:16.000Z",
                    "InstanceId": "i-4b41a37c",
                    "VolumeId": "vol-2e410a47",
                    "State": "attached",
                    "DeleteOnTermination": true,
                    "Device": "/dev/sda1"
                }
            ],
            "VolumeType": "standard",
            "VolumeId": "vol-2e410a47",
            "State": "in-use",
            "SnapshotId": "snap-708e8348",
            "CreateTime": "2013-09-18T20:26:15.000Z",
            "Size": 8
        }
    ]
}
```

First, we can display only the first volume from the `Volumes` list with the following command\.

```
$ aws ec2 describe-volumes --query 'Volumes[0]'
{
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
        {
            "AttachTime": "2013-09-17T00:55:03.000Z",
            "InstanceId": "i-a071c394",
            "VolumeId": "vol-e11a5288",
            "State": "attached",
            "DeleteOnTermination": true,
            "Device": "/dev/sda1"
        }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-e11a5288",
    "State": "in-use",
    "SnapshotId": "snap-f23ec1c8",
    "CreateTime": "2013-09-17T00:55:03.000Z",
    "Size": 30
}
```

Now, we use the wildcard notation `[*]` to iterate over the entire list and also filter out three elements: `VolumeId`, `AvailabilityZone`, and `Size`\. Note that the dictionary notation requires that you provide an alias for each key, like this: \{Alias1:Key1,Alias2:Key2\}\. A dictionary is inherently *unordered*, so the ordering of the key\-aliases within a structure may not be consistent in some cases\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,AZ:AvailabilityZone,Size:Size}'
[
    {
        "AZ": "us-west-2a",
        "ID": "vol-e11a5288",
        "Size": 30
    },
    {
        "AZ": "us-west-2a",
        "ID": "vol-2e410a47",
        "Size": 8
    }
]
```

In the dictionary notation, you can also use chained keys such as `key1.key2[0].key3` to filter elements deeply nested within the structure\. The example below demonstrates this with the `Attachments[0].InstanceId` key, aliased to simply `InstanceId`\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,InstanceId:Attachments[0].InstanceId,AZ:AvailabilityZone,Size:Size}'
[
    {
        "InstanceId": "i-a071c394",
        "AZ": "us-west-2a",
        "ID": "vol-e11a5288",
        "Size": 30
    },
    {
        "InstanceId": "i-4b41a37c",
        "AZ": "us-west-2a",
        "ID": "vol-2e410a47",
        "Size": 8
    }
]
```

You can also filter multiple elements with the list notation: `[key1, key2]`\. This will format all filtered attributes into a single *ordered* list per object, regardless of type\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].[VolumeId, Attachments[0].InstanceId, AvailabilityZone, Size]'
[
    [
        "vol-e11a5288",
        "i-a071c394",
        "us-west-2a",
        30
    ],
    [
        "vol-2e410a47",
        "i-4b41a37c",
        "us-west-2a",
        8
    ]
]
```

To filter results by the value of a specific field, use the JMESPath "?" operator\. The following example query outputs only volumes in the us\-west\-2a availability zone:

```
$ aws ec2 describe-volumes --query 'Volumes[?AvailabilityZone==`us-west-2a`]'
```

**Note**  
When specifying a literal value such as "us\-west\-2" above in a JMESPath query expression, you must surround the value in backticks \(`\) in order for it to be read properly\.

Combined with the three output formats that will be explained in more detail in the following sections, the `--query` option is a powerful tool you can use to customize the content and style of outputs\. For more examples and the full spec of JMESPath, the underlying JSON\-processing library, visit [http://jmespath\.org/specification\.html](http://jmespath.org/specification.html)\.

## JSON Output Format<a name="controlling-output-json"></a>

JSON is the default output format of the AWS CLI\. Most languages can easily decode JSON strings using built\-in functions or with publicly available libraries\. As shown in the previous topic along with output examples, the `--query` option provides powerful ways to filter and format the AWS CLI's JSON formatted output\. If you need more advanced features that may not be possible with `--query`, you can check out `jq`, a command line JSON processor\. You can download it and find the official tutorial at: [http://stedolan\.github\.io/jq/](http://stedolan.github.io/jq/)\.

## Text Output Format<a name="text-output"></a>

The *text* format organizes the AWS CLI's output into tab\-delimited lines\. It works well with traditional Unix text tools such as `grep`, `sed`, and `awk`, as well as Windows PowerShell\. 

The text output format follows the basic structure shown below\. The columns are sorted alphabetically by the corresponding key names of the underlying JSON object\.

```
IDENTIFIER  sorted-column1 sorted-column2
IDENTIFIER2 sorted-column1 sorted-column2
```

The following is an example of a text output\.

```
$ aws ec2 describe-volumes --output text
VOLUMES us-west-2a      2013-09-17T00:55:03.000Z        30      snap-f23ec1c8   in-use  vol-e11a5288    standard
ATTACHMENTS     2013-09-17T00:55:03.000Z        True    /dev/sda1       i-a071c394      attached        vol-e11a5288
VOLUMES us-west-2a      2013-09-18T20:26:15.000Z        8       snap-708e8348   in-use  vol-2e410a47    standard
ATTACHMENTS     2013-09-18T20:26:16.000Z        True    /dev/sda1       i-4b41a37c      attached        vol-2e410a47
```

*We strongly recommend that the text output be used along with the `--query` option to ensure consistent behavior*\. This is because the text format alphabetically orders output columns, and similar resources may not always have the same collection of keys\. For example, a JSON representation of a Linux EC2 instance may have elements that are not present in the JSON representation of a Windows instance, or vice versa\. Also, resources may have key\-value elements added or removed in future updates, altering the column ordering\. This is where `--query` augments the functionality of the text output to enable complete control over the output format\. In the example below, the command pre\-selects which elements to display and defines the ordering of the columns with the list notation `[key1, key2, ...]`\. This gives users full confidence that the correct key values will always be displayed in the expected column\. Finally, notice how the AWS CLI outputs 'None' as values for keys that don't exist\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].[VolumeId, Attachments[0].InstanceId, AvailabilityZone, Size, FakeKey]' --output text
vol-e11a5288    i-a071c394      us-west-2a      30      None
vol-2e410a47    i-4b41a37c      us-west-2a      8       None
```

Below is an example of how `grep` and `awk` can be used along with a text output from `aws ec2 describe-instances` command\. The first command displays the Availability Zone, state, and instance ID of each instance in text output\. The second command outputs only the instance IDs of all running instances in the `us-west-2a` Availability Zone\.

```
$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text
us-west-2a      running i-4b41a37c
us-west-2a      stopped i-a071c394
us-west-2b      stopped i-97a217a0
us-west-2a      running i-3045b007
us-west-2a      running i-6fc67758

$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text | grep us-west-2a | grep running | awk '{print $3}'
i-4b41a37c
i-3045b007
i-6fc67758
```

The next command shows a similar example for all stopped instances and takes it one step further to automate changing instance types for each stopped instance\.

```
$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[State.Name, InstanceId]' --output text |
> grep stopped |
> awk '{print $2}' |
> while read line;
> do aws ec2 modify-instance-attribute --instance-id $line --instance-type '{"Value": "m1.medium"}';
> done
```

The text output is useful in Windows PowerShell as well\. Because AWS CLI's text output is tab\-delimited, it is easily split into an array in PowerShell using the ``t` delimiter\. The following command displays the value of the third column \(`InstanceId`\) if the first column \(`AvailabilityZone`\) matches `us-west-2a`\.

```
> aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text |
%{if ($_.split("`t")[0] -match "us-west-2a") { $_.split("`t")[2]; } }
i-4b41a37c
i-a071c394
i-3045b007
i-6fc67758
```

## Table Output Format<a name="table-output"></a>

The `table` format produces human\-readable representations of AWS CLI output\. Here is an example:

```
$ aws ec2 describe-volumes --output table
---------------------------------------------------------------------------------------------------------------------
|                                                  DescribeVolumes                                                  |
+-------------------------------------------------------------------------------------------------------------------+
||                                                     Volumes                                                     ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
|| AvailabilityZone |        CreateTime         | Size  |  SnapshotId    |  State  |   VolumeId     | VolumeType   ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
||  us-west-2a      |  2013-09-17T00:55:03.000Z |  30   |  snap-f23ec1c8 |  in-use |  vol-e11a5288  |  standard    ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
|||                                                  Attachments                                                  |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
|||        AttachTime         |  DeleteOnTermination   |   Device    | InstanceId   |   State    |   VolumeId     |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
|||  2013-09-17T00:55:03.000Z |  True                  |  /dev/sda1  |  i-a071c394  |  attached  |  vol-e11a5288  |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
||                                                     Volumes                                                     ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
|| AvailabilityZone |        CreateTime         | Size  |  SnapshotId    |  State  |   VolumeId     | VolumeType   ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
||  us-west-2a      |  2013-09-18T20:26:15.000Z |  8    |  snap-708e8348 |  in-use |  vol-2e410a47  |  standard    ||
|+------------------+---------------------------+-------+----------------+---------+----------------+--------------+|
|||                                                  Attachments                                                  |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
|||        AttachTime         |  DeleteOnTermination   |   Device    | InstanceId   |   State    |   VolumeId     |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
|||  2013-09-18T20:26:16.000Z |  True                  |  /dev/sda1  |  i-4b41a37c  |  attached  |  vol-2e410a47  |||
||+---------------------------+------------------------+-------------+--------------+------------+----------------+||
```

The `--query` option can be used with the table format to display a set of elements pre\-selected from the raw output\. Note the output differences in dictionary and list notations: column names are alphabetically ordered in the first example, and unnamed columns are ordered as defined by the user in the second example\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,InstanceId:Attachments[0].InstanceId,AZ:AvailabilityZone,Size:Size}' --output table
------------------------------------------------------
|                   DescribeVolumes                  | 
+------------+----------------+--------------+-------+
|     AZ     |      ID        | InstanceId   | Size  |
+------------+----------------+--------------+-------+
|  us-west-2a|  vol-e11a5288  |  i-a071c394  |  30   |
|  us-west-2a|  vol-2e410a47  |  i-4b41a37c  |  8    |
+------------+----------------+--------------+-------+

$ aws ec2 describe-volumes --query 'Volumes[*].[VolumeId,Attachments[0].InstanceId,AvailabilityZone,Size]' --output table
----------------------------------------------------
|                  DescribeVolumes                 |
+--------------+--------------+--------------+-----+
|  vol-e11a5288|  i-a071c394  |  us-west-2a  |  30 |
|  vol-2e410a47|  i-4b41a37c  |  us-west-2a  |  8  |
+--------------+--------------+--------------+-----+
```