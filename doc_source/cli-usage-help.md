# Getting help with the AWS CLI<a name="cli-usage-help"></a>

You can get help with any command when using the AWS Command Line Interface \(AWS CLI\)\. To do so, simply type `help` at the end of a command name\. 

For example, the following command displays help for the general AWS CLI options and the available top\-level commands\. 

```
$ aws help
```

The following command displays the available Amazon Elastic Compute Cloud \(Amazon EC2\) specific commands\. 

```
$ aws ec2 help
```

The following example displays detailed help for the Amazon EC2 `DescribeInstances` operation\. The help includes descriptions of its input parameters, available filters, and what is included as output\. It also includes examples showing how to type common variations of the command\.

```
$ aws ec2 describe-instances help
```

The help for each command is divided into six sections:

Name  
The name of the command\.  

```
NAME
       describe-instances -
```

Description  
A description of the API operation that the command invokes\.  

```
DESCRIPTION
       Describes one or more of your instances.

       If you specify one or more instance IDs, Amazon EC2 returns information
       for those instances. If you do not specify  instance  IDs,  Amazon  EC2
       returns  information  for  all  relevant  instances.  If you specify an
       instance ID that is not valid, an error is returned. If you specify  an
       instance  that  you  do  not  own,  it  is not included in the returned
       results.
...
```

Synopsis  
The basic syntax for using the command and its options\. If an option is shown in square brackets, it's optional, has a default value, or has an alternative option that you can use\.  

```
SYNOPSIS
            describe-instances
          [--dry-run | --no-dry-run]
          [--instance-ids <value>]
          [--filters <value>]
          [--cli-input-json <value>]
          [--starting-token <value>]
          [--page-size <value>]
          [--max-items <value>]
          [--generate-cli-skeleton]
```
For example, `describe-instances` has a default behavior that describes ***all*** instances in the current account and AWS Region\. You can optionally specify a list of `instance-ids` to describe one or more instances; `dry-run` is an optional Boolean flag that doesn't take a value\. To use a Boolean flag, specify either shown value, in this case `--dry-run` or `--no-dry-run`\. Likewise, `--generate-cli-skeleton` doesn't take a value\. If there are conditions on an option's use, they are described in the `OPTIONS` section, or shown in the examples\.

Options  
A description of each of the options shown in the synopsis\.  

```
OPTIONS
       --dry-run | --no-dry-run (boolean)
          Checks whether you have the required  permissions  for  the  action,
          without actually making the request, and provides an error response.
          If you have the required permissions, the error response is  DryRun-
          Operation . Otherwise, it is UnauthorizedOperation .

       --instance-ids (list)
          One or more instance IDs.

          Default: Describes all your instances.
...
```

Examples  
Examples showing the usage of the command and its options\. If no example is available for a command or use case that you need, request one using the feedback link on this page, or in the AWS CLI command reference on the help page for the command\.  

```
    EXAMPLES
    To describe an Amazon EC2 instance

    Command:
    
    aws ec2 describe-instances --instance-ids i-5203422c
    
    To describe all instances with the instance type m1.small
    
    Command:
    
    aws ec2 describe-instances --filters "Name=instance-type,Values=m1.small"
    
    To describe all instances with an Owner tag
    
    Command:
    
    aws ec2 describe-instances --filters "Name=tag-key,Values=Owner"
...
```

Output  
Descriptions of each of the fields and data types included in the response from AWS\.  
For `describe-instances`, the output is a list of reservation objects, each of which contains several fields and objects that contain information about the instances associated with it\. This information comes from the [API documentation for the reservation data type](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_Reservation.html) used by Amazon EC2\.  

```
OUTPUT
       Reservations -> (list)
          One or more reservations.

          (structure)
              Describes a reservation.

              ReservationId -> (string)
                 The ID of the reservation.

              OwnerId -> (string)
                 The ID of the AWS account that owns the reservation.

              RequesterId -> (string)
                 The ID of the requester that launched the instances  on  your
                 behalf (for example, AWS Management Console or Auto Scaling).

              Groups -> (list)
                 One or more security groups.

                 (structure)
                     Describes a security group.

                     GroupName -> (string)
                        The name of the security group.

                     GroupId -> (string)
                        The ID of the security group.

              Instances -> (list)
                 One or more instances.

                 (structure)
                     Describes an instance.

                     InstanceId -> (string)
                        The ID of the instance.

                     ImageId -> (string)
                        The ID of the AMI used to launch the instance.

                     State -> (structure)
                        The current state of the instance.

                        Code -> (integer)
                            The  low  byte represents the state. The high byte
                            is an opaque internal value and should be ignored.
...
```
When the AWS CLI renders the output into JSON, it becomes an array of reservation objects, similar to the following example\.  

```
{
    "Reservations": [
        {
            "OwnerId": "012345678901",
            "ReservationId": "r-4c58f8a0",
            "Groups": [],
            "RequesterId": "012345678901",
            "Instances": [
                {
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "PublicDnsName": "ec2-52-74-16-12.us-west-2.compute.amazonaws.com",
                    "State": {
                        "Code": 16,
                        "Name": "running"
                    },
...
```
Each reservation object contains fields describing the reservation and an array of instance objects, each with its own fields \(for example, `PublicDnsName`\) and objects \(for example, `State`\) that describe it\.  
**Windows users**  
You can *pipe* \(\|\) the output of the help command to the `more` command to view the help file one page at a time\. Press the space bar or **PgDn** to view more of the document, and **q** to quit\.   

```
C:\> aws ec2 describe-instances help | more
```

## AWS CLI documentation<a name="cli-reference"></a>

The [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/) also contains the help content for all AWS CLI commands\. The descriptions are presented for easy navigation and viewing on mobile, tablet, or desktop screens\. 

**Note**  
The help files contain links that cannot be viewed or navigated to from the command line\. You can view and interact with these links by using the online [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\. 

## API documentation<a name="api-reference"></a>

All commands in the AWS CLI correspond to requests made to an AWS service's public API\. Each service with a public API has an API reference that can be found on the service's home page on the [AWS Documentation website](http://aws.amazon.com/documentation/)\. The content for an API reference varies based on how the API is constructed and which protocol is used\. Typically, an API reference contains detailed information about the operations supported by the API, the data sent to and from the service, and any error conditions that the service can report\. 

**API Documentation Sections**
+  **Actions** – Detailed information on each operation and its parameters \(including constraints on length or content, and default values\)\. It lists the errors that can occur for this operation\. Each operation corresponds to a subcommand in the AWS CLI\. 
+  **Data Types** – Detailed information about structures that a command might require as a parameter, or return in response to a request\.
+  **Common Parameters** – Detailed information about the parameters that are shared by all of action for the service\. 
+  **Common Errors** – Detailed information about errors that can be returned by any of the service's operations\. 

The name and availability of each section can vary, depending on the service\. 

**Service\-specific CLIs**  
Some services have a separate CLI that dates from before a single AWS CLI was created to work with all services\. These service\-specific CLIs have separate documentation that is linked from the service's documentation page\. Documentation for service\-specific CLIs does not apply to the AWS CLI\. 