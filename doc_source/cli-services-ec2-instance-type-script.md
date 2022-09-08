--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Change an Amazon EC2 instance type using a bash script<a name="cli-services-ec2-instance-type-script"></a>

This bash scripting example for Amazon EC2 changes the instance type for an Amazon EC2 instance using the AWS Command Line Interface \(AWS CLI\)\. It stops the instance if it's running, changes the instance type, and then, if requested, restarts the instance\. Shell scripts are programs designed to run in a command line interface\.

**Note**  
For additional command examples, see the [AWS CLI reference guide](https://docs.aws.amazon.com/cli/latest/reference/)\.

**Topics**
+ [Before you start](#cli-services-ec2-instance-type-script-prereqs)
+ [About this example](#cli-services-ec2-instance-type-script-about)
+ [Parameters](#cli-services-ec2-instance-type-script-params)
+ [Files](#cli-services-ec2-instance-type-script-files.title)
+ [References](#cli-services-ec2-instance-type-script-references)

## Before you start<a name="cli-services-ec2-instance-type-script-prereqs"></a>

Before you can run any of the below examples, the following things need to be completed\.
+ AWS CLI installed, see [Installing, updating, and uninstalling the AWS CLI](cli-chap-install.md) for more information\.
+ AWS CLI configured, see [Configuration basics](cli-configure-quickstart.md) for more information\. The profile that you use must have permissions that allow the AWS operations performed by the examples\.
+ A running Amazon EC2 instance in the account for which you have permission to stop and modify\. If you run the test script, it launches an instance for you, tests changing the type, and then terminates the instance\.
+ As an AWS best practice, grant this code least privilege, or only the permissions required to perform a task\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *AWS Identity and Access Management \(IAM\) User Guide*\.
+ This code has not been tested in all AWS Regions\. Some AWS services are available only in specific Regions\. For more information, see [ Service Endpoints and Quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *AWS General Reference Guide*\. 
+ Running this code can result in charges to your AWS account\. It is your responsibility to ensure that any resources created by this script are removed when you are done with them\. 

## About this example<a name="cli-services-ec2-instance-type-script-about"></a>

This example is written as a function in the shell script file `change_ec2_instance_type.sh` that you can `source` from another script or from the command line\. Each script file contains comments describing each of the functions\. Once the function is in memory, you can invoke it from the command line\. For example, the following commands change the type of the specified instance to `t2.nano`:

```
$ source ./change_ec2_instance_type.sh
$ ./change_ec2_instance_type -i *instance-id* -t new-type
```

For the full example and downloadable script files, see [Change Amazon EC2 Instance Type](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/aws-cli/bash-linux/ec2/change-ec2-instance-type) in the *AWS Code Examples Repository* on *GitHub*\.

## Parameters<a name="cli-services-ec2-instance-type-script-params"></a>

**\-i** \- *\(string\)* Specifies the instance ID to modify\.

**\-t** \- *\(string\)* Specifies the Amazon EC2 instance type to switch to\.

**\-r** \- *\(switch\)* By default, this is unset\. If `-r` is set, restarts the instance after the type switch\.

**\-f** \- *\(switch\)* By default, the script prompts the user to confirm shutting down the instance before making the switch\. If `-f` is set, the function doesn't prompt the user before shutting down the instance to make the type switch

**\-v** \- *\(switch\)* By default, the script operates silently and displays output only in the event of an error\. If `-v` is set, the function displays status throughout its operation\.

## Files<a name="cli-services-ec2-instance-type-script-files.title"></a>

**`change_ec2_instance_type.sh`**  
The main script file contains the `change_ec2_instance_type()` function that performs the following tasks:  
+ Verifies that the specified Amazon EC2 instance exists\.
+ Unless `-f` is selected, warns the user before stopping the instance\.
+ Changes the instance type
+ If you set `-r`, restarts the instance and confirms that the instance is running
View the code for `[change\_ec2\_instance\_type\.sh](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/aws-cli/bash-linux/ec2/change-ec2-instance-type/change_ec2_instance_type.sh)` on *GitHub*\.

**`test_change_ec2_instance_type.sh`**  
The file `change_ec2_instance_type_test.sh` script tests the various code paths for the `change_ec2_instance_type` function\. If all steps in the test script work correctly, the test script removes all resources that it created\.  
You can run the test script with the following parameters:  
+ **\-v** \- *\(switch\)* The each test shows a pass/failure status as they run\. By default, the tests runs silently and the output includes only the final overall pass/failure status\.
+ **\-i** \- *\(switch\)* The script pauses after each test to enable you to browse the intermediate results of each step\. Enables you to examine the current status of the instance using the Amazon EC2 console\. The script proceeds to the next step after you press *ENTER* at the prompt\.
View the code for `[test\_change\_ec2\_instance\_type\.sh](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/aws-cli/bash-linux/ec2/change-ec2-instance-type/test_change_ec2_instance_type.sh)` on *GitHub*\.

**`awsdocs_general.sh`**  
The script file `awsdocs_general.sh` holds general purpose functions used across advanced examples for the AWS CLI\.  
View the code for `[awsdocs\_general\.sh](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/aws-cli/bash-linux/ec2/change-ec2-instance-type/awsdocs_general.sh)` on *GitHub*\.

## References<a name="cli-services-ec2-instance-type-script-references"></a>

**AWS CLI reference:**
+ `[aws ec2](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html)`
+ `[aws ec2 describe\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html)`
+ `[aws ec2 modify\-instance\-attribute](https://docs.aws.amazon.com/cli/latest/reference/ec2/modify-instance-attribute.html)`
+ `[aws ec2 start\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/start-instances.html)`
+ `[aws ec2 stop\-instances](https://docs.aws.amazon.com/cli/latest/reference/ec2/stop-instances.html)`
+ `[aws ec2 wait instance\-running](https://docs.aws.amazon.com/cli/latest/reference/ec2/wait/instance-running.html)`
+ `[aws ec2 wait instance\-stopped](https://docs.aws.amazon.com/cli/latest/reference/ec2/wait/instance-stopped.html)`

**Other reference:**
+ [Amazon Elastic Compute Cloud Documentation](https://docs.aws.amazon.com/https://docs.aws.amazon.com/ec2/)
+ To view and contribute to AWS SDK and AWS CLI code examples, see the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/) on *GitHub*\.