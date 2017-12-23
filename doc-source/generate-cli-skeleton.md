# Generate CLI Skeleton and CLI Input JSON Parameters<a name="generate-cli-skeleton"></a>

Most AWS CLI commands support `--generate-cli-skeleton` and `--cli-input-json` parameters that you can use to store parameters in JSON and read them from a file instead of typing them at the command line\. 

Generate CLI Skeleton outputs JSON that outlines all of the parameters that can be specified for the operation\. 

**To use \-\-generate\-cli\-skeleton with aws ec2 run\-instances**

1. Execute the run\-instances command with the `--generate-cli-skeleton` option to view the JSON skeleton\. 

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

1. Direct the output to a file to save the skeleton locally:

   ```
   $ aws ec2 run-instances --generate-cli-skeleton > ec2runinst.json
   ```

1. Open the skeleton in a text editor and remove any parameters that you will not use:

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

   Leave the `DryRun` parameter set to true to use EC2's dry run feature, which lets you test your configuration without creating resources\. 

1. Fill in the values for the instance type, key name, security group and AMI in your default region\. In this example, `ami-dfc39aef` is a 64\-bit [Amazon Linux](http://aws.amazon.com/amazon-linux-ami/) image in the `us-west-2` region\. 

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

1. Pass the JSON configuration to the `--cli-input-json` parameter using the `file://` prefix: 

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json
   A client error (DryRunOperation) occurred when calling the RunInstances operation: Request would have succeeded, but DryRun flag is set.
   ```

   The dry run error indicates that the JSON is formed correctly and the parameter values are valid\. If any other issues are reported in the output, fix them and repeat the above step until the dry run error is shown\. 

1. Set the DryRun parameter to false to disable the dry run feature\.

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

1. Run the `run-instances` command again to launch an instance:

   ```
   $ aws ec2 run-instances --cli-input-json file://ec2runinst.json
   {
       "OwnerId": "123456789012",
       "ReservationId": "r-d94a2b1",
       "Groups": [],
       "Instances": [
   ...
   ```