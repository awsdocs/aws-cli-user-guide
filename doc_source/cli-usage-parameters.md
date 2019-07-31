# Specifying Parameter Values for the AWS CLI<a name="cli-usage-parameters"></a>

Many parameters used in the AWS Command Line Interface \(AWS CLI\) are simple string or numeric values, such as the key pair name `my-key-pair` in the following example\. 

```
$ aws ec2 create-key-pair --key-name my-key-pair
```

Strings without any space characters can be surrounded with quotation marks or not\. However, you must use quotation marks around strings that include one or more space characters\. Use single quotation marks \(' '\) in Linux, macOS, Unix, or PowerShell\. Use double quotation marks \(" "\) in the Windows command prompt, as shown in the following examples\. 

**PowerShell, Linux, macOS, or Unix**

```
$ aws ec2 create-key-pair --key-name 'my key pair'
```

**Windows command prompt**

```
C:\> aws ec2 create-key-pair --key-name "my key pair"
```

Optionally, you can optionally separate the parameter name from the value with an equals sign \(=\) instead of a space\. This is typically necessary only if the value of the parameter starts with a hyphen\.

```
$ aws ec2 delete-key-pair --key-name=-mykey
```

**Topics**
+ [Common Parameter Types](#parameter-types)
+ [Using JSON for Parameters](#cli-usage-parameters-json)
+ [Using Quotation Marks with Strings](#quoting-strings)
+ [Loading Parameters from a File](#cli-usage-parameters-file)

## Common Parameter Types<a name="parameter-types"></a>

This section describes some of the common parameter types and the typical required format\. If you are having trouble formatting a parameter for a specific command, check the help by typing **help** after the command name, for example: 

```
$ aws ec2 describe-spot-price-history help
```

The help for each subcommand describes its function, options, output, and examples\. The options section includes the name and description of each option with the option's parameter type in parentheses\. 

 **String** – String parameters can contain alphanumeric characters, symbols, and white space from the [ASCII](https://wikipedia.org/wiki/ASCII) character set\. Strings that contain white space must be surrounded by quotation marks\. We recommend that you don't use symbols or white space other than the standard space character because it can cause unexpected results\. 

Some string parameters can accept binary data from a file\. See [Binary Files](#cli-usage-parameters-file-binary) for an example\. 

 **Timestamp** – Timestamps are formatted according to the [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) standard\. These are sometimes referred to as "DateTime" or "Date" parameters\. 

```
$ aws ec2 describe-spot-price-history --start-time 2014-10-13T19:00:00Z
```

Acceptable formats include:
+ *YYYY*\-*MM*\-*DD*T*hh*:*mm*:*ss\.sss**TZD \(UTC\)*, for example, 2014\-10\-01T20:30:00\.000Z
+ *YYYY*\-*MM*\-*DD*T*hh*:*mm*:*ss\.sss**TZD \(with offset\)*, for example, 2014\-10\-01T12:30:00\.000\-08:00
+ *YYYY*\-*MM*\-*DD*, for example, 2014\-10\-01
+ Unix time in seconds, for example 1412195400\. This is sometimes referred to as [Unix Epoch Time](https://wikipedia.org/wiki/Unix_time) and represents the number of seconds since midnight, January 1, 1970 UTC\.

 **List** – One or more strings separated by spaces\. If any of the string items contain a space, you must put quotation marks around that item\.

```
$ aws ec2 describe-spot-price-history --instance-types m1.xlarge m1.medium
```

 **Boolean** – Binary flag that turns an option on or off\. For example, `ec2 describe-spot-price-history` has a Boolean `--dry-run` parameter that, when specified, validates the query with the service without actually running the query\. 

```
$ aws ec2 describe-spot-price-history --dry-run
```

The output indicates whether the command was well formed\. This command also includes a `--no-dry-run` version of the parameter that you can use to explicitly indicate that the command should be run normally\. Including it isn't necessary because this is the default behavior\. 

 **Integer** – An unsigned, whole number\. 

```
$ aws ec2 describe-spot-price-history --max-items 5
```

 **Blob** – Binary object\. Blob parameters take a path to a local file that contains the binary data\. The path should not contain any protocol identifier, such as `http://` or `file://`\. The specified path is interpreted as being relative to the current working directory\.

For example, the `--body` parameter for `aws s3api put-object` is a blob\. 

```
$ aws s3api put-object --bucket my-bucket --key testimage.png --body /tmp/image.png
```

 **Map** – A set of key\-value pairs specified in JSON or by using the CLI's [shorthand syntax](cli-usage-shorthand.md)\. The following JSON example reads an item from an Amazon DynamoDB table named *my\-table* with a map parameter, `--key`\. The parameter specifies the primary key named *id* with a number value of *1* in a nested JSON structure\. 

```
$ aws dynamodb get-item --table-name my-table --key '{"id": {"N":"1"}}'
{
    "Item": {
        "name": {
            "S": "John"
        },
        "id": {
            "N": "1"
        }
    }
}
```

## Using JSON for Parameters<a name="cli-usage-parameters-json"></a>

JSON is useful for specifying complex command line parameters\. For example, the following command lists all Amazon EC2 instances that have an instance type of `m1.small` or `m1.medium` that are also in the `us-west-2c` Availability Zone\. 

```
$ aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro,m1.medium" "Name=availability-zone,Values=us-west-2c"
```

Alternatively, you can specify the equivalent list of filters as a JSON array\. Square brackets are used to create an array of JSON objects separated by commas\. Each object is a comma\-separated list of key\-value pairs \(in this example, "Name" and "Values" are both keys\)\. 

The value to the right of the "Values" key is itself an array\. This is required, even if the array contains only one value string\. 

```
[
  {
    "Name": "instance-type",
    "Values": ["t2.micro", "m1.medium"]
  },
  {
    "Name": "availability-zone",
    "Values": ["us-west-2c"]
  }
]
```

The outermost brackets, however, are required only if more than one filter is specified\. A single filter version of the previous command, formatted in JSON, looks like this\. 

```
$ aws ec2 describe-instances --filters '{"Name": "instance-type", "Values": ["t2.micro", "m1.medium"]}'
```

For some operations, you *must* format the data as JSON\. For example, to pass parameters to the `--block-device-mappings` parameter in the `ec2 run-instances` command, you need to format the block device information as JSON\. 

This example shows the JSON to specify a single 20 GiB Amazon Elastic Block Store \(Amazon EBS\) device to be mapped at `/dev/sdb` on the launching instance\. 

```
{
  "DeviceName": "/dev/sdb",
  "Ebs": {
    "VolumeSize": 20,
    "DeleteOnTermination": false,
    "VolumeType": "standard"
  }
}
```

To attach multiple devices, list the objects in an array, as shown in the next example\. 

```
[
  {
    "DeviceName": "/dev/sdb",
    "Ebs": {
      "VolumeSize": 20,
      "DeleteOnTermination": false,
      "VolumeType": "standard"
    }
  },
  {
    "DeviceName": "/dev/sdc",
    "Ebs": {
      "VolumeSize": 10,
      "DeleteOnTermination": true,
      "VolumeType": "standard"
    }
  }
]
```

You can enter the JSON directly on the command line \(see [Using Quotation Marks with Strings](#quoting-strings)\), or save it to a file that is referenced from the command line \(see [Loading Parameters from a File](#cli-usage-parameters-file)\)\. 

When passing in large blocks of data, you might find it easier to first save the JSON to a file and then reference it from the command line\. JSON data in a file is easier to read, edit, and share with others\. This technique is described in a later section\. 

For more information about JSON, see [JSON\.org](https://json.org), [Wikipedia's JSON entry](https://wikipedia.org/wiki/JSON), and [RFC4627 \- The application/json Media Type for JSON](http://tools.ietf.org/html/rfc4627)\. 

## Using Quotation Marks with Strings<a name="quoting-strings"></a>

The way you enter JSON\-formatted parameters on the command line differs depending upon your operating system\. 

Linux, macOS, or Unix  
Use single quotation marks \(' '\) to enclose the JSON data structure, as in the following example:   

```
$ aws ec2 run-instances --image-id ami-12345678 --block-device-mappings '[{"DeviceName":"/dev/sdb","Ebs":{"VolumeSize":20,"DeleteOnTermination":false,"VolumeType":"standard"}}]'
```

PowerShell  
PowerShell requires single quotation marks \(' '\) to enclose the JSON data structure, as well as a backslash \(\\\) to escape each double quotation mark \("\) within the JSON structure, as in the following example:   

```
PS C:\> aws ec2 run-instances --image-id ami-12345678 --block-device-mappings '[{\"DeviceName\":\"/dev/sdb\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false,\"VolumeType\":\"standard\"}}]'
```

Windows Command Prompt  
The Windows command prompt requires double quotation marks \(" "\) to enclose the JSON data structure\. You must then escape \(precede with a backslash \[ \\ \] character\) each double quotation mark \("\) within the JSON data structure itself, as in the following example:   

```
C:\> aws ec2 run-instances --image-id ami-12345678 --block-device-mappings "[{\"DeviceName\":\"/dev/sdb\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false,\"VolumeType\":\"standard\"}}]"
```
Only the outermost double quotation marks are not escaped\.

If the value of a parameter is itself a JSON document, escape the quotation marks on the embedded JSON document\. For example, the `attribute` parameter for `aws sqs create-queue` can take a `RedrivePolicy` key\. The `--attributes` parameter takes a JSON document, which in turn contains `RedrivePolicy`, which also takes a JSON document as its value\. The inner JSON embedded in the outer JSON must be escaped\.

```
$ aws sqs create-queue --queue-name my-queue --attributes '{ "RedrivePolicy":"{\"deadLetterTargetArn\":\"arn:aws:sqs:us-west-2:0123456789012:deadletter\", \"maxReceiveCount\":\"5\"}"}'
```

## Loading Parameters from a File<a name="cli-usage-parameters-file"></a>

Some parameters expect file names as arguments, from which the AWS CLI loads the data\. Other parameters enable you to specify the parameter value as either text typed on the command line or read from a file\. Whether a file is required or optional, the file must be encoded correctly to be understood by the AWS CLI\. The file's encoding must match the reading system's default locale\. This can be determined by using the Python `locale.getpreferredencoding()` method\.

**Note**  
By default, Windows PowerShell outputs text as UTF\-16, which conflicts with the UTF\-8 encoding used by many Linux systems\. We recommend that you use `-Encoding ascii` with your PowerShell `Out-File` commands to ensure the resulting file can be read by the AWS CLI\. 

Sometimes it's convenient to load a parameter value from a file instead of trying to type it all as a command line parameter value, such as when the parameter is a complex JSON string\. To specify a file that contains the value, specify a file URL in the following format:

```
file://complete/path/to/file
```

The first two slash '/' characters are part of the specification\. If the required path begins with a '/', then the result is three slash characters: `file:///folder/file`\.

The URL provides the path to the file that contains the actual parameter content\. 

**Note**  
This behaviour is disabled automatically for parameters that already expect a URL, such as parameter that identifies a AWS CloudFormation template URL\.  
You can also disable this behaviour yourself by adding the following line to your CLI configuration file:  

```
cli_follow_urlparam = false
```

The file paths in the following examples are interpreted to be relative to the current working directory\.

**Linux, macOS, or Unix**

```
// Read from a file in the current directory
$ aws ec2 describe-instances --filters file://filter.json

// Read from a file in /tmp
$ aws ec2 describe-instances --filters file:///tmp/filter.json
```

**Windows**

```
// Read from a file in C:\temp
C:\> aws ec2 describe-instances --filters file://C:\temp\filter.json
```

The `file://` prefix option supports Unix\-style expansions, including "`~/`", "`./`", and "`../`"\. On Windows, the "`~/`" expression expands to your user directory, stored in the `%USERPROFILE%` environment variable\. For example, on Windows 10 you would typically have a user directory under `C:\Users\User Name\`\.

JSON documents that are embedded as the value of another JSON document must still be escaped\.

```
$ aws sqs create-queue --queue-name my-queue --attributes file://attributes.json
```

**attributes\.json**

```
{
  "RedrivePolicy": "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-west-2:0123456789012:deadletter\", \"maxReceiveCount\":\"5\"}"
}
```

### Binary Files<a name="cli-usage-parameters-file-binary"></a>

For commands that take binary data as a parameter, specify that the data is binary content by using the `fileb://` prefix\. Commands that accept binary data include: 
+  **`aws ec2 run-instances`** – `--user-data` parameter\. 
+  **`aws s3api put-object`** – `--sse-customer-key` parameter\. 
+  **`aws kms decrypt`** – `--ciphertext-blob` parameter\. 

The following example generates a binary 256\-bit AES key using a Linux command line tool, and then provides it to Amazon S3 to encrypt an uploaded file server\-side\. 

```
$ dd if=/dev/urandom bs=1 count=32 > sse.key
32+0 records in
32+0 records out
32 bytes (32 B) copied, 0.000164441 s, 195 kB/s
$ aws s3api put-object --bucket my-bucket --key test.txt --body test.txt --sse-customer-key fileb://sse.key --sse-customer-algorithm AES256
{
    "SSECustomerKeyMD5": "iVg8oWa8sy714+FjtesrJg==",
    "SSECustomerAlgorithm": "AES256",
    "ETag": "\"a6118e84b76cf98bf04bbe14b6045c6c\""
}
```

### Remote Files<a name="cli-usage-parameters-file-remote"></a>

The AWS CLI also supports loading parameters from a file hosted on the internet with an `http://` or `https://` URL\. The following example references a file stored in an Amazon S3 bucket\. This allows you to access parameter files from any computer, but it does require that the container is publicly accessible\. 

```
$ aws ec2 run-instances --image-id ami-12345678 --block-device-mappings http://my-bucket.s3.amazonaws.com/filename.json
```

The preceding example assumes that the file `filename.json` contains the following JSON data\.

```
[
  {
    "DeviceName": "/dev/sdb",
    "Ebs": {
      "VolumeSize": 20,
      "DeleteOnTermination": false,
      "VolumeType": "standard"
    }
  }
]
```

For another example referencing a file containing more complex JSON\-formatted parameters, see [Attach an IAM Managed Policy to an IAM User](cli-services-iam-policy.md)\. 