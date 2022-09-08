--------

--------

# Setting the AWS CLI output format<a name="cli-usage-output-format"></a>

This topic describes the different output formats for the AWS Command Line Interface \(AWS CLI\)\. The AWS CLI supports the following output formats:
+ **[`json`](#json-output)** – The output is formatted as a [JSON](https://json.org/) string\.
+  **[`yaml`](#yaml-output)** – The output is formatted as a [YAML](https://yaml.org/) string\.
+ **[`yaml-stream`](#yaml-stream-output)** – The output is streamed and formatted as a [YAML](https://yaml.org/) string\. Streaming allows for faster handling of large data types\.
+ **[`text`](#text-output)** – The output is formatted as multiple lines of tab\-separated string values\. This can be useful to pass the output to a text processor, like `grep`, `sed`, or `awk`\.
+ **[`table`](#table-output)** – The output is formatted as a table using the characters \+\|\- to form the cell borders\. It typically presents the information in a "human\-friendly" format that is much easier to read than the others, but not as programmatically useful\.

## How to select the output format<a name="cli-usage-output-format"></a>

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

## JSON output format<a name="json-output"></a>

[JSON](https://json.org) is the default output format of the AWS CLI\. Most programming languages can easily decode JSON strings using built\-in functions or with publicly available libraries\. You can combine JSON output with the [\-\-query option](cli-usage-filter.md) in powerful ways to filter and format the AWS CLI JSON\-formatted output\. 

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
*We strongly recommend that if you specify `text` output, you also always use the [`--query`](cli-usage-filter.md) option to ensure consistent behavior*\.   
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

You can combine the `--query` option with the `table` format to display a set of elements preselected from the raw output\. Notice the output differences between dictionary and list notations: in the first example, column names are ordered alphabetically, and in the second example, unnamed columns are ordered as defined by the user\. For more information about the `--query` option, see [Filtering AWS CLI output](cli-usage-filter.md)\.

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