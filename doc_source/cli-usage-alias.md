--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Creating and using AWS CLI aliases<a name="cli-usage-alias"></a>

Aliases are shortcuts you can create in the AWS Command Line Interface \(AWS CLI\) to shorten commands or scripts that you frequently use\. You create aliases in the `alias` file located in your configuration folder\.

**Topics**
+ [Prerequisites](#cli-usage-alias-prepreqs)
+ [Step 1: Creating the alias file](#cli-usage-alias-create-file)
+ [Step 2: Creating an alias](#cli-usage-alias-create-alias)
+ [Step 3: Calling an alias](#cli-usage-alias-call-alias)
+ [Alias repository examples](#cli-usage-alias-examples)
+ [Resources](#cli-usage-alias-references)

## Prerequisites<a name="cli-usage-alias-prepreqs"></a>

To use alias commands, you need to complete the following:
+ Install and configure the AWS CLI\. For more information, see [Installing, updating, and uninstalling the AWS CLI](cli-chap-install.md) and [Configuration basics](cli-configure-quickstart.md)\.
+ Use a minimum AWS CLI version of 1\.11\.24 or 2\.0\.0\.
+ \(Optional\) To use AWS CLI alias bash scripts, you must use a bash\-compatible terminal\.

## Step 1: Creating the alias file<a name="cli-usage-alias-create-file"></a>

To create the `alias` file, you can use your file navigation and a text editor, or use your preferred terminal by using the step\-by\-step procedure\. To quickly create your alias file, use the following command block\.

------
#### [ Linux and macOS ]

```
$ mkdir -p ~/.aws/cli
$ echo '[toplevel]' > ~/.aws/cli/alias
```

------
#### [ Windows ]

```
C:\> md %USERPROFILE%\.aws\cli
C:\> echo [toplevel] > %USERPROFILE%/.aws/cli/alias
```

------

**To create the alias file**

1. Create a folder named `cli` in your AWS CLI configuration folder\. By default the configuration folder is `~/.aws/` on Linux or macOS and `%USERPROFILE%\.aws\` on Windows\. You can create this through your file navigation or by using the following command\.

------
#### [ Linux and macOS ]

   ```
   $ mkdir -p ~/.aws/cli
   ```

------
#### [ Windows ]

   ```
   C:\> md %USERPROFILE%\.aws\cli
   ```

------

   The resulting `cli` folder default path is `~/.aws/cli/` on Linux or macOS and `%USERPROFILE%\.aws\cli` on Windows\.

1. In the `cli` folder, create a text file named `alias` with no extension and add `[toplevel]` to the first line\. You can create this file through your preferred text editor or use the following command\.

------
#### [ Linux and macOS ]

   ```
   $ echo '[toplevel]' > ~/.aws/cli/alias
   ```

------
#### [ Windows ]

   ```
   $ echo [toplevel] > %USERPROFILE%/.aws/cli/alias
   ```

------

## Step 2: Creating an alias<a name="cli-usage-alias-create-alias"></a>

You can create an alias using basic commands or bash scripting\.

### Creating a basic command alias<a name="cli-usage-alias-create-alias-basic"></a>

You can create your alias by adding a command using the following syntax in the `alias` file you created in the previous step\. 

**Syntax**

```
aliasname = command [--options]
```

The *aliasname* is what you call your alias\. The *command* is the command you want to call, which can include other aliases\. You can include options or parameters in your alias, or add them when calling your alias\.

The following example creates an alias named `aws whoami` using the [https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html) command\. Since this alias calls an existing AWS CLI command, you can write the command without the `aws` prefix\.

```
whoami = sts get-caller-identity
```

The following example takes the previous `whoami` example and adds the `Account` filter and text `output` options\.

```
whoami2 = sts get-caller-identity --query AccountName --output text
```

### Creating a bash scripting alias<a name="cli-usage-alias-create-alias-scripting"></a>

**Warning**  
To use AWS CLI alias bash scripts, you must use a bash\-compatible terminal

You can create an alias using bash scripts for more advanced processes using the following syntax\.

**Syntax**

```
aliasname = 
    !f() {
        script content
}; f
```

The *aliasname* is what you call your alias and *script content* is the script you want to run when you call the alias\.

The following example uses `opendns` to output your current IP address\. Since you can use aliases in other aliases, the following `myip` alias is useful to allow or revoke access for your IP address from within other aliases\. 

```
myip =
  !f() {
    dig +short myip.opendns.com @resolver1.opendns.com
  }; f
```

The following script example calls the previous `aws myip` alias to authorize your IP address for an Amazon EC2 security group ingress\.

```
authorize-my-ip =
  !f() {
    ip=$(aws myip)
    aws ec2 authorize-security-group-ingress --group-id ${1} --cidr $ip/32 --protocol tcp --port 22
  }; f
```

When you call aliases that use bash scripting, the variables are always passed in the order that you entered them\. In bash scripting, the variable names are not taken into consideration, only the order they appear\. In the following `textalert` alias example, the variable for the `--message` option is first and `--phone-number` option is second\.

```
textalert =
  !f() {
    aws sns publish --message "${1}" --phone-number ${2}
  }; f
```

## Step 3: Calling an alias<a name="cli-usage-alias-call-alias"></a>

To run the alias you created in your `alias` file use the following syntax\. You can add additional options when you call your alias\.

**Syntax**

```
$ aws aliasname
```

The following example uses the `aws whoami` alias\.

```
$ aws whoami
{
    "UserId": "A12BCD34E5FGHI6JKLM",
    "Account": "1234567890987",
    "Arn": "arn:aws:iam::1234567890987:user/userName"
}
```

The following example uses the `aws whoami` alias with additional options to only return the `Account` number in `text` output\.

```
$ aws whoami --query Account --output text
1234567890987
```

### Calling an alias using bash scripting variables<a name="cli-usage-alias-call-alias-variables"></a>

When you call aliases that use bash scripting, variables are passed in the order they are entered\. In bash scripting, the name of the variables are not taken into consideration, only the order they appear\. For example, in the following `textalert` alias, the variable for the option `--message` is first and `--phone-number` is second\.

```
textalert =
  !f() {
    aws sns publish --message "${1}" --phone-number ${2}
  }; f
```

When you call the `textalert` alias, you need to pass variables in the same order as they are run in the alias\. In the following example we use the variables `$message` and `$phone`\. The `$message` variable is passed as `${1}` for the `--message` option and the `$phone` variable is passed as `${2}` for the `--phone-number` option\. This results in successfully calling the `textalert` alias to send a message\.

```
$ aws textalert $message $phone
{
    "MessageId": "1ab2cd3e4-fg56-7h89-i01j-2klmn34567"
}
```

In the following example, the order is switched when calling the alias to `$phone` and `$message`\. The `$phone` variable is passed as `${1}` for the `--message` option and the `$message` variable is passed as `${2}` for the `--phone-number` option\. Since the variables are out of order, the alias passes the variables incorrectly\. This causes an error because the contents of `$message` do not match the phone number formatting requirements for the `--phone-number` option\.

```
$ aws textalert $phone $message
usage: aws [options] <command> <subcommand> [<subcommand> ...] [parameters]
To see help text, you can run:

  aws help
  aws <command> help
  aws <command> <subcommand> help

Unknown options: text
```

## Alias repository examples<a name="cli-usage-alias-examples"></a>

The [AWS CLI alias repository](https://github.com/awslabs/awscli-aliases) on *GitHub* contains AWS CLI alias examples created by the AWS CLI developer team and community\. You can use the entire `alias` file example or take individual aliases for your own use\.

**Warning**  
Running the commands in this section deletes your existing `alias` file\. To avoid overwriting your existing alias file, change your download location\.

**To use aliases from the repository**

1. Install Git\. For installation instructions, see [Getting Started \- Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) in the *Git Documentation*\.

1. Install the `jp` command\. The `jp` command is used in the `tostring` alias\. For installation instructions, see the [JMESPath \(jp\) README\.md](https://github.com/jmespath/jp) on *GitHub*\.

1. Install the `jq` command\. The `jq` command is used in the `tostring-with-jq` alias\. For installation instructions, see the [JSON processor \(jq\)](https://stedolan.github.io/jq/download/) on *GitHub*\.

1. Download the `alias` file by doing one of the following:
   + Run the following commands that downloads from the repository and copies the `alias` file to your configuration folder\.

------
#### [ Linux and macOS ]

     ```
     $ git clone https://github.com/awslabs/awscli-aliases.git
     $ mkdir -p ~/.aws/cli
     $ cp awscli-aliases/alias ~/.aws/cli/alias
     ```

------
#### [ Windows ]

     ```
     C:\> git clone https://github.com/awslabs/awscli-aliases.git
     C:\> md %USERPROFILE%\.aws\cli
     C:\> copy awscli-aliases\alias %USERPROFILE%\.aws\cli
     ```

------
   + Download directly from the repository and save to the `cli` folder in your AWS CLI configuration folder\. By default the configuration folder is `~/.aws/` on Linux or macOS and `%USERPROFILE%\.aws\` on Windows\. 

1. To verify the aliases are working, run the following alias\.

   ```
   $ aws whoami
   ```

   This displays the same response as the `aws sts get-caller-identity` command:

   ```
   {
       "Account": "012345678901",
       "UserId": "AIUAINBADX2VEG2TC6HD6",
       "Arn": "arn:aws:iam::012345678901:user/myuser"
   }
   ```

## Resources<a name="cli-usage-alias-references"></a>
+ The [AWS CLI alias repository](https://github.com/awslabs/awscli-aliases) on *GitHub* contains AWS CLI alias examples created by the AWS CLI developer team and the contribution of the AWS CLI community\.
+ The alias feature announcement from [AWS re:Invent 2016: The Effective AWS CLI User](https://www.youtube.com/watch?t=1590&v=Xc1dHtWa9-Q) on *YouTube*\. 
+ [https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html)