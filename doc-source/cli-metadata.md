# Instance Metadata<a name="cli-metadata"></a>

To use the CLI from an EC2 instance, create a role that has access to the resources needed and assign that role to the instance when it is launched\. Launch the instance and check to see if the AWS CLI is already installed \(it comes pre\-installed on Amazon Linux\)\. 

 Install the AWS CLI if necessary and configure a default region to avoid having to specify it in every command\. You can set the region using `aws configure` without entering credentials by pressing enter twice to skip the first two prompts: 

```
$ aws configure
AWS Access Key ID [None]: ENTER
AWS Secret Access Key [None]: ENTER
Default region name [None]: us-west-2
Default output format [None]: json
```

 The AWS CLI will read credentials from the instance metadata\. For more information, see [ Granting Applications that Run on Amazon EC2 Instances Access to AWS Resources](http://docs.aws.amazon.com/IAM/latest/UserGuide/role-usecase-ec2app.html) in *IAM User Guide*\. 