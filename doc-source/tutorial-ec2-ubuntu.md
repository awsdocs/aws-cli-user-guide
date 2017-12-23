# Deploying a Development Environment in Amazon EC2 Using the AWS Command Line Interface<a name="tutorial-ec2-ubuntu"></a>

This tutorial details how to set up a development environment in Amazon EC2 using the AWS CLI\. It includes a short version of the installation and configuration instructions, and it can be run start to finish on Windows, Linux, macOS, or Unix\.


+ [Install the AWS CLI](#install-cli)
+ [Configure the AWS CLI](#configure-cli)
+ [Create a Security Group, Key Pair, and Role for the EC2 Instance](#configure-security)
+ [Launch and Connect to the Instance](#launch-and-connect)

## Install the AWS CLI<a name="install-cli"></a>

You can install the AWS CLI with an installer \(Windows\) or by using `pip`, a package manager for Python\.

### Windows<a name="install-cli-windows"></a>

1. Download the MSI installer\.

   + [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI64.msi)

   + [Download the AWS CLI MSI installer for Windows \(32\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI32.msi)

1. Run the downloaded MSI installer\.

1. Follow the instructions that appear\.

### Linux, macOS, or Unix<a name="install-cli-unix"></a>

These steps require that you have a working installation of Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+\. If you encounter any issues using the following steps, see the full installation instructions in the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\. 

1. Download and run the installation script from the [pip website](https://pip.pypa.io/en/latest/installing.html):

   ```
   $ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
   $ python get-pip.py --user
   ```

1. Install the AWS CLI Using `pip`:

   ```
   $ pip install awscli --user
   ```

## Configure the AWS CLI<a name="configure-cli"></a>

Run `aws configure` at the command line to set up your credentials and settings\.

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

The AWS CLI will prompt you for the following information:

+ **AWS Access Key ID and AWS Secret Access Key** – These are your account credentials\. If you don't have keys, see [How Do I Get Security Credentials?](http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *Amazon Web Services General Reference*\.

+ **Default region name** – This is the name of the region you want to make calls against by default\.
**Note**  
Use `us-west-2` for this tutorial \(the AMI we will use is specific to this region\)\. You can change the default region later by running `aws configure` again\. 

+ **Default output format** – This format can be either json, text, or table\. If you don't specify an output format, json will be used\.

Run a command to verify that your credentials are configured correctly and that you can connect to AWS\.

```
$ aws ec2 describe-regions --output table
----------------------------------------------------------
|                     DescribeRegions                    |
+--------------------------------------------------------+
||                        Regions                       ||
|+-----------------------------------+------------------+|
||             Endpoint              |   RegionName     ||
|+-----------------------------------+------------------+|
||  ec2.ap-south-1.amazonaws.com     |  ap-south-1      ||
||  ec2.eu-west-2.amazonaws.com      |  eu-west-2       ||
||  ec2.eu-west-1.amazonaws.com      |  eu-west-1       ||
||  ec2.ap-northeast-2.amazonaws.com |  ap-northeast-2  ||
||  ec2.ap-northeast-1.amazonaws.com |  ap-northeast-1  ||
||  ec2.sa-east-1.amazonaws.com      |  sa-east-1       ||
||  ec2.ca-central-1.amazonaws.com   |  ca-central-1    ||
||  ec2.ap-southeast-1.amazonaws.com |  ap-southeast-1  ||
||  ec2.ap-southeast-2.amazonaws.com |  ap-southeast-2  ||
||  ec2.eu-central-1.amazonaws.com   |  eu-central-1    ||
||  ec2.us-east-1.amazonaws.com      |  us-east-1       ||
||  ec2.us-east-2.amazonaws.com      |  us-east-2       ||
||  ec2.us-west-1.amazonaws.com      |  us-west-1       ||
||  ec2.us-west-2.amazonaws.com      |  us-west-2       ||
|+-----------------------------------+------------------+|
```

## Create a Security Group, Key Pair, and Role for the EC2 Instance<a name="configure-security"></a>

Your next step is to set up prerequisites for launching an EC2 instance that can be accessed using SSH\. For more information about Amazon EC2 features, go to the *[Amazon EC2 User Guide for Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)* 

**To create a security group, key pair, and role**

1. First, create a new security group and add a rule that allows incoming traffic over port 22 for SSH\. Note the security group ID for later use\.

   ```
   $ aws ec2 create-security-group --group-name devenv-sg --description "security group for development environment in EC2"
   {
       "GroupId": "sg-b018ced5"
   }
   $ aws ec2 authorize-security-group-ingress --group-name devenv-sg --protocol tcp --port 22 --cidr 0.0.0.0/0
   ```

1. Replace the CIDR range in the above with the one that you will connect from for more security\. You can use the `aws ec2 describe-security-groups` command to admire your handiwork\. 

1. Next, create a key pair, which allows you to connect to the instance\. 

   ```
   $ aws ec2 create-key-pair --key-name devenv-key --query 'KeyMaterial' --output text > devenv-key.pem
   ```

   This command saves the contents of the key to a file called `devenv-key.pem`\. 
**Windows**  
In the Windows Command Processor, enclose queries with double quotes instead of single quotes\. 

1. On Linux, you will also need to change the file mode so that only you have access to the key file\. 

   ```
   $ chmod 400 devenv-key.pem
   ```

## Launch and Connect to the Instance<a name="launch-and-connect"></a>

Finally, you are ready to launch an instance and connect to it\. 

**To launch and connect to the instance**

1. Run the following command, replacing the security group ID output in the previous step\. 

   ```
   $ aws ec2 run-instances --image-id ami-6e1a0117 --security-group-ids sg-b018ced5 --count 1 --instance-type t2.micro --key-name devenv-key --query 'Instances[0].InstanceId'
   "i-0787e4282810ef9cf"
   ```

   The image ID *ami\-6e1a0117* specifies the Amazon Machine Image \(AMI\) that Amazon EC2 uses to bootstrap the instance\. You can find image IDs for other regions and operating systems in the [Amazon EC2 Management Console](https://console.aws.amazon.com/ec2/v2/home) Launch Instance Wizard\.
**Note**  
T2 instance types require a VPC\. If you don't have a default VPC, you can specify a subnet in a custom VPC with the `--subnet-id` option\. If you don't have any VPCs, choose a different instance type such as `t1.micro`\.

1. The instance will take a few moments to launch\. Once the instance is up and running, the following command will retrieve the public IP address that you will use to connect to the instance\.

   ```
   $ aws ec2 describe-instances --instance-ids "i-0787e4282810ef9cf" --query 'Reservations[0].Instances[0].PublicIpAddress'
   "54.183.22.255"
   ```

1. To connect to the instance, use the public IP address and private key with your preferred terminal program\. On Linux, macOS, or Unix, you can do this from the command line with the following command: 

   ```
   $ ssh -i devenv-key.pem ubuntu@54.183.22.255
   ```

   If you get an error like *Permission denied \(publickey\)* when attempting to connect to your instance, check that the following are correct:

   + **Key** – The key specified with the `-i` option must be at the path indicated and must be the private key, not the public one\. Permissions on the key must be restricted to the owner\.

   + **User name** – The user name must match the user associated with the key pair on the instance\. For Ubuntu instances, this is `ubuntu`\. For Amazon Linux, it is `ec2-user`\.

   + **Instance** – The public IP address or DNS name of the instance\. Verify that the address is public and that port 22 is open to your local machine on the instance's security group\.

   You can also use the `-v` option to view additional information related to the error\.
**SSH on Windows**  
On Windows, you can use the PuTTY terminal application available [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/)\. Get `putty.exe` and `puttygen.exe` from the downloads page\.   
Use `puttygen.exe` to convert your private key to a `.ppk` file required by PuTTY\. Launch `putty.exe`, enter the public IP address of the instance in the **Host Name** field, and set the connection type to SSH\.   
In the **Category** panel, navigate to **Connection** > **SSH** > **Auth**, and click **Browse** to select your `.ppk` file, and then click **Open** to connect\. 

1. The terminal will prompt you to accept the server's public key\. Type `yes` and click **Enter** to complete the connection\. 

You've now configured a security group, created a key pair, launched an EC2 instance, and connected to it without ever leaving the command line\. 