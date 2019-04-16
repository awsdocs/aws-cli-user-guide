# Generate the CLI Skeleton and Input Parameters from a JSON Input File<a name="cli-usage-skeleton"></a>

Most of the AWS Command Line Interface \(AWS CLI\) commands support the ability to accept all of the parameter input from a file using the `--cli-input-json` parameter\.

Those same commands helpfully provide the `--generate-cli-skeleton` to generate a file with all of the parameters that you can edit and fill in\. Then you can run the command with the `--cli-input-json` parameter and point to the filled\-in file\.

**Important**  
There are several AWS CLI commands that don't map directly to individual AWS API operations, such as the [`aws s3` commands](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)\. Such commands don't support either the `--generate-cli-skeleton` or `--cli-input-json` parameters that are discussed on this page\. If you have any question about whether a specific command supports these parameters, run the following command, replacing the *service* and *command* names with the ones you're interested in:  

```
$ aws service command help
```
The output includes a `Synopsis` section that shows the parameters that the specified command supports\.

The `--generate-cli-skeleton` parameter causes the command not to run, but instead to generate and display a parameter template that you can customize and then use as input on a later command\. The generated template includes all of the parameters supported by the command\.

For example, if you run the following command, it generates the parameter template for the Amazon Elastic Compute Cloud \(Amazon EC2\) command run\-instances\.

```
$ aws ec2 run-instances --generate-cli-skeleton
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

**To generate and use a parameter skeleton file**

1. Run the command with the `--generate-cli-skeleton` parameter and direct the output to a file to save it\.

   ```
   $ aws ec2 run-instances --generate-cli-skeleton > ec2runinst.json
   ```

1. Open the parameter skeleton file in your text editor and remove any of the parameters that you don't need\. For example, you might strip it down to the following\.

   ```
    1. {
    2.     "DryRun": true,
    3.     "ImageId": "",
    4.     "KeyName": "",
    5.     "SecurityGroups": [
    6.         ""
    7.     ],
    8.     "InstanceType": "",
    9.     "Monitoring": {
   10.         "Enabled": true
   11.     }
   12. }
   ```

   In this example, we leave the `DryRun` parameter set to true to use EC2's dry run feature, which lets you safely test the command without actually creating or modifying any resources\. 

1. Fill in the remaining values with values appropriate for your scenario\. In this example, we provide the instance type, key name, security group and identifier of the AMI to use\. This example assumes the default region\. The AMI `ami-dfc39aef` is a 64\-bit Amazon Linux image hosted in the `us-west-2` region\. If you use a different region, you must [find the correct AMI ID to use](http://aws.amazon.com/amazon-linux-ami/)\.

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

1. Run the command with the completed parameters by passing the JSON file to the `--cli-input-json` parameter using the `file://` prefix\. The AWS CLI interprets the path to be relative to your current working directory, so the following example which displays only the file name with no path is looked for the file directly in the current working directory\.

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json
   A client error (DryRunOperation) occurred when calling the RunInstances operation: Request would have succeeded, but DryRun flag is set.
   ```

   The dry run error indicates that the JSON is formed correctly and the parameter values are valid\. If any other issues are reported in the output, fix them and repeat the above step until the "Request would have succeeded" message is displayed\. 

1. Now you can set the DryRun parameter to false to disable dry run\.

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

1. Now when you run the command, `run-instances` actually launches an EC2 instance and displays the details generated by the successful launch\.

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json
   {
       "OwnerId": "123456789012",
       "ReservationId": "r-d94a2b1",
       "Groups": [],
       "Instances": [
   ...
   ```