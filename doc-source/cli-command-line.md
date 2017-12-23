# Command Line Options<a name="cli-command-line"></a>

 The AWS CLI uses GNU\-style long command line options preceded by two hyphens\. Command line options can be used to override default configuration settings for a single operation, but cannot be used to specify credentials\. 

The following settings can be specified at the command line\.

**\-\-profile** – name of a profile to use, or "default" to use the default profile\.

**\-\-region** – AWS region to call\.

**\-\-output** – output format\.

 **\-\-endpoint\-url** – The endpoint to make the call against\. The endpoint can be the address of a proxy or an endpoint URL for the in\-use AWS region\. Specifying an endpoint is not required for normal use as the AWS CLI determines which endpoint to call based on the in\-use region\. 

 The above options override the corresponding profile settings for a single operation\. Each takes a string argument with a space or equals sign \("="\) separating the argument from the option name\. Quotes around the argument are not required unless the argument string contains a space\. 

**Tip**  
You can use the \-\-profile option with `aws configure` to set up additional profiles  

```
$ aws configure --profile profilename
```

 Common uses for command line options include checking your resources in multiple regions and changing output format for legibility or ease of use when scripting\. For example, if you are not sure which region your instance is running in you could run the describe\-instances command against each region until you find it: 

```
$ aws ec2 describe-instances --output table --region us-east-1
-------------------
|DescribeInstances|
+-----------------+
$ aws ec2 describe-instances --output table --region us-west-1
-------------------
|DescribeInstances|
+-----------------+
$ aws ec2 describe-instances --output table --region us-west-2
------------------------------------------------------------------------------
|                              DescribeInstances                             |
+----------------------------------------------------------------------------+
||                               Reservations                               ||
|+-------------------------------------+------------------------------------+|
||  OwnerId                            |  012345678901                      ||
||  ReservationId                      |  r-abcdefgh                        ||
|+-------------------------------------+------------------------------------+|
|||                                Instances                               |||
||+------------------------+-----------------------------------------------+||
|||  AmiLaunchIndex        |  0                                            |||
|||  Architecture          |  x86_64                                       |||
...
```

 Command line option parameter types \(string, boolean, etc\.\) are discussed in detail in the  section later in this guide\. 