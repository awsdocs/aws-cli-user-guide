# Creating, configuring, and deleting security groups for Amazon EC2<a name="cli-services-ec2-sg"></a>

You can create a security group for your Amazon Elastic Compute Cloud \(Amazon EC2\) instances that essentially operates as a firewall, with rules that determine what network traffic can enter and leave\. 

You can create security groups to use in a virtual private cloud \(VPC\), or in the EC2\-Classic shared flat network\. For more information about the differences between EC2\-Classic and EC2\-VPC, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Use the AWS Command Line Interface \(AWS CLI\) to create a security group, add rules to existing security groups, and delete security groups\. 

**Note**  
The following examples assume that you have already [configured your default credentials](cli-services-ec2-keypairs.md)\.

**Topics**
+ [Create a security group](#creating-a-security-group)
+ [Add rules to your security group](#configuring-a-security-group)
+ [Delete your security group](#deleting-a-security-group)

## Create a security group<a name="creating-a-security-group"></a>

You can create security groups associated with VPCs or for EC2\-Classic\.

### EC2\-VPC<a name="sg-ec2-vpc"></a>

The following example shows how to create a security group for a specified VPC\.

```
$ aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id vpc-1a2b3c4d
{
    "GroupId": "sg-903004f8"
}
```

To view the initial information for a security group, run the [describe\-security\-groups](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command\. You can reference an EC2\-VPC security group only by its `vpc-id`, not its name\.

```
$ aws ec2 describe-security-groups --group-ids sg-903004f8
{
    "SecurityGroups": [
        {
            "IpPermissionsEgress": [
                {
                    "IpProtocol": "-1",
                    "IpRanges": [
                        {
                            "CidrIp": "0.0.0.0/0"
                        }
                    ],
                    "UserIdGroupPairs": []
                }
            ],
            "Description": "My security group"
            "IpPermissions": [],
            "GroupName": "my-sg",
            "VpcId": "vpc-1a2b3c4d",
            "OwnerId": "123456789012",
            "GroupId": "sg-903004f8"
        }
    ]
}
```

### EC2\-Classic<a name="sg-ec2-classic"></a>

The following example shows how to create a security group for EC2\-Classic\.

```
$ aws ec2 create-security-group --group-name my-sg --description "My security group"
{
    "GroupId": "sg-903004f8"
}
```

To view the initial information for `my-sg`, run the [describe\-security\-groups](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command\. For an EC2\-Classic security group, you can reference it by its name\.

```
$ aws ec2 describe-security-groups --group-names my-sg
{
    "SecurityGroups": [
        {
            "IpPermissionsEgress": [],
            "Description": "My security group"
            "IpPermissions": [],
            "GroupName": "my-sg",
            "OwnerId": "123456789012",
            "GroupId": "sg-903004f8"
        }
    ]
}
```

## Add rules to your security group<a name="configuring-a-security-group"></a>

When you run an Amazon EC2 instance, you must enable rules in the security group to allow incoming network traffic for your means of connecting to the image\. 

For example, if you're launching a Windows instance, you typically add a rule to allow inbound traffic on TCP port 3389 to support Remote Desktop Protocol \(RDP\)\. If you're launching a Linux instance, you typically add a rule to allow inbound traffic on TCP port 22 to support SSH connections\. 

Use the `[authorize\-security\-group\-ingress](https://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-security-group-ingress.html)` command to add a rule to your security group\. A required parameter of this command is the public IP address of your computer, or the network \(in the form of an address range\) that your computer is attached to, in [CIDR](https://wikipedia.org/wiki/Classless_Inter-Domain_Routing) notation\.

**Note**  
We provide the following service, [https://checkip\.amazonaws\.com/](https://checkip.amazonaws.com/), to enable you to determine your public IP address\. To find other services that can help you identify your IP address, use your browser to search for "*what is my IP address*"\. If you connect through an ISP or from behind your firewall using a dynamic IP address \(through a NAT gateway from a private network\), your address can change periodically\. In that case, you must find out the range of IP addresses used by client computers\.

### EC2\-VPC<a name="rules-ec2-vpc"></a>

The following example shows how to add a rule for RDP \(TCP port 3389\) to an EC2\-VPC security group with the ID `sg-903004f8`\. This example assumes the client computer has an address somewhere in the CIDR range 203\.0\.113\.0/24\.

You can start by confirming that your public address shows as included in the CIDR range 203\.0\.113\.0/24\.

```
$ curl https://checkip.amazonaws.com
203.0.113.57
```

With that information confirmed, you can add the range to your security group by running the `[authorize\-security\-group\-ingress](https://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-security-group-ingress.html)` command\.

```
$ aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 3389 --cidr 203.0.113.0/24
```

The following command adds another rule to enable SSH to instances in the same security group\.

```
$ aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 22 --cidr 203.0.113.0/24
```

To view the changes to the security group, run the [describe\-security\-groups](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command\.

```
$ aws ec2 describe-security-groups --group-ids sg-903004f8
{
    "SecurityGroups": [
        {
            "IpPermissionsEgress": [
                {
                    "IpProtocol": "-1",
                    "IpRanges": [
                        {
                            "CidrIp": "0.0.0.0/0"
                        }
                    ],
                    "UserIdGroupPairs": []
                }
            ],
            "Description": "My security group"
            "IpPermissions": [
                {
                    "ToPort": 22,
                    "IpProtocol": "tcp",
                    "IpRanges": [
                        {
                            "CidrIp": "203.0.113.0/24"
                        }
                    ]
                    "UserIdGroupPairs": [],
                    "FromPort": 22
                }
            ],
            "GroupName": "my-sg",
            "OwnerId": "123456789012",
            "GroupId": "sg-903004f8"
        }
    ]
}
```

### EC2\-Classic<a name="rules-ec2-classic"></a>

The following command adds a rule for RDP to the EC2\-Classic security group named *`my-sg`*\.

```
$ aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 3389 --cidr 203.0.113.0/24
```

The following command adds another rule for SSH to the same security group\.

```
$ aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 22 --cidr 203.0.113.0/24
```

To view the changes to your security group, run the [describe\-security\-groups](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command\.

```
$ aws ec2 describe-security-groups --group-names my-sg
{
    "SecurityGroups": [
        {
            "IpPermissionsEgress": [],
            "Description": "My security group"
            "IpPermissions": [
                {
                    "ToPort": 22,
                    "IpProtocol": "tcp",
                    "IpRanges": [
                        {
                            "CidrIp": "203.0.113.0/24"
                        }
                    ]
                    "UserIdGroupPairs": [],
                    "FromPort": 22
                }
            ],
            "GroupName": "my-sg",
            "OwnerId": "123456789012",
            "GroupId": "sg-903004f8"
        }
    ]
}
```

## Delete your security group<a name="deleting-a-security-group"></a>

To delete a security group, run the [delete\-security\-group](https://docs.aws.amazon.com/cli/latest/reference/ec2/delete-security-group.html) command\. 

**Note**  
You can't delete a security group if it's currently attached to an environment\.

### EC2\-VPC<a name="delete-ec2-vpc"></a>

The following command deletes an EC2\-VPC security group\.

```
$ aws ec2 delete-security-group --group-id sg-903004f8
```

### EC2\-Classic<a name="delete-ec2-classic"></a>

The following command deletes the EC2\-Classic security group named `my-sg`\.

```
$ aws ec2 delete-security-group --group-name my-sg
```