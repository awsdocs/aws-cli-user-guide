# Common AWS CLI parameter types<a name="cli-usage-parameters-types"></a>

This section describes some of the common parameter types and the typical required format\. If you are having trouble formatting a parameter for a specific command, check the help by entering **help** after the command name, as shown\. 

```
$ aws ec2 describe-spot-price-history help
```

The help for each subcommand describes its function, options, output, and examples\. The options section includes the name and description of each option with the option's parameter type in parentheses\. 

## String<a name="parameter-type-string"></a>

String parameters can contain alphanumeric characters, symbols, and white space from the [ASCII](https://wikipedia.org/wiki/ASCII) character set\. Strings that contain white space must be surrounded by quotation marks\. We recommend that you don't use symbols or white space other than the standard space character because it can cause unexpected results\. 

Some string parameters can accept binary data from a file\. See [Binary files](cli-usage-parameters-file.md#cli-usage-parameters-file-binary) for an example\. 

## Timestamp<a name="parameter-type-timestamp"></a>

Timestamps are formatted according to the [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) standard\. These are sometimes referred to as "DateTime" or "Date" parameters\. 

```
$ aws ec2 describe-spot-price-history --start-time 2014-10-13T19:00:00Z
```

Acceptable formats include:
+ *YYYY*\-*MM*\-*DD*T*hh*:*mm*:*ss\.sss**TZD \(UTC\)*, for example, 2014\-10\-01T20:30:00\.000Z
+ *YYYY*\-*MM*\-*DD*T*hh*:*mm*:*ss\.sss**TZD \(with offset\)*, for example, 2014\-10\-01T12:30:00\.000\-08:00
+ *YYYY*\-*MM*\-*DD*, for example, 2014\-10\-01
+ Unix time in seconds, for example, 1412195400\. This is sometimes referred to as [Unix Epoch time](https://wikipedia.org/wiki/Unix_time) and represents the number of seconds since midnight, January 1, 1970 UTC\.

*\(Available in the AWS CLI version 2 only\.\)* By default, the AWS CLI version 2 translates all ***response*** DateTime values to ISO 8601 format\.

## List<a name="parameter-type-list"></a>

One or more strings separated by spaces\. If any of the string items contain a space, you must put quotation marks around that item\.

```
$ aws ec2 describe-spot-price-history --instance-types m1.xlarge m1.medium
```

 **Boolean** – Binary flag that turns an option on or off\. For example, `ec2 describe-spot-price-history` has a Boolean `--dry-run` parameter that, when specified, validates the query with the service without actually running the query\. 

```
$ aws ec2 describe-spot-price-history --dry-run
```

The output indicates whether the command was well formed\. This command also includes a `--no-dry-run` version of the parameter that you can use to explicitly indicate that the command should be run normally\. Including it isn't necessary because this is the default behavior\. 

## Integer<a name="parameter-type-integer"></a>

An unsigned, whole number\.

```
$ aws ec2 describe-spot-price-history --max-items 5
```

## Binary/Blob \(binary large object\)<a name="parameter-type-blob"></a>

In the AWS CLI version 1, to pass a value to a parameter with type `blob`, you must specify a path to a local file that contains the binary data\. The path should not contain any protocol identifier, such as `http://` or `file://`\. The specified path is interpreted as being relative to the current working directory\. For example, the `--body` parameter for `aws s3api put-object` is a blob\.

```
$ aws s3api put-object --bucket my-bucket --key testimage.png --body /tmp/image.png
```

*\(Available in the AWS CLI version 2 only\.\)* In the AWS CLI version 2, you can pass a binary value as a base64\-encoded string directly on the command line\. Also, by default in the AWS CLI version 2, files referenced with the `file://` prefix are treated as base64\-encoded text\. 

You can revert the AWS CLI version 2 to be compatible with AWS CLI version 1 by setting the `cli-binary-format` setting:
+ If the setting's value is `raw-in-base64-out`, files referenced using the `file://` prefix are treated as raw unencoded binary\.
+ If the setting's value is `base64` \(the default value\), files referenced using the `file://` prefix are treated as base64\-encoded text\.

Files referenced using the `fileb://` prefix are always treated as raw unencoded binary, regardless of the `cli_binary_format` setting\. 

For more information, see the setting `cli-binary-format`\.

## Map<a name="parameter-type-map"></a>

A set of key\-value pairs specified in JSON or by using the CLI's [shorthand syntax](cli-usage-shorthand.md)\. The following JSON example reads an item from an Amazon DynamoDB table named *my\-table* with a map parameter, `--key`\. The parameter specifies the primary key named *id* with a number value of *1* in a nested JSON structure\. 

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