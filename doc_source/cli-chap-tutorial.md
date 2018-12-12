# Tutorial: Using the AWS Command Line Interface to Deploy an Amazon EC2 Development Environment<a name="cli-chap-tutorial"></a>

This tutorial describes how to use the AWS CLI to set up a development environment in Amazon EC2\. It includes a short version of the installation and configuration instructions\. It can be run start to finish on Windows, Linux, macOS, or Unix\.

**Topics**
+ [Install the AWS CLI](#tutorial-install-cli)
+ [Configure the AWS CLI](#tutorial-configure-cli)
+ [Create a Security Group and Key Pair for the EC2 Instance](#tutorial-configure-security)
+ [Launch and Connect to the Instance](#tutorial-launch-and-connect)

## Install the AWS CLI<a name="tutorial-install-cli"></a>

You can install the AWS CLI with an installer \(Windows\) or by using `pip`, a package manager for Python\.

### Windows<a name="tutorial-install-cli-windows"></a>

1. Download the MSI installer\.
   + [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI64.msi)
   + [Download the AWS CLI MSI installer for Windows \(32\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI32.msi)

1. Run the downloaded MSI installer\.

1. Follow the instructions that appear\.

### Linux, macOS, or Unix<a name="tutorial-install-cli-unix"></a>

These steps require that you have a working installation of Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+\. If you encounter any issues using the following steps, see the full installation instructions in the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\. 

1. If you have pip installed, skip to step 2\. If you don't already have pip installed, then download and run the installation script from the [pip website](https://pip.pypa.io/en/latest/installing.html):

   ```
   $ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
     % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
   100 1622k  100 1622k    0     0  14.9M      0 --:--:-- --:--:-- --:--:-- 14.9M
   $ python3 get-pip.py --user
   Collecting pip
     Using cached https://files.pythonhosted.org/packages/c2/d7/90f34cb0d83a6c5631cf71dfe64cc1054598c843a92b400e55675cc2ac37/pip-18.1-py2.py3-none-any.whl
   Collecting setuptools
     Using cached https://files.pythonhosted.org/packages/e7/16/da8cb8046149d50940c6110310983abb359bbb8cbc3539e6bef95c29428a/setuptools-40.6.2-py2.py3-none-any.whl
   Collecting wheel
     Using cached https://files.pythonhosted.org/packages/ff/47/1dfa4795e24fd6f93d5d58602dd716c3f101cfd5a77cd9acbe519b44a0a9/wheel-0.32.3-py2.py3-none-any.whl
   Installing collected packages: pip, setuptools, wheel
     The script wheel is installed in '/home/myusername/.local/bin' which is not on PATH.
     Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
   ```

   The `--user` switch specifies that you want to install pip in your user's local home directory\. This is required if you don't have root/administrator permissions\. If you don't specify `--user`, then pip tries to install in a system folder for all users\.

1. Ensure that the pip installation folder is in your PATH environment variable\. The installer typically informs you if it is not, as shown in the previous example\. To address this, add a statement like the following at the end of your shell's RC script, which is appropriate if `pip` was installed into the `.local/bin` folder in your home directory\.

   ```
   export PATH=~/.local/bin:$PATH
   ```

1. Now you can use `pip` to install the AWS CLI\. 

   ```
   $ pip install awscli --user
   ```

   We again recommend that you use the `--user` switch to install the AWS CLI in your local home folder which does not require root/administrator permissions\.

## Configure the AWS CLI<a name="tutorial-configure-cli"></a>

First, configure the AWS CLI with your credentials and default settings\.

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-2
Default output format [None]: json
```

The AWS CLI prompts you for the following information:
+ **AWS Access Key ID and AWS Secret Access Key** – These are your user or account credentials\. If you don't have keys, see [Access Keys \(Access Key ID and Secret Access Key\)](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *Amazon Web Services General Reference*\.
+ **Default region name** – This specifies the name of the region you want the CLI to send its requests to by default\.
+ **Default output format** – This specifies the output format you want the CLI to use by default\. The value can be: `json`, `text`, or `table`\. If you don't specify an output format, `json` is used\.

Now try a simple command to verify that your credentials are configured correctly and that you can connect to AWS\.

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
||  ec2.eu-west-3.amazonaws.com      |  eu-west-3       ||
||  ec2.eu-west-2.amazonaws.com      |  eu-west-2       ||
||  ec2.eu-west-1.amazonaws.com      |  eu-west-1       ||
||  ec2.ap-northeast-3.amazonaws.com |  ap-northeast-3  ||
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

## Create a Security Group and Key Pair for the EC2 Instance<a name="tutorial-configure-security"></a>

Your next step is to set up the prerequisites for launching an Amazon EC2 instance that can be accessed with a terminal emulator connected using the [Secure Shell \(SSH\)](https://www.ssh.com/ssh/) protocol\. For more information about Amazon EC2 and its features, see the *[Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)*\.

Amazon EC2 requires the following prerequisites to communicate with an EC2 instance:
+ **[Security group](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)** – a security group determines what network traffic is allowed to enter and leave your instance\. You can think of it as a virtual firewall\. A security group contains rules that control inbound and outbound network traffic\.
+ **[Key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)** – Public–key cryptography uses a public key to encrypt a piece of data, such as a password, then the recipient uses the private key to decrypt the data\. Amazon EC2 uses the specified key pair to encrypt the credentials used to access the instance\. 

**To create a security group and key pair**

1. First, create a new security group for the VPC in which you'll launch the instance\. If you are using the default VPC for the region, you can omit the `--vpc-id` parameter; otherwise, specify the ID of the VPC in which you'll launch your instance\. The output shows the identifier of your new security group\.

   ```
   $ aws ec2 create-security-group --group-name devenv-sg --vpc-id vpc-xxxxxxxx --description "Security group for development environment"
   {
       "GroupId": "sg-b018ced5"
   }
   ```

1. Next, create a rule for your security group that enables inbound network traffic on port 22 from the CIDR network address range that you'll use to connect to the instance\.
**Important**  
This example shows the `0.0.0.0/0` CIDR range which enables inbound traffic from anywhere on the internet\. We strongly recommend that you replace this with the *public* CIDR range of the network \(as seen by AWS\) from which you'll connect to your instance\. 

   ```
   $ aws ec2 authorize-security-group-ingress --group-name devenv-sg --protocol tcp --port 22 --cidr 0.0.0.0/0
   ```

   Make a note of the security group ID\. You'll need it later when you launch the instance\.

1. Next, create the SSH cryptographic key pair that you'll use to connect to the instance\. This example command shows how to save the contents of the key to a file named `devenv-key.pem`\.

   ```
   $ aws ec2 create-key-pair --key-name devenv-key --query "KeyMaterial" --output text > devenv-key.pem
   ```

   The `--query "KeyMaterial"` parameter extracts only the part of the output that you need in the \.pem file\.
**Double quotes for the `--query` parameter**  
All of the examples in this topic that include the `--query` parameter use double\-quotes\. Although Linux lets you use single quotes for the `--query` parameter, the Windows Command prompt requires you to use double quotes instead of single quotes\.

1. On Linux, change the access for the new key file so that only you have access to it\. 

   ```
   $ chmod 400 devenv-key.pem
   ```

## Launch and Connect to the Instance<a name="tutorial-launch-and-connect"></a>

You are now ready to launch an instance and connect to it\. 

**To launch and connect to the instance**

1. Run the following command, using the ID of the security group that you created in the previous step\. The `--image-id` parameter specifies the Amazon Machine Image \(AMI\) that Amazon EC2 uses to bootstrap the instance\. You can find an image ID for your region and operating system using the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\. If you are using the default subnet for a default VPC, you can omit the `--subnet-id` parameter; otherwise, specify the ID of the subnet in which you'll launch your instance\. 
**Note**  
This example shows the command split across multiple lines with the Linux '\\' line continuation character\. You can, of course, submit it all as a single line\. For the Windows command line, replace the '\\' with a '^'\.

   ```
   $ aws ec2 run-instances --image-id ami-xxxxxxxx \
                                --subnet-id subnet-xxxxxxxx \
                                --security-group-ids sg-b018ced5 \
                                --count 1 \
                                --instance-type t2.micro \
                                --key-name devenv-key \
                                --query "Instances[0].InstanceId"
   "i-0787e4282810ef9cf"
   ```

1. The instance takes a few moments to launch\. After the instance is up and running, you can retrieve the public IP address of the instance that you'll need to connect it with the following command:

   ```
   $ aws ec2 describe-instances --instance-ids i-0787e4282810ef9cf --query "Reservations[0].Instances[0].PublicIpAddress"
   "54.183.22.255"
   ```

1. To connect to the instance, use the public IP address and private key \.pem file with your preferred terminal program\. On Linux, macOS, or Unix, you can do this from the command line using the following command: 

   ```
   $ ssh -i devenv-key.pem user@54.183.22.255
   ```

   If you get an error like *Permission denied \(publickey\)* when attempting to connect to your instance, check that the following are correct:
   + **Key** – The key specified must be at the path indicated and must be the private key, not the public one\. Permissions on the key must be restricted to the owner\.
   + **User** – The user name must match the default user name associated with the AMI you used to launch the instance\. For an Ubuntu AMI, this is `ubuntu`\. For an Amazon Linux AMI, it is `ec2-user`\.
   + **Instance** – The public IP address or DNS name of the instance\. Verify that the address is public and that port 22 is open to your local machine on the instance's security group\.

   You can also use the `-v` option to view additional information related to the error\.
**SSH on Windows**  
On Windows, you can use the PuTTY terminal application available [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/)\. Get `putty.exe` and `puttygen.exe` from the downloads page\.   
Use `puttygen.exe` to convert your private key to a `.ppk` file required by PuTTY\. Launch `putty.exe`, enter the public IP address of the instance in the **Host Name** field, and set the connection type to SSH\.   
In the **Category** panel, navigate to **Connection** > **SSH** > **Auth**, and click **Browse** to select your `.ppk` file, and then click **Open** to connect\. 

1. The terminal prompts you to accept the server's public key\. Type `yes` and press **Enter** to complete the connection\. 

You've now configured a security group, created a key pair, launched an EC2 instance, and connected to it without ever leaving the command line\. 