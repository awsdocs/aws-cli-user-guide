# Command structure in the AWS CLI<a name="cli-usage-commandstructure"></a>

The AWS Command Line Interface \(AWS CLI\) uses a multipart structure on the command line that must be specified in this order:

1. The base call to the `aws` program\.

1. The top\-level *command*, which typically corresponds to an AWS service supported by the AWS CLI\.

1. The *subcommand* that specifies which operation to perform\. 

1. General CLI options or parameters required by the operation\. You can specify these in any order as long as they follow the first three parts\. If an exclusive parameter is specified multiple times, only the *last value* applies\.

```
$ aws <command> <subcommand> [options and parameters]
```

Parameters can take various types of input values, such as numbers, strings, lists, maps, and JSON structures\. What is supported is dependent upon the command and subcommand you specify\.