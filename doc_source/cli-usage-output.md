# Controlling command output from the AWS CLI<a name="cli-usage-output"></a>

This topic describes the different ways to control the output from the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [How to select the output format](#cli-usage-output-format)
+ [JSON output format](#json-output)
+ [YAML output format](#yaml-output)
+ [YAML stream output format](#yaml-stream-output)
+ [Text output format](#text-output)
+ [Table output format](#table-output)
+ [How to filter the output with the `--query` option](#cli-usage-output-filter)
+ [How to set the output’s default pager program](#cli-usage-output-pager)

## How to select the output format<a name="cli-usage-output-format"></a>

The AWS CLI supports four output formats:
+ [**`json`**](#json-output) – The output is formatted as a [JSON](https://json.org/) string\.
+ [**`yaml`**](#yaml-output) – The output is formatted as a [YAML](https://yaml.org/) string\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`yaml-stream`**](#yaml-stream-output) – The output is streamed and formatted as a [YAML](https://yaml.org/) string\. Streaming allows for faster handling of large data types\. *\(Available in the AWS CLI version 2 only\.\)*
+ [**`text`**](#text-output) – The output is formatted as multiple lines of tab\-separated string values\. This can be useful to pass the output to a text processor, like `grep`, `sed`, or `awk`\.
+ [**`table`**](#table-output) – The output is formatted as a table using the characters \+\|\- to form the cell borders\. It typically presents the information in a "human\-friendly" format that is much easier to read than the others, but not as programmatically useful\.

As explained in the [configuration](cli-chap-configure.md) topic, you can specify the output format in three ways:
+ **Using the `output` option in a named profile in the `config` file** – The following example sets the default output format to `text`\.

  ```
  [default]
  output=text
  ```
+ **Using the `AWS_DEFAULT_OUTPUT` environment variable** – The following output sets the format to `table` for the commands in this command line session until the variable is changed or the session ends\. Using this environment variable overrides any value set in the `config` file\.

  ```
  $ export AWS_DEFAULT_OUTPUT="table"
  ```
+ **Using the `--output` option on the command line ** – The following example sets the output of only this one command to `json`\. Using this option on the command overrides any currently set environment variable or the value in the `config` file\.

  ```
  $ aws swf list-domains --registration-status REGISTERED --output json
  ```

You can customize and filter the results in any format by using the \-\-query parameter\. For more information, see [How to filter the output with the `--query` option](#cli-usage-output-filter)\. 

## JSON output format<a name="json-output"></a>

[JSON](https://json.org) is the default output format of the AWS CLI\. Most programming languages can easily decode JSON strings using built\-in functions or with publicly available libraries\. You can combine JSON output with the [\-\-query option](#cli-usage-output-filter) in powerful ways to filter and format the AWS CLI JSON\-formatted output\. 

For more advanced filtering that you might not be able to do with `--query`, you can consider `jq`, a command line JSON processor\. You can download it and find the official tutorial at [http://stedolan\.github\.io/jq/](http://stedolan.github.io/jq/)\.

The following is an example of JSON output\.

```
$ aws iam list-users --output json
```

```
{
    "Users": [
        {
            "Path": "/",
            "UserName": "Admin",
            "UserId": "AIDA1111111111EXAMPLE",
            "Arn": "arn:aws:iam::123456789012:user/Admin",
            "CreateDate": "2014-10-16T16:03:09+00:00",
            "PasswordLastUsed": "2016-06-03T18:37:29+00:00"
        },
        {
            "Path": "/backup/",
            "UserName": "backup-user",
            "UserId": "AIDA2222222222EXAMPLE",
            "Arn": "arn:aws:iam::123456789012:user/backup/backup-user",
            "CreateDate": "2019-09-17T19:30:40+00:00"
        },
        {
            "Path": "/",
            "UserName": "cli-user",
            "UserId": "AIDA3333333333EXAMPLE",
            "Arn": "arn:aws:iam::123456789012:user/cli-user",
            "CreateDate": "2019-09-17T19:11:39+00:00"
        }
    ]
}
```

## YAML output format<a name="yaml-output"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

[YAML](https://yaml.org) is a good choice for handling the output programmatically with services and tools that emit or consume [YAML](https://yaml.org)\-formatted strings, such as AWS CloudFormation with its support for [YAML\-formatted templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html)\.

For more advanced filtering that you might not be able to do with `--query`, you can consider `yq`, a command line YAML processor\. You can download it and find documentation at [https://mikefarah.gitbook.io/yq/](https://mikefarah.gitbook.io/yq/)\.

The following is an example of YAML output\.

```
$ aws iam list-users --output yaml
```

```
Users:
- Arn: arn:aws:iam::123456789012:user/Admin
  CreateDate: '2014-10-16T16:03:09+00:00'
  PasswordLastUsed: '2016-06-03T18:37:29+00:00'
  Path: /
  UserId: AIDA1111111111EXAMPLE
  UserName: Admin
- Arn: arn:aws:iam::123456789012:user/backup/backup-user
  CreateDate: '2019-09-17T19:30:40+00:00'
  Path: /backup/
  UserId: AIDA2222222222EXAMPLE
  UserName: arq-45EFD6D1-CE56-459B-B39F-F9C1F78FBE19
- Arn: arn:aws:iam::123456789012:user/cli-user
  CreateDate: '2019-09-17T19:30:40+00:00'
  Path: /
  UserId: AIDA3333333333EXAMPLE
  UserName: cli-user
```

## YAML stream output format<a name="yaml-stream-output"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

The `yaml-stream` format takes advantage of the [YAML](https://yaml.org) format while providing more responsive/faster viewing of large data sets by streaming the data to you\. You can start viewing and using YAML data before the entire query downloads\. 

For more advanced filtering that you might not be able to do with `--query`, you can consider `yq`, a command line YAML processor\. You can download it and find documentation at [http://mikefarah.github.io/yq/](http://mikefarah.github.io/yq/)\.

The following is an example of `yaml-stream` output\.

```
$ aws iam list-users --output yaml-stream
```

```
- IsTruncated: false
  Users:
  - Arn: arn:aws:iam::123456789012:user/Admin
    CreateDate: '2014-10-16T16:03:09+00:00'
    PasswordLastUsed: '2016-06-03T18:37:29+00:00'
    Path: /
    UserId: AIDA1111111111EXAMPLE
    UserName: Admin
  - Arn: arn:aws:iam::123456789012:user/backup/backup-user
    CreateDate: '2019-09-17T19:30:40+00:00'
    Path: /backup/
    UserId: AIDA2222222222EXAMPLE
    UserName: arq-45EFD6D1-CE56-459B-B39F-F9C1F78FBE19
  - Arn: arn:aws:iam::123456789012:user/cli-user
    CreateDate: '2019-09-17T19:30:40+00:00'
    Path: /
    UserId: AIDA3333333333EXAMPLE
    UserName: cli-user
```

The following is an example of `yaml-stream` output in conjunction with using the `--page-size` parameter to paginate the streamed YAML content\.

```
$ aws iam list-users --output yaml-stream --page-size 2
```

```
- IsTruncated: true
  Marker: ab1234cdef5ghi67jk8lmo9p/q012rs3t445uv6789w0x1y2z/345a6b78c9d00/1efgh234ij56klmno78pqrstu90vwxyx  
  Users:
  - Arn: arn:aws:iam::123456789012:user/Admin
    CreateDate: '2014-10-16T16:03:09+00:00'
    PasswordLastUsed: '2016-06-03T18:37:29+00:00'
    Path: /
    UserId: AIDA1111111111EXAMPLE
    UserName: Admin
  - Arn: arn:aws:iam::123456789012:user/backup/backup-user
    CreateDate: '2019-09-17T19:30:40+00:00'
    Path: /backup/
    UserId: AIDA2222222222EXAMPLE
    UserName: arq-45EFD6D1-CE56-459B-B39F-F9C1F78FBE19
- IsTruncated: false
  Users:
  - Arn: arn:aws:iam::123456789012:user/cli-user
    CreateDate: '2019-09-17T19:30:40+00:00'
    Path: /
    UserId: AIDA3333333333EXAMPLE
    UserName: cli-user
```

## Text output format<a name="text-output"></a>

The `text` format organizes the AWS CLI output into tab\-delimited lines\. It works well with traditional Unix text tools such as `grep`, `sed`, and `awk`, and the text processing performed by PowerShell\. 

The `text` output format follows the basic structure shown below\. The columns are sorted alphabetically by the corresponding key names of the underlying JSON object\.

```
IDENTIFIER  sorted-column1 sorted-column2
IDENTIFIER2 sorted-column1 sorted-column2
```

The following is an example of `text` output\. Each field is tab separated from the others, with an extra tab where there is an empty field\.

```
$ aws iam list-users --output text
```

```
USERS   arn:aws:iam::123456789012:user/Admin                2014-10-16T16:03:09+00:00   2016-06-03T18:37:29+00:00   /          AIDA1111111111EXAMPLE   Admin
USERS   arn:aws:iam::123456789012:user/backup/backup-user   2019-09-17T19:30:40+00:00                               /backup/   AIDA2222222222EXAMPLE   backup-user
USERS   arn:aws:iam::123456789012:user/cli-user             2019-09-17T19:11:39+00:00                               /          AIDA3333333333EXAMPLE   cli-user
```

The fourth column is the `PasswordLastUsed` field, and is empty for the last two entries because those users never sign in to the AWS Management Console\.

**Important**  
*We strongly recommend that if you specify `text` output, you also always use the [`--query`](#cli-usage-output-filter) option to ensure consistent behavior*\.   
This is because the text format alphabetically orders output columns by the key name of the underlying JSON object returned by the AWS service, and similar resources might not have the same key names\. For example, the JSON representation of a Linux\-based Amazon EC2 instance might have elements that are not present in the JSON representation of a Windows\-based instance, or vice versa\. Also, resources might have key\-value elements added or removed in future updates, altering the column ordering\. This is where `--query` augments the functionality of the `text` output to provide you with complete control over the output format\.   
In the following example, the command specifies which elements to display and *defines the ordering* of the columns with the list notation `[key1, key2, ...]`\. This gives you full confidence that the correct key values are always displayed in the expected column\. Finally, notice how the AWS CLI outputs None as the value for keys that don't exist\.  

```
$ aws iam list-users --output text --query 'Users[*].[UserName,Arn,CreateDate,PasswordLastUsed,UserId]'
```

```
Admin         arn:aws:iam::123456789012:user/Admin         2014-10-16T16:03:09+00:00   2016-06-03T18:37:29+00:00   AIDA1111111111EXAMPLE
backup-user   arn:aws:iam::123456789012:user/backup-user   2019-09-17T19:30:40+00:00   None                        AIDA2222222222EXAMPLE
cli-user      arn:aws:iam::123456789012:user/cli-backup    2019-09-17T19:11:39+00:00   None                        AIDA3333333333EXAMPLE
```

The following example shows how you can use `grep` and `awk` with the text output from the `aws ec2 describe-instances` command\. The first command displays the Availability Zone, current state, and the instance ID of each instance in `text` output\. The second command processes that output to display only the instance IDs of all running instances in the `us-west-2a` Availability Zone\.

```
$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text
```

```
us-west-2a      running i-4b41a37c
us-west-2a      stopped i-a071c394
us-west-2b      stopped i-97a217a0
us-west-2a      running i-3045b007
us-west-2a      running i-6fc67758
```

```
$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text | grep us-west-2a | grep running | awk '{print $3}'
```

```
i-4b41a37c
i-3045b007
i-6fc67758
```

The following example goes a step further and shows not only how to filter the output, but how to use that output to automate changing instance types for each stopped instance\.

```
$ aws ec2 describe-instances --query 'Reservations[*].Instances[*].[State.Name, InstanceId]' --output text |
> grep stopped |
> awk '{print $2}' |
> while read line;
> do aws ec2 modify-instance-attribute --instance-id $line --instance-type '{"Value": "m1.medium"}';
> done
```

The `text` output can also be useful in PowerShell\. Because the columns in `text` output are tab delimited, you can easily split the output into an array by using PowerShell's ``t` delimiter\. The following command displays the value of the third column \(`InstanceId`\) if the first column \(`AvailabilityZone`\) matches the string `us-west-2a`\.

```
PS C:\>aws ec2 describe-instances --query 'Reservations[*].Instances[*].[Placement.AvailabilityZone, State.Name, InstanceId]' --output text |
%{if ($_.split("`t")[0] -match "us-west-2a") { $_.split("`t")[2]; } }
```

```
-4b41a37c
i-a071c394
i-3045b007
i-6fc67758
```

Notice that although the previous example does show how to use the `--query` parameter to parse the underlying JSON objects and pull out the desired column, PowerShell has its own ability to handle JSON, if cross\-platform compatibility isn't a concern\. Instead of handling the output as text, as most command shells require, PowerShell lets you use the `ConvertFrom-JSON` cmdlet to produce a hierarchically structured object\. You can then directly access the member you want from that object\.

```
(aws ec2 describe-instances --output json | ConvertFrom-Json).Reservations.Instances.InstanceId
```

**Tip**  
If you output text, and filter the output to a single field using the `--query` parameter, the output is a single line of tab\-separated values\. To get each value onto a separate line, you can put the output field in brackets, as shown in the following examples\.  
Tab separated, single\-line output:  

```
$ aws iam list-groups-for-user --user-name susan  --output text --query "Groups[].GroupName"
```

```
HRDepartment    Developers      SpreadsheetUsers  LocalAdmins
```
Each value on its own line by putting `[GroupName]` in brackets:  

```
$ aws iam list-groups-for-user --user-name susan  --output text --query "Groups[].[GroupName]"
```

```
HRDepartment
Developers
SpreadsheetUsers
LocalAdmins
```

## Table output format<a name="table-output"></a>

The `table` format produces human\-readable representations of complex AWS CLI output in a tabular form\.

```
$ aws iam list-users --output table
```

```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
|                                                                                 ListUsers                                                                     |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
||                                                                                  Users                                                                      ||
|+----------------------------------------------------+---------------------------+---------------------------+----------+-----------------------+-------------+|
||                         Arn                        |       CreateDate          |    PasswordLastUsed       |   Path   |        UserId         |   UserName  ||
|+----------------------------------------------------+---------------------------+---------------------------+----------+-----------------------+-------------+|
||  arn:aws:iam::123456789012:user/Admin              | 2014-10-16T16:03:09+00:00 | 2016-06-03T18:37:29+00:00 | /        | AIDA1111111111EXAMPLE | Admin       ||
||  arn:aws:iam::123456789012:user/backup/backup-user | 2019-09-17T19:30:40+00:00 |                           | /backup/ | AIDA2222222222EXAMPLE | backup-user ||
||  arn:aws:iam::123456789012:user/cli-user           | 2019-09-17T19:11:39+00:00 |                           | /        | AIDA3333333333EXAMPLE | cli-user    ||
+---------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

You can combine the `--query` option with the `table` format to display a set of elements preselected from the raw output\. Notice the output differences between dictionary and list notations: in the first example, column names are ordered alphabetically, and in the second example, unnamed columns are ordered as defined by the user\. For more information about the `--query` option, see [How to filter the output with the `--query` option](#cli-usage-output-filter)\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,InstanceId:Attachments[0].InstanceId,AZ:AvailabilityZone,Size:Size}' --output table
```

```
------------------------------------------------------
|                   DescribeVolumes                  | 
+------------+----------------+--------------+-------+
|     AZ     |      ID        | InstanceId   | Size  |
+------------+----------------+--------------+-------+
|  us-west-2a|  vol-e11a5288  |  i-a071c394  |  30   |
|  us-west-2a|  vol-2e410a47  |  i-4b41a37c  |  8    |
+------------+----------------+--------------+-------+
```

```
$ aws ec2 describe-volumes --query 'Volumes[*].[VolumeId,Attachments[0].InstanceId,AvailabilityZone,Size]' --output table
```

```
----------------------------------------------------
|                  DescribeVolumes                 |
+--------------+--------------+--------------+-----+
|  vol-e11a5288|  i-a071c394  |  us-west-2a  |  30 |
|  vol-2e410a47|  i-4b41a37c  |  us-west-2a  |  8  |
+--------------+--------------+--------------+-----+
```

## How to filter the output with the `--query` option<a name="cli-usage-output-filter"></a>

The AWS CLI provides built\-in JSON\-based output filtering capabilities with the `--query` option\. The `--query` parameter accepts strings that are compliant with the [JMESPath specification](http://jmespath.org/)\. 

**Important**  
The output type you specify \(`json`, `yaml`, `text`, or `table`\) impacts how the `--query` option operates:  
If you specify `--output text`, the output is paginated *before* the `--query` filter is applied, and the AWS CLI runs the query once on *each page* of the output\. This can result in unexpected extra output, especially if your filter specifies an array element using something like \[0\], because the output then includes the first matching element on each page\. To work around the extra output that `--output text` can produce, you can specify `--no-paginate`\. This causes the filter to apply only to the complete set of results\. But it does remove any pagination, so it might result in long output\. You can also use other command line tools such as `head` or `tail` to additionally filter the output to only the values you want\. 
If you specify `--output json`, the output is completely processed as a single, native JSON structure before the `--query` filter is applied\. The AWS CLI runs the query only once against the entire JSON structure, producing a filtered JSON result that is then output\.
If you specify `--output yaml`, the output is completely processed as a single, native JSON structure before the `--query` filter is applied\. The AWS CLI runs the query only once against the entire JSON structure, producing a filtered JSON result that is then converted to YAML and output\.

To demonstrate how `--query` works, we start with the following default JSON output\. This describes two Amazon Elastic Block Store \(Amazon EBS\) volumes attached to separate Amazon EC2 instances\.

```
$ aws ec2 describe-volumes
```

```
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

You can choose to display only the first volume from the `Volumes` list by using the following command that [indexes the first volume in the array](http://jmespath.org/specification.html#index-expressions)\.

```
$ aws ec2 describe-volumes --query 'Volumes[0]'
```

```
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

The next example uses the [wildcard notation `[*]`](http://jmespath.org/specification.html#wildcard-expressions) to iterate over all of the volumes in the list, filtering out three elements from each volume: `VolumeId`, `AvailabilityZone`, and `Size`\. The dictionary notation requires that you provide an alias for each JSON key, like this: \{Alias1:JSONKey1,Alias2:JSONKey2\}\. A dictionary is inherently *unordered*, so the ordering of the keys/aliases within a structure might be inconsistent\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,AZ:AvailabilityZone,Size:Size}'
```

```
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

Using dictionary notation, you can also chain keys together, like `key1.key2[0].key3`, to filter elements deeply nested within the structure\. The following example demonstrates this with the `Attachments[0].InstanceId` key, aliased to simply `InstanceId`\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].{ID:VolumeId,InstanceId:Attachments[0].InstanceId,AZ:AvailabilityZone,Size:Size}'
```

```
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

You can also filter multiple elements using list notation: `[key1, key2]`\. This formats all filtered attributes into a single *ordered* list per object, regardless of type\.

```
$ aws ec2 describe-volumes --query 'Volumes[*].[VolumeId, Attachments[0].InstanceId, AvailabilityZone, Size]'
```

```
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

To filter results by the value of a specific field, use the [JMESPath "?" operator](http://jmespath.org/specification.html#filter-expressions)\. The following example query outputs only volumes in the `us-west-2a` Availability Zone\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[?AvailabilityZone==`us-west-2a`]'
```

**Note**  
When specifying a literal value such as `"us-west-2"` above in a JMESPath query expression, you must surround the value in backticks \(` `\) for it to be read properly\.

Here are some additional examples that illustrate how you can get only the details you want from the output of your commands\.

The following example lists Amazon EC2 volumes\. The service produces a list of all attached volumes in the `us-west-2a` Availability Zone\. The `--query` parameter further limits the output to only those volumes with a `Size` value that is larger than 50, and shows only the specified fields with user\-defined names\.

```
$ aws ec2 describe-volumes \
    --filters "Name=availability-zone,Values=us-west-2a" "Name=status,Values=attached" \
    --query 'Volumes[?Size > `50`].{Id:VolumeId,Size:Size,Type:VolumeType}'
```

```
[
    {
        "Id": "vol-0be9bb0bf12345678",
        "Size": 80,
        "Type": "gp2"
    }
]
```

The following example retrieves a list of images that meet several criteria\. It then uses the `--query` parameter to sort the output by `CreationDate`, selecting only the most recent\. Finally, it displays the `ImageId` of that one image\.

```
$ aws ec2 describe-images \
    --owners amazon \
    --filters "Name=name,Values=amzn*gp2" "Name=virtualization-type,Values=hvm" "Name=root-device-type,Values=ebs" \
    --query "sort_by(Images, &CreationDate)[-1].ImageId" \
    --output text
```

```
ami-00ced3122871a4921
```

The following example uses the `--query` parameter to find a specific item in a list and then extracts information from that item\. The example lists all of the Availability Zones associated with the specified service endpoint\. It extracts the item from the `ServiceDetails` list that has the specified `ServiceName`, then outputs the `AvailabilityZones` field from that selected item\. 

```
$ aws --region us-east-1 ec2 describe-vpc-endpoint-services \
    --query 'ServiceDetails[?ServiceName==`com.amazonaws.us-east-1.ecs`].AvailabilityZones'
```

```
[
    [
        "us-east-1a",
        "us-east-1b",
        "us-east-1c",
        "us-east-1d",
        "us-east-1e",
        "us-east-1f"
    ]
]
```

The `--query` parameter also enables you to count items in the output\. The following example displays the number of available volumes that are more than 1000 IOPS by using `length` to count how many are in a list\.

```
$ aws ec2 describe-volumes \
    --filters "Name=status,Values=available" \
    --query 'length(Volumes[?Iops > `1000`])'
```

```
3
```

The following example shows how to list all of your snapshots that were created after a specified date, including only a few of the available fields in the output\.

```
$ aws ec2 describe-snapshots --owner self \
    --output json \
    --query 'Snapshots[?StartTime>=`2018-02-07`].{Id:SnapshotId,VId:VolumeId,Size:VolumeSize}' \
```

```
[
    {
        "id": "snap-0effb42b7a1b2c3d4",
        "vid": "vol-0be9bb0bf12345678",
        "Size": 8
    }
]
```

The following example lists the five most recent Amazon Machine Images \(AMIs\) that you created, sorted from most recent to oldest\.

```
$ aws ec2 describe-images \
    --owners self \
    --query 'reverse(sort_by(Images,&CreationDate))[:5].{id:ImageId,date:CreationDate}'
```

```
[
    {
        "id": "ami-0a1b2c3d4e5f60001",
        "date": "2018-11-28T17:16:38.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60002",
        "date": "2018-09-15T13:51:22.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60003",
        "date": "2018-08-19T10:22:45.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60004",
        "date": "2018-05-03T12:04:02.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60005",
        "date": "2017-12-13T17:16:38.000Z"
    }

]
```

This following example shows only the `InstanceId` for any unhealthy instances in the specified Auto Scaling group\.

```
$ aws autoscaling describe-auto-scaling-groups \
    --auto-scaling-group-name My-AutoScaling-Group-Name \
    --output text \
    --query 'AutoScalingGroups[*].Instances[?HealthStatus==`Unhealthy`].InstanceId'
```

Combined with the output formats that are explained in more detail previously in this topic, the `--query` option is a powerful tool you can use to customize the content and style of outputs\. 

For more examples and the full spec of JMESPath, the underlying JSON\-processing library, see [http://jmespath\.org/specification\.html](http://jmespath.org/specification.html)\.

## How to set the output’s default pager program<a name="cli-usage-output-pager"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

AWS CLI version 2 provides the use of a client\-side pager program for output\. By default, this feature returns all output through your operating system’s default pager program\. Client\-side pagination occurs after any server\-side pagination you specify, see [Pagination](cli-usage-pagination.md)\. 

To disable all use of an external paging program, set the variable to an empty string\. 

You can specify the output pager in two ways\.

**Using the `cli_pager` option in the `config` file**

The following example sets the default output pager to the `less` program\.

```
[default]
cli_pager=less
```

The following example disabled the use of a pager\.

```
[default]
cli_pager=
```

**Using the `AWS_PAGER` environment variable** 

The following example sets the default to less\.

------
#### [ Linux and macOS ]

```
$ export AWS_PAGER="less"
```

------
#### [ Windows ]

```
C:\> setx AWS_PAGER "less"
```

------

The following example disables the use of a pager\.

------
#### [ Linux and macOS ]

```
$ export AWS_PAGER=""
```

------
#### [ Windows ]

```
C:\> setx AWS_PAGER ""
```

------