# Using Amazon EC2 Instances<a name="cli-ec2-launch"></a>

You can use the AWS CLI to launch, list, and terminate instances\. You'll need a key pair and a security group; for information about creating these through the AWS CLI, see [Using Key Pairs](cli-ec2-keypairs.md) and [Using Security Groups](cli-ec2-sg.md)\. You'll also need to select an Amazon Machine Image \(AMI\) and note its AMI ID\. For more information, see [Finding a Suitable AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html) in the *Amazon EC2 User Guide for Linux Instances*\.

If you launch an instance that is not within the AWS Free Tier, you are billed after you launch the instance and charged for the time that the instance is running, even if it remains idle\.

**Note**  
Before you try the example command, set your default credentials\.

**Topics**
+ [Launching an Instance](#launching-instances)
+ [Adding a Block Device Mapping to Your Instance](#block-device-mapping)
+ [Adding a Name Tag to Your Instance](#tagging-instances)
+ [Connecting to Your Instance](#connecting-to-instances)
+ [Listing Your Instances](#listing-instances)
+ [Terminating Your Instance](#terminating-instances)

## Launching an Instance<a name="launching-instances"></a>

To launch a single Amazon EC2 instance using the AMI you selected, use the [run\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html) command\. Depending on the platforms that your account supports, you can launch the instance into EC2\-Classic or EC2\-VPC\.

Initially, your instance is in the `pending` state, but will be in the `running` state in a few minutes\.

### EC2\-VPC<a name="run-ec2-vpc"></a>

The following command launches a `t2.micro` instance in the specified subnet:

```
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids sg-xxxxxxxx --subnet-id subnet-xxxxxxxx
{
    "OwnerId": "123456789012",
    "ReservationId": "r-5875ca20",
    "Groups": [
        {
            "GroupName": "my-sg",
            "GroupId": "sg-903004f8"
        }
    ],
    "Instances": [
        {
            "Monitoring": {
                "State": "disabled"
            },
            "PublicDnsName": null,
            "Platform": "windows",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "EbsOptimized": false,
            "LaunchTime": "2013-07-19T02:42:39.000Z",
            "PrivateIpAddress": "10.0.1.114",
            "ProductCodes": [],
            "VpcId": "vpc-1a2b3c4d",
            "InstanceId": "i-5203422c",
            "ImageId": "ami-173d747e",
            "PrivateDnsName": ip-10-0-1-114.ec2.internal,
            "KeyName": "MyKeyPair",
            "SecurityGroups": [
                {
                    "GroupName": "my-sg",
                    "GroupId": "sg-903004f8"
                }
            ],
            "ClientToken": null,
            "SubnetId": "subnet-6e7f829e",
            "InstanceType": "t2.micro",
            "NetworkInterfaces": [
                {
                    "Status": "in-use",
                    "SourceDestCheck": true,
                    "VpcId": "vpc-1a2b3c4d",
                    "Description": "Primary network interface",
                    "NetworkInterfaceId": "eni-a7edb1c9",
                    "PrivateIpAddresses": [
                        {
                            "PrivateDnsName": "ip-10-0-1-114.ec2.internal",
                            "Primary": true,
                            "PrivateIpAddress": "10.0.1.114"
                        }
                    ],
                    "PrivateDnsName": "ip-10-0-1-114.ec2.internal",
                    "Attachment": {
                        "Status": "attached",
                        "DeviceIndex": 0,
                        "DeleteOnTermination": true,
                        "AttachmentId": "eni-attach-52193138",
                        "AttachTime": "2013-07-19T02:42:39.000Z"
                    },
                    "Groups": [
                        {
                            "GroupName": "my-sg",
                            "GroupId": "sg-903004f8"
                        }
                    ],
                    "SubnetId": "subnet-6e7f829e",
                    "OwnerId": "123456789012",
                    "PrivateIpAddress": "10.0.1.114"
                }              
            ],
            "SourceDestCheck": true,
            "Placement": {
                "Tenancy": "default",
                "GroupName": null,
                "AvailabilityZone": "us-west-2b"
            },
            "Hypervisor": "xen",
            "BlockDeviceMappings": [
                {
                    "DeviceName": "/dev/sda1",
                    "Ebs": {
                        "Status": "attached",
                        "DeleteOnTermination": true,
                        "VolumeId": "vol-877166c8",
                        "AttachTime": "2013-07-19T02:42:39.000Z"
                    }
                }              
            ],
            "Architecture": "x86_64",
            "StateReason": {
                "Message": "pending",
                "Code": "pending"
            },
            "RootDeviceName": "/dev/sda1",
            "VirtualizationType": "hvm",
            "RootDeviceType": "ebs",
            "Tags": [
                {
                    "Value": "MyInstance",
                    "Key": "Name"
                }
            ],
            "AmiLaunchIndex": 0
        }
    ]
}
```

### EC2\-Classic<a name="run-ec2-classic"></a>

The following command launches a `t1.micro` instance in EC2\-Classic:

```
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t1.micro --key-name MyKeyPair --security-groups my-sg
{
    "OwnerId": "123456789012",
    "ReservationId": "r-5875ca20",
    "Groups": [
        {
            "GroupName": "my-sg",
            "GroupId": "sg-903004f8"
        }
    ],
    "Instances": [
        {
            "Monitoring": {
                "State": "disabled"
            },
            "PublicDnsName": null,
            "Platform": "windows",
            "State": {
                "Code": 0,
                "Name": "pending"
            },
            "EbsOptimized": false,
            "LaunchTime": "2013-07-19T02:42:39.000Z",
            "ProductCodes": [],
            "InstanceId": "i-5203422c",
            "ImageId": "ami-173d747e",
            "PrivateDnsName": null,
            "KeyName": "MyKeyPair",
            "SecurityGroups": [
                {
                    "GroupName": "my-sg",
                    "GroupId": "sg-903004f8"
                }
            ],
            "ClientToken": null,
            "InstanceType": "t1.micro",
            "NetworkInterfaces": [],
            "Placement": {
                "Tenancy": "default",
                "GroupName": null,
                "AvailabilityZone": "us-west-2b"
            },
            "Hypervisor": "xen",
            "BlockDeviceMappings": [
                {
                    "DeviceName": "/dev/sda1",
                    "Ebs": {
                        "Status": "attached",
                        "DeleteOnTermination": true,
                        "VolumeId": "vol-877166c8",
                        "AttachTime": "2013-07-19T02:42:39.000Z"
                    }
                }              
            ],
            "Architecture": "x86_64",
            "StateReason": {
                "Message": "pending",
                "Code": "pending"
            },
            "RootDeviceName": "/dev/sda1",
            "VirtualizationType": "hvm",
            "RootDeviceType": "ebs",
            "Tags": [
                {
                    "Value": "MyInstance",
                    "Key": "Name"
                }
            ],
            "AmiLaunchIndex": 0
        }
    ]
}
```

## Adding a Block Device Mapping to Your Instance<a name="block-device-mapping"></a>

Each instance that you launch has an associated root device volume\. You can use block device mapping to specify additional EBS volumes or instance store volumes to attach to an instance when it's launched\.

To add a block device mapping to your instance, specify the `--block-device-mappings` option when you use `run-instances`\.

The following example adds a standard Amazon EBS volume, mapped to `/dev/sdf`, that's 20 GB in size\.

```
--block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"Ebs\":{\"VolumeSize\":20,\"DeleteOnTermination\":false}}]"
```

The following example adds an Amazon EBS volume, mapped to `/dev/sdf`, based on a snapshot\. When you specify a snapshot, it isn't necessary to specify a volume size, but if you do, it must be greater than or equal to the size of the snapshot\.

```
--block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"Ebs\":{\"SnapshotId\":\"snap-xxxxxxxx\"}}]"
```

The following example adds two instance store volumes\. Note that the number of instance store volumes available to your instance depends on its instance type\.

```
--block-device-mappings "[{\"DeviceName\":\"/dev/sdf\",\"VirtualName\":\"ephemeral0\"},{\"DeviceName\":\"/dev/sdg\",\"VirtualName\":\"ephemeral1\"}]"
```

The following example omits a mapping for a device specified by the AMI used to launch the instance \(`/dev/sdj`\):

```
--block-device-mappings "[{\"DeviceName\":\"/dev/sdj\",\"NoDevice\":\"\"}]"
```

For more information, see [Block Device Mapping](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/block-device-mapping-concepts.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Adding a Name Tag to Your Instance<a name="tagging-instances"></a>

To add the tag `Name=MyInstance` to your instance, use the [create\-tags](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html) command as follows:

```
aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Name,Value=MyInstance
```

For more information, see [Tagging Your Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Connecting to Your Instance<a name="connecting-to-instances"></a>

While your instance is running, you can connect to it and use it just as you'd use a computer sitting in front of you\. For more information, see [Connect to Your Amazon EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Listing Your Instances<a name="listing-instances"></a>

You can use the AWS CLI to list your instances and view information about them\. You can list all your instances, or filter the results based on the instances that you're interested in\.

**Note**  
Before you try the example commands, set your default credentials\.

The following examples show how to use the [describe\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html) command\.

**Example 1: List the instances with the specified instance type**  
The following command lists your `t2.micro` instances\.  

```
aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query Reservations[].Instances[].InstanceId
```

**Example 2: List the instances with the specified tag**  
The following command lists the instances with a tag Name=MyInstance\.  

```
aws ec2 describe-instances --filters "Name=tag:Name,Values=MyInstance"
```

**Example 3: List the instances launched using the specified images**  
The following command lists your instances that were launched from the following AMIs: `ami-x0123456`, `ami-y0123456`, and `ami-z0123456`\.  

```
aws ec2 describe-instances --filters "Name=image-id,Values=ami-x0123456,ami-y0123456,ami-z0123456"
```

## Terminating Your Instance<a name="terminating-instances"></a>

Terminating an instance effectively deletes it; you can't reconnect to an instance after you've terminated it\. As soon as the state of the instance changes to `shutting-down` or `terminated`, you stop incurring charges for that instance\.

When you are finished with the instance, use the [terminate\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/terminate-instances.html) command as follows:

```
aws ec2 terminate-instances --instance-ids i-5203422c
{
    "TerminatingInstances": [
        {
            "InstanceId": "i-5203422c",
            "CurrentState": {
                "Code": 32,
                "Name": "shutting-down"
            },
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}
```

For more information, see [Terminate Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\.