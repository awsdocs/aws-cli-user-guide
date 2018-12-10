# Instance Metadata<a name="cli-configure-metadata"></a>

When you run the AWS CLI from within an Amazon EC2 instance, you can simplify providing credentials to your commands\. Each Amazon EC2 instance contains metadata that the AWS CLI can directly query for temporary credentials\. To provide these, create an IAM role that has access to the resources needed and attach that role to the Amazon EC2 instance when you launch it\.

Launch the instance and check to see if the AWS CLI is already installed \(it comes pre\-installed on Amazon Linux\)\. Install the AWS CLI if necessary\. You must still configure a default region to avoid having to specify it in every command\. 

You can set the region and default output format by running `aws configure` without specifying credentials by pressing enter twice to skip the first two prompts: 

```
$ aws configure
AWS Access Key ID [None]: ENTER
AWS Secret Access Key [None]: ENTER
Default region name [None]: us-west-2
Default output format [None]: json
```

When an IAM role is attached to the instance, the AWS CLI automatically and securely retrieves the credentials from the instance metadata\. For more information, see [Granting Applications that Run on Amazon EC2 Instances Access to AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in *IAM User Guide*\.