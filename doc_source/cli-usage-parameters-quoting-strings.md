# Using quotation marks with strings in the AWS CLI<a name="cli-usage-parameters-quoting-strings"></a>

There are primarily two ways single and double quotes are used in the AWS CLI\.
+ [Using quotation marks around strings that contain white spaces](#cli-usage-parameters-quoting-strings-around)
+ [Using quotation marks inside strings](#cli-usage-parameters-quoting-strings-containing)

## Using quotation marks around strings that contain white spaces<a name="cli-usage-parameters-quoting-strings-around"></a>

Parameter names and their values are separated by spaces on the command line\. If a string value contains an embedded space, then you must surround the entire string with quotation marks to prevent the AWS CLI from misinterpreting the space as a divider between the value and the next parameter name\. Which type of quotation mark you use depends on the operating system you are running the AWS CLI on\.

------
#### [ Linux and macOS ]

Use single quotation marks `' '` 

```
$ aws ec2 create-key-pair --key-name 'my key pair'
```

For more information on using quotes, see the user documentation for your preferred shell\.

------
#### [ PowerShell ]

Use single quotation marks `' '` or double quotation marks `" "` \.

```
PS C:\> aws ec2 create-key-pair --key-name 'my key pair'
```

```
PS C:\> aws ec2 create-key-pair --key-name "my key pair"
```

For more information on using quotes, see [About Quoting Rules](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7) in the *Microsoft PowerShell Docs*\.

------
#### [ Windows command prompt ]

Use double quotation marks `" "` \.

```
C:\> aws ec2 create-key-pair --key-name "my key pair"
```

------

Optionally, you can separate the parameter name from the value with an equals sign `=` instead of a space\. This is typically necessary only if the value of the parameter starts with a hyphen\.

```
$ aws ec2 delete-key-pair --key-name=-mykey
```

## Using quotation marks inside strings<a name="cli-usage-parameters-quoting-strings-containing"></a>

Strings may contain quotation marks, and your shell may require escaping quotations for them to work properly\. One of the common parameter value types is a JSON string\. This is complex since it includes spaces and double quotation marks `" "` around each element name and value in the JSON structure\. The way you enter JSON\-formatted parameters on the command line differs depending on your operating system\. 

------
#### [ Linux and macOS ]

For Linux and macOS to interpret strings literally use single quotation marks `' '` to enclose the JSON data structure, as in the following example\. You do not need to escape double quotation marks embedded in the JSON string, as they are being treated literally\. Since the JSON is enclosed in single quotation marks, any single quotation marks in the string will need to be escaped, this is usually accomplished using a backslash before the single quote `\'`\.

```
$ aws ec2 run-instances \
    --image-id ami-12345678 \
    --block-device-mappings '[{"DeviceName":"/dev/sdb","Ebs":{"VolumeSize":20,"DeleteOnTermination":false,"VolumeType":"standard"}}]'
```

For more information on using quotes, see the user documentation for your preferred shell\.

------
#### [ PowerShell ]

Use single quotation marks `' '` or double quotation marks `" "`\.

**Single quotations \(recommended\)**

Since JSON data structures include double quotes, we suggest **single** quotation marks `' '` to enclose it\. If you use **single** quotation marks, you do not need to escape **double** quotation marks embedded in the JSON string\. However, you need to escape each **single** quotation mark with a backtick ``` within the JSON structure\.

```
PS C:\> aws ec2 run-instances `
    --image-id ami-12345678 `
    --block-device-mappings '[{"DeviceName":"/dev/sdb","Ebs":{"VolumeSize":20,"DeleteOnTermination":false,"VolumeType":"standard"}}]'
```

**Double quotations**

If you use **double** quotation marks, you do not need to escape **single** quotation marks embedded in the JSON string\. However, you need to escape each **double** quotation mark with a backtick ``` within the JSON structure, as with the following example\.

```
PS C:\> aws ec2 run-instances `
    --image-id ami-12345678 `
    --block-device-mappings "[{`"DeviceName`":`"/dev/sdb`",`"Ebs`":{`"VolumeSize`":20,`"DeleteOnTermination`":false,`"VolumeType`":`"standard`"}}]"
```

For more information on using quotes, see [About Quoting Rules](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_quoting_rules?view=powershell-7) in the *Microsoft PowerShell Docs*\.

------
#### [ Windows command prompt ]

The Windows command prompt requires double quotation marks `" "` to enclose the JSON data structure\. Also, to prevent the command processor from misinterpreting the double quotation marks embedded in the JSON, you must also escape \(precede with a backslash `\` character\) each double quotation mark `"` within the JSON data structure itself, as in the following example\. 

```
C:\> aws ec2 run-instances ^
    --image-id ami-12345678 ^
    --block-device-mappings "[{\"DeviceName\":\"/dev/sdb\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false,\"VolumeType\":\"standard\"}}]"
```

Only the outermost double quotation marks are not escaped\.

------

For more advanced JSON usage in a command line, consider using a command line JSON processor, like `jq`, to create JSON strings\. For more information on `jq`, see the [jq repository](http://stedolan.github.io/jq/) on *GitHub*\.