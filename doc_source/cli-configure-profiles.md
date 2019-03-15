# Named Profiles<a name="cli-configure-profiles"></a>

 The AWS CLI supports using any of multiple *named profiles* that are stored in the `config` and `credentials` files\. You can configure additional profiles by using `aws configure` with the `--profile` option, or by adding entries to the `config` and `credentials` files\. 

The following example shows a `credentials` file with two profiles\. The first is used when you run a CLI command with no profile\. The second is used when you run a CLI command with the `--profile user1` parameter\.

**`~/.aws/credentials`** \(Linux & Mac\) or **`%USERPROFILE%\.aws\credentials`** \(Windows\)

```
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[user1]
aws_access_key_id=AKIAI44QH8DHBEXAMPLE
aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
```

Each profile uses different credentials—perhaps from different IAM users—and can also specify different AWS Regions and output formats\.

**`~/.aws/config`** \(Linux & Mac\) or **`%USERPROFILE%\.aws\config`** \(Windows\)

```
[default]
region=us-west-2
output=json

[profile user1]
region=us-east-1
output=text
```

**Important**  
The `credentials` file uses a different naming format than the CLI `config` file for named profiles\. Include the prefix "`profile`" only when configuring a named profile in the `config` file\. Do ***not*** use `profile` when configuring the `credentials` file\.

## Using Profiles with the AWS CLI<a name="using-profiles"></a>

To use a named profile, add the `--profile profile-name` option to your command\. The following example lists all of your Amazon EC2 instances using the `user1` profile from the previous example files\.

```
$ aws ec2 describe-instances --profile user2
```

To use a named profile for multiple commands, you can avoid specifying the profile in every command by setting the `AWS_DEFAULT_PROFILE` environment variable at the command line\.

**Linux, macOS, or Unix**

```
$ export AWS_DEFAULT_PROFILE=user2
```

Setting the environment variable changes the default profile until the end of your shell session, or until you set the variable to a different value\. You can make environment variables persistent across future sessions by putting them in your shell's startup script\. For more information, see [Environment Variables](cli-configure-envvars.md)\.

**Windows**

```
C:\> setx AWS_DEFAULT_PROFILE user2
```

Using `[set](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1)` to set an environment variable changes the value used until the end of the current command prompt session, or until you set the variable to a different value\. Using [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx) to set an environment variable changes the value used in both the current command shell and all command shells that you create after running the command\. It does ***not*** affect other command shells that are already running at the time you run the command\.
