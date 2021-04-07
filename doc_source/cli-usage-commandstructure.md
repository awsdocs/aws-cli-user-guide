# Command structure in the AWS CLI<a name="cli-usage-commandstructure"></a>

This topic covers how AWS Command Line Interface \(AWS CLI\) command is structured, and how to use wait commands\.

## Command structure<a name="cli-usage-commandstructure-structure.title"></a>

The AWS CLI uses a multipart structure on the command line that must be specified in this order:

1. The base call to the `aws` program\.

1. The top\-level *command*, which typically corresponds to an AWS service supported by the AWS CLI\.

1. The *subcommand* that specifies which operation to perform\.

1. General CLI options or parameters required by the operation\. You can specify these in any order as long as they follow the first three parts\. If an exclusive parameter is specified multiple times, only the *last value* applies\.

```
$ aws <command> <subcommand> [options and parameters]
```

Parameters can take various types of input values, such as numbers, strings, lists, maps, and JSON structures\. What is supported is dependent upon the command and subcommand you specify\.

## Wait commands<a name="cli-usage-commandstructure-wait"></a>

Some AWS services have `wait` commands available\. Any command that uses `aws wait` usually waits until a command is complete before it moves on to the next step\. This is especially useful for multipart commands or scripting, as you can use a wait command to to prevent moving to subsequent steps if the wait command fails\.

The AWS CLI uses a multipart structure on the command line for the `wait` command that must be specified in this order:

1. The base call to the `aws` program\.

1. The top\-level *command*, which typically corresponds to an AWS service supported by the AWS CLI\.

1. The `wait` command\.

1. The *subcommand* that specifies which operation to perform\.

1. General CLI options or parameters required by the operation\. You can specify these in any order as long as they follow the first three parts\. If an exclusive parameter is specified multiple times, only the *last value* applies\.

```
$ aws <command> wait <subcommand> [options and parameters]
```

Parameters can take various types of input values, such as numbers, strings, lists, maps, and JSON structures\. What is supported is dependent upon the command and subcommand you specify\.

**Note**  
Not every AWS service supports `wait` commands\. See the [AWS CLI reference guide](https://docs.aws.amazon.com/cli/latest/reference/deploy/wait/index.html) to see if your service supports `wait` commands\.

### Wait examples<a name="cli-usage-commandstructure-wait-example"></a>

**AWS CloudFormation**

The following [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudformation/wait/change-set-create-complete.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudformation/wait/change-set-create-complete.html) command examples pauses and resumes only after it can confirm that the *my\-change\-set* change set in the *my\-stack* stack is ready to run\.

```
$ aws cloudformation wait change-set-create-complete --stack-name my-stack --change-set-name my-change-set
```

For more information on the AWS CloudFormation `wait` commands, see [wait](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/wait/index.html) in the *AWS CLI Command Reference*\.

**AWS CodeDeploy**

The following [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/deploy/wait/deployment-successful.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/deploy/wait/deployment-successful.html) command examples pauses until the *d\-A1B2C3111* deployment completes successfully\.

```
$ aws deploy wait deployment-successful --deployment-id d-A1B2C3111
```

For more information on the AWS CodeDeploy `wait` commands, see [wait](https://docs.aws.amazon.com/cli/latest/reference/deploy/wait/index.html) in the *AWS CLI Command Reference*\.