# Common AWS CLI Parameter Types<a name="cli-usage-parameters-types"></a>

This section describes some of the common parameter types and the typical required format\. If you are having trouble formatting a parameter for a specific command, check the help by entering **help** after the command name, as shown\. 

```
$ aws ec2 describe-spot-price-history help
```

The help for each subcommand describes its function, options, output, and examples\. The options section includes the name and description of each option with the option's parameter type in parentheses\. 

## String<a name="parameter-type-string"></a>

String parameters can contain alphanumeric characters, symbols, and white space from the [ASCII](https://wikipedia.org/wiki/ASCII) character set\. Strings that contain white space must be surrounded by quotation marks\. We recommend that you don't use symbols or white space other than the standard space character because it can cause unexpected results\. 

Some string parameters can accept binary data from a file\. See [Binary Files](cli-usage-parameters-file.md#cli-usage-parameters-file-binary) for an example\. 

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

## Blob<a name="parameter-type-blob"></a>

"Binary large object"\. Blob parameters take a path to a local file that contains the binary data\. The path should not contain any protocol identifier, such as `http://` or `file://`\. The specified path is interpreted as being relative to the current working directory\.

For example, the `--body` parameter for `aws s3api put-object` is a blob\. 

```
$ aws s3api put-object --bucket my-bucket --key testimage.png --body /tmp/image.png
```

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