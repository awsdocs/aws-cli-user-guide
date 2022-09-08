--------

--------

# Using the AWS CLI wizards<a name="cli-usage-wizard"></a>

The AWS Command Line Interface \(AWS CLI\) provides the ability to use a wizard for some commands\. To contribute or view the full list of available AWS CLI wizards, see the [AWS CLI wizards folder](https://github.com/aws/aws-cli/tree/v2/awscli/customizations/wizard/wizards) on GitHub\. 

## How it works<a name="cli-usage-wizard-how"></a>

Similar to the AWS console, the AWS CLI has a UI wizard that guides you through managing your AWS resources\. To use the wizard, you call the `wizard` subcommand and the wizard name after the service name in a command\. The command structure is as follows:

**Syntax:**

```
$ aws <command> wizard <wizardName>
```

The following example is calling the wizard to create a new `dynamodb` table\.

```
$ aws dynamodb wizard new-table
```

`aws configure` is the only wizard that does not have a wizard name\. When running the wizard, run the `aws configure wizard` command as the following example demonstrates:

```
$ aws configure wizard
```

After calling a wizard, a form in the shell is displayed\. For each parameter, you are either provided a list of options to select from or prompted to enter in a string\. To select from a list, use your up and down arrow keys and press **ENTER**\. To view details on an option, press the right arrow key\. When you've finished filling out a parameter, press **ENTER**\.

```
$ aws configure wizard
What would you like to configure
> Static Credentials
  Assume Role
  Process Provider
  Additional CLI configuration
Enter the name of the profile: 
Enter your Access Key Id: 
Enter your Secret Access Key:
```

To edit previous prompts, use **SHIFT** \+ **TAB**\. For some wizards, after filling in all prompts, you can preview an AWS CloudFormation template or the AWS CLI command filled with your information\. This preview mode is useful to learn the AWS CLI, service APIs, and creating templates for scripts\.

Press **ENTER** after previewing or the last prompt to run the final command\.

```
$ aws configure wizard
What would you like to configure
Enter the name of the profile: testWizard
Enter your Access Key Id: AB1C2D3EF4GH5I678J90K
Enter your Secret Access Key: ab1c2def34gh5i67j8k90l1mnop2qr3s45tu678v90
<ENTER>
```