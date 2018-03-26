# Named Profiles<a name="cli-multiple-profiles"></a>

 The AWS CLI supports *named profiles* stored in the config and credentials files\. You can configure additional profiles by using `aws configure` with the `--profile` option or by adding entries to the config and credentials files\. 

The following example shows a credentials file with two profiles:

**`~/.aws/credentials`**

```
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[user2]
aws_access_key_id=AKIAI44QH8DHBEXAMPLE
aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

Each profile uses different credentials—perhaps from two different IAM users—and can also use different regions and output formats\.

**`~/.aws/config`**

```
[default]
region=us-west-2
output=json

[profile user2]
region=us-east-1
output=text
```

**Important**  
The AWS credentials file uses a different naming format than the CLI config file for named profiles\. Do not include the 'profile ' prefix when configuring a named profile in the AWS credentials file\.

## Using Profiles with the AWS CLI<a name="using-profiles"></a>

To use a named profile, add the `--profile` option to your command\. The following example lists running instances using the `user2` profile from the previous section\.

```
$ aws ec2 describe-instances --region us-east-1 --profile user2
```

If you are going to use a named profile for multiple commands, you can avoid specifying the profile in every command by setting the AWS\_PROFILE environment variable at the command line:

**Linux, macOS, or Unix**

```
$ export AWS_PROFILE=user2
```

**Windows**

```
> set AWS_PROFILE=user2
```

Setting the environment variable changes the default profile until the end of your shell session, or until you set the variable to a different value\. More on variables in the next section\.
