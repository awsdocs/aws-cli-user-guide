# Using quotation marks with strings in the AWS CLI<a name="cli-usage-parameters-quoting-strings"></a>

Parameter names and their values are separated by spaces on the command line\. If a string value contains an embedded space, then you must surround the entire string with quotation marks to prevent the AWS CLI from misinterpreting the space as a divider between the value and the next parameter name\. Which type of quotation marks you use depend on the operating system you are running the AWS CLI on\.

Use single quotation marks \(' '\) in Linux, macOS, Unix, or PowerShell\. Use double quotation marks \(" "\) in the Windows command prompt, as shown in the following examples\. 

**Linux or macOS**, **PowerShell**

```
$ aws ec2 create-key-pair --key-name 'my key pair'
```

**Windows command prompt**

```
C:\> aws ec2 create-key-pair --key-name "my key pair"
```

Optionally, you can separate the parameter name from the value with an equals sign \(=\) instead of a space\. This is typically necessary only if the value of the parameter starts with a hyphen\.

```
$ aws ec2 delete-key-pair --key-name=-mykey
```

One of the common parameter value types is a JSON string\. This is complex not only because it includes spaces, but also requires the double quotation marks \(" "\)around each element name and value in the JSON structure\. The way you enter JSON\-formatted parameters on the command line differs depending on your operating system\. 

Linux or macOS  
Use single quotation marks \(' '\) to enclose the JSON data structure, as in the following example\. You don't have to do anything special with the embedded double quotation marks embedded in the JSON string\.  

```
$ aws ec2 run-instances --image-id ami-12345678 --block-device-mappings '[{"DeviceName":"/dev/sdb","Ebs":{"VolumeSize":20,"DeleteOnTermination":false,"VolumeType":"standard"}}]'
```

PowerShell  
PowerShell requires single quotation marks \(' '\) to enclose the JSON data structure\. Also, because double quotation marks have a special meaning to PowerShell, you must use a backslash \(\\\) to *escape* each double quotation mark \("\) within the JSON structure, as in the following example\.  

```
PS C:\> aws ec2 run-instances --image-id ami-12345678 --block-device-mappings '[{\"DeviceName\":\"/dev/sdb\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false,\"VolumeType\":\"standard\"}}]'
```

Windows Command Prompt  
The Windows command prompt requires double quotation marks \(" "\) to enclose the JSON data structure\. Also, to prevent the command processor from misinterpreting the double quotation marks embedded in the JSON, you must also escape \(precede with a backslash \[ \\ \] character\) each double quotation mark \("\) within the JSON data structure itself, as in the following example\.   

```
C:\> aws ec2 run-instances --image-id ami-12345678 --block-device-mappings "[{\"DeviceName\":\"/dev/sdb\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false,\"VolumeType\":\"standard\"}}]"
```
Only the outermost double quotation marks are not escaped\.