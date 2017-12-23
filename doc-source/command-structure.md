# Command Structure in the AWS Command Line Interface<a name="command-structure"></a>

The AWS CLI uses a multipart structure on the command line\. It starts with the base call to `aws`\. The next part specifies a top\-level command, which often represents an AWS service supported in the AWS CLI\. Each AWS service has additional subcommands that specify the operation to perform\. The general CLI options, or the specific parameters for an operation, can be specified on the command line in any order\. If an exclusive parameter is specified multiple times, then only the *last value* applies\.

```
$ aws <command> <subcommand> [options and parameters]
```

Parameters can take various types of input values, such as numbers, strings, lists, maps, and JSON structures\.