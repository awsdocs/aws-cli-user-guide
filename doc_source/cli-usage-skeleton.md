--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# AWS CLI skeletons and input files<a name="cli-usage-skeleton"></a>

Most of the AWS Command Line Interface \(AWS CLI\) commands accept all parameter inputs from a file\. These templates can be generated using the `generate-cli-skeleton` option\.

**Topics**
+ [About AWS CLI skeletons and input files](#cli-usage-skeleton-about)
+ [Generating a command skeleton](#cli-usage-skeleton-generate)

## About AWS CLI skeletons and input files<a name="cli-usage-skeleton-about"></a>

Most of the AWS Command Line Interface \(AWS CLI\) commands support the ability to accept all parameter inputs from a file using the `--cli-input-json` parameters\.

Those same commands helpfully provide the `--generate-cli-skeleton` parameter to generate a file in JSON format with all of the parameters that you can edit and fill in\. Then you can run the command with the relevant `--cli-input-json` parameter and point to the filled\-in file\.

**Important**  
Several AWS CLI commands don't map directly to individual AWS API operations, such as the [`aws s3` commands](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)\. Such commands don't support either the `--generate-cli-skeleton` or `--cli-input-json` parameters described in this topic\. If you don't know whether a specific command supports these parameters, run the following command, replacing the *service* and *command* names with the ones you're interested in\.  

```
$ aws service command help
```
The output includes a `Synopsis` section that shows the parameters that the specified command supports\.  

```
$ aws iam list-users help
...
SYNOPSIS
          list-users
          ...
          [--cli-input-json]
          ...
          [--generate-cli-skeleton <value>]
...
```

The `--generate-cli-skeleton` parameter causes the command not to run, but instead to generate and display a parameter template that you can customize and use as input on a later command\. The generated template includes all of the parameters that the command supports\.

The `--generate-cli-skeleton` parameter accepts one of the following values:
+ `input` – The generated template includes all input parameters formatted as JSON\. This is the default value\.
+ `output` – The generated template includes all output parameters formatted as JSON\.

Because the AWS CLI is essentially a "wrapper" around the service's API, the skeleton file expects you to reference all parameters by their underlying API parameter names\. This is likely different from the AWS CLI parameter name\. For example, an AWS CLI parameter named `user-name` might map to the AWS service's API parameter named `UserName` \(notice the altered capitalization and missing dash\)\. We recommend that you use the `--generate-cli-skeleton` option to generate the template with the "correct" parameter names to avoid errors\. You can also reference the API Reference Guide for the service to see the expected parameter names\. You can delete any parameters from the template that are not required and for which you don't want to supply a value\.

For example, if you run the following command, it generates the parameter template for the Amazon Elastic Compute Cloud \(Amazon EC2\) command run\-instances\.

------
#### [ JSON ]

The following example shows how to generate a template formatted in JSON by using the default value \(`input`\) for the `--generate-cli-skeleton` parameter\.

```
$ aws ec2 run-instances --generate-cli-skeleton
```

```
{
    "DryRun": true,
    "ImageId": "",
    "MinCount": 0,
    "MaxCount": 0,
    "KeyName": "",
    "SecurityGroups": [
        ""
    ],
    "SecurityGroupIds": [
        ""
    ],
    "UserData": "",
    "InstanceType": "",
    "Placement": {
        "AvailabilityZone": "",
        "GroupName": "",
        "Tenancy": ""
    },
    "KernelId": "",
    "RamdiskId": "",
    "BlockDeviceMappings": [
        {
            "VirtualName": "",
            "DeviceName": "",
            "Ebs": {
                "SnapshotId": "",
                "VolumeSize": 0,
                "DeleteOnTermination": true,
                "VolumeType": "",
                "Iops": 0,
                "Encrypted": true
            },
            "NoDevice": ""
        }
    ],
    "Monitoring": {
        "Enabled": true
    },
    "SubnetId": "",
    "DisableApiTermination": true,
    "InstanceInitiatedShutdownBehavior": "",
    "PrivateIpAddress": "",
    "ClientToken": "",
    "AdditionalInfo": "",
    "NetworkInterfaces": [
        {
            "NetworkInterfaceId": "",
            "DeviceIndex": 0,
            "SubnetId": "",
            "Description": "",
            "PrivateIpAddress": "",
            "Groups": [
                ""
            ],
            "DeleteOnTermination": true,
            "PrivateIpAddresses": [
                {
                    "PrivateIpAddress": "",
                    "Primary": true
                }
            ],
            "SecondaryPrivateIpAddressCount": 0,
            "AssociatePublicIpAddress": true
        }
    ],
    "IamInstanceProfile": {
        "Arn": "",
        "Name": ""
    },
    "EbsOptimized": true
}
```

------

## Generating a command skeleton<a name="cli-usage-skeleton-generate"></a>

**To generate and use a parameter skeleton file**

1. Run the command with the `--generate-cli-skeleton` parameter to produce JSON and direct the output to a file to save it\.

------
#### [ JSON ]

   ```
   $ aws ec2 run-instances --generate-cli-skeleton input > ec2runinst.json
   ```

------

1. Open the parameter skeleton file in your text editor and remove any of the parameters that you don't need\. For example, you might strip the template down to the following\. Be sure that the file is still valid JSON after you remove the elements you don't need\.

------
#### [ JSON ]

   ```
   {
       "DryRun": true,
       "ImageId": "",
       "KeyName": "",
       "SecurityGroups": [
           ""
       ],
       "InstanceType": "",
       "Monitoring": {
           "Enabled": true
       }
   }
   ```

------

   In this example, we leave the `DryRun` parameter set to `true` to use the Amazon EC2 dry run feature\. This feature lets you safely test the command without actually creating or modifying any resources\. 

1. Fill in the remaining values with values appropriate for your scenario\. In this example, we provide the instance type, key name, security group, and identifier of the Amazon Machine Image \(AMI\) to use\. This example assumes the default AWS Region\. The AMI `ami-dfc39aef` is a 64\-bit Amazon Linux image hosted in the `us-west-2` Region\. If you use a different Region, you must [find the correct AMI ID to use](http://aws.amazon.com/amazon-linux-ami/)\.

------
#### [ JSON ]

   ```
   {
       "DryRun": true,
       "ImageId": "ami-dfc39aef",
       "KeyName": "mykey",
       "SecurityGroups": [
           "my-sg"
       ],
       "InstanceType": "t2.micro",
       "Monitoring": {
           "Enabled": true
       }
   }
   ```

------

1. Run the command with the completed parameters by passing the completed template file to the `--cli-input-json` parameter by using the `file://` prefix\. The AWS CLI interprets the path to be relative to your current working directory, so in the following example that displays only the file name with no path, it looks for the file directly in the current working directory\.

------
#### [ JSON ]

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json
   ```

   ```
   A client error (DryRunOperation) occurred when calling the RunInstances operation: Request would have succeeded, but DryRun flag is set.
   ```

------

   The dry run error indicates that the JSON is formed correctly and that the parameter values are valid\. If other issues are reported in the output, fix them and repeat the previous step until the "Request would have succeeded" message is displayed\. 

1. Now you can set the DryRun parameter to false to disable dry run\.

------
#### [ JSON ]

   ```
   {
       "DryRun": false,
       "ImageId": "ami-dfc39aef",
       "KeyName": "mykey",
       "SecurityGroups": [
           "my-sg"
       ],
       "InstanceType": "t2.micro",
       "Monitoring": {
           "Enabled": true
       }
   }
   ```

------

1. Run the command, and `run-instances` actually launches an Amazon EC2 instance and displays the details generated by the successful launch\. The format of the output is controlled by the `--output` parameter, separately from the format of your input parameter template\.

------
#### [ JSON ]

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json --output json
   ```

   ```
   {
       "OwnerId": "123456789012",
       "ReservationId": "r-d94a2b1",
       "Groups": [],
       "Instances": [
   ...
   ```

------