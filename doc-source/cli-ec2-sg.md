# Using Security Groups<a name="cli-ec2-sg"></a>

You create a security group for use in either EC2\-Classic or EC2\-VPC\. For more information about EC2\-Classic and EC2\-VPC, see [Supported Platforms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\.

You can use the AWS CLI to create, add rules to, and delete your security groups\. 

**Note**  
Before you try the example commands, set your default credentials\. 


+ [Creating a Security Group](#creating-a-security-group)
+ [Adding Rules to Your Security Group](#configuring-a-security-group)
+ [Deleting Your Security Group](#deleting-a-security-group)

## Creating a Security Group<a name="creating-a-security-group"></a>

To create a security group named `my-sg`, use the [create\-security\-group](http://docs.aws.amazon.com/cli/latest/reference/ec2/create-security-group.html) command\.

### EC2\-VPC<a name="sg-ec2-vpc"></a>

The following command creates a security group named `my-sg` for the specified VPC:

```
$ aws ec2 create-security-group --group-name my-sg --description "My security group" --vpc-id vpc-1a2b3c4d
{
    "GroupId": "sg-903004f8"
}
```

To view the initial information for `my-sg`, use the [describe\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command as follows\. Note that you can't reference a security group for EC2\-VPC by name\.

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

The following command creates a security group for EC2\-Classic:

```
$ aws ec2 create-security-group --group-name my-sg --description "My security group"
{
    "GroupId": "sg-903004f8"
}
```

To view the initial information for `my-sg`, use the [describe\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command as follows:

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

## Adding Rules to Your Security Group<a name="configuring-a-security-group"></a>

If you're launching a Windows instance, you must add a rule to allow inbound traffic on TCP port 3389 \(RDP\)\. If you're launching a Linux instance, you must add a rule to allow inbound traffic on TCP port 22 \(SSH\)\. Use the [authorize\-security\-group\-ingress](http://docs.aws.amazon.com/cli/latest/reference/ec2/authorize-security-group-ingress.html) command to add a rule to your security group\. One of the required parameters of this command is the public IP address of your computer, in CIDR notation\.

**Note**  
You can get the public IP address of your local computer using a service\. For example, we provide the following service: [http://checkip\.amazonaws\.com/](http://checkip.amazonaws.com/)\. To locate another service that provides your IP address, use the search phrase "what is my IP address"\. If you are connecting through an ISP or from behind your firewall without a static IP address, you need to find out the range of IP addresses used by client computers\.

### EC2\-VPC<a name="rules-ec2-vpc"></a>

The following command adds a rule for RDP to the security group with the ID sg\-903004f8:

```
$ aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 3389 --cidr 203.0.113.0/24
```

The following command adds a rule for SSH to the security group with the ID sg\-903004f8:

```
$ aws ec2 authorize-security-group-ingress --group-id sg-903004f8 --protocol tcp --port 22 --cidr 203.0.113.0/24
```

To view the changes to `my-sg`, use the [describe\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command as follows:

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

The following command adds a rule for RDP to the security group `my-sg`:

```
$ aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 3389 --cidr 203.0.113.0/24
```

The following command adds a rule for SSH to the security group for `my-sg`:

```
$ aws ec2 authorize-security-group-ingress --group-name my-sg --protocol tcp --port 22 --cidr 203.0.113.0/24
```

To view the changes to `my-sg`, use the [describe\-security\-groups](http://docs.aws.amazon.com/cli/latest/reference/ec2/describe-security-groups.html) command as follows:

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

## Deleting Your Security Group<a name="deleting-a-security-group"></a>

To delete a security group, use the [delete\-security\-group](http://docs.aws.amazon.com/cli/latest/reference/ec2/delete-security-group.html) command\. Note that you can't delete a security group if it is attached to an environment\.

### EC2\-VPC<a name="delete-ec2-vpc"></a>

The following command deletes the security group with the ID sg\-903004f8:

```
$ aws ec2 delete-security-group --group-id sg-903004f8
```

### EC2\-Classic<a name="delete-ec2-classic"></a>

The following command deletes the security group named `my-sg`:

```
$ aws ec2 delete-security-group --group-name my-sg
```