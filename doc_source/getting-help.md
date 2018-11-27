# Getting Help with the AWS Command Line Interface<a name="getting-help"></a>

To get help when using the AWS CLI, you can simply add `help` to the end of a command\. For example, the following command lists help for the general AWS CLI options and the available top\-level commands\. 

```
$ aws help
```

The following command lists the available subcommands for Amazon EC2\. 

```
$ aws ec2 help
```

The next example lists the detailed help for the EC2 `DescribeInstances` operation, including descriptions of its input parameters, filters, and output\. Check the examples section of the help if you are not sure how to phrase a command\. 

```
$ aws ec2 describe-instances help
```

The help for each command is divided into six sections:

**Name** – the name of the command\.

```
NAME
       describe-instances -
```

**Description** – a description of the API operation that the command invokes, pulled from the API documentation for the command's service\.

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

**Synopsis** – list of the command and its options\. If an option is shown in square brackets, it is either optional, has a default value, or has an alternative option that can be used instead\.

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

`describe-instances` has a default behavior that describes all instances in the current account and region\. You can optionally specify a list of `instance-ids` to describe one or more instances\. `dry-run` is an optional boolean flag that doesn't take a value\. To use a boolean flag, specify either shown value, in this case `--dry-run` or `--no-dry-run`\. Likewise, `--generate-cli-skeleton` does not take a value\. If there are conditions on an option's use, they should be described in the `OPTIONS` section, or shown in the examples\.

**Options** – description of each of the options shown in the synopsis\.

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

**Examples** – examples showing the usage of the command and its options\. If no example is available for a command or use case that you need, please request one using the feedback link on this page, or in the AWS CLI command reference on the help page for the command\.

```
    EXAMPLES
    To describe an Amazon EC2 instance

    Command:
    
    aws ec2 describe-instances --instance-ids i-5203422c
    
    To describe all instances with the instance type m1.small
    
    Command:
    
    aws ec2 describe-instances --filters "Name=instance-type,Values=m1.small"
    
    To describe all instances with a Owner tag
    
    Command:
    
    aws ec2 describe-instances --filters "Name=tag-key,Values=Owner"
...
```

**Output** – descriptions of each of the fields and datatypes returned in the response from AWS\.

For `describe-instances`, the output is a list of reservation objects, each of which contains several fields and objects that contain information about the instance\(s\) associated with it\. This information comes from the [API documentation for the reservation datatype](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_Reservation.html) used by Amazon EC2\.

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

When the output is rendered into JSON by the AWS CLI, it becomes an array of reservation objects, like this:

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

Each reservation object contains fields describing the reservation and an array of instance objects, each with its own fields \(e\.g\. `PublicDnsName`\) and objects \(e\.g\. `State`\) that describe it\.

**Windows Users**  
Pipe the output of the help command to `more` to view the help file one page at a time\. Press the space bar or Page Down to view more of the document, and q to quit\.   

```
> aws ec2 describe-instances help | more
```

## AWS CLI Documentation<a name="cli-reference"></a>

The [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/) provides the content of all AWS CLI commands' help files, compiled and presented online for easy navigation and viewing on mobile, tablet, and desktop screens\. 

Help files sometimes contain links that cannot be viewed or followed from the command line view; these are preserved in the online AWS CLI reference\. 

## API Documentation<a name="api-reference"></a>

All subcommands in the AWS CLI correspond to calls made against a service's public API\. Each service with a public API, in turn, has a set of API reference documentation that can be found from the service's homepage on the [AWS Documentation website](http://aws.amazon.com/documentation/)\. 

The content of an API reference varies based on how the API is constructed and which protocol is used\. Typically, an API reference will contain detailed information on actions supported by the API, data sent to and from the service, and possible error conditions\. 

**API Documentation Sections**
+  **Actions** – Detailed information on parameters \(including constraints on length or content\) and errors specific to an action\. Actions correspond to subcommands in the AWS CLI\. 
+  **Data Types** – May contain additional information about object data returned by a subcommand\. 
+  **Common Parameters** – Detailed information about parameters that are used by all of a service's actions\. 
+  **Common Errors** – Detailed information about errors returned by all of a service's actions\. 

The name and availability of each section may vary depending on the service\. 

**Service\-Specific CLIs**  
Some services have a separate CLI from before a single AWS CLI was created that works with all services\. These service\-specific CLIs have separate documentation that is linked from the service's documentation page\. Documentation for service\-specific CLIs does not apply to the AWS CLI\. 