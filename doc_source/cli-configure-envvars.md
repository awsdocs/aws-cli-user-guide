# Environment Variables To Configure the AWS CLI<a name="cli-configure-envvars"></a>

Environment variables provide another way to specify configuration options and credentials, and can be useful for scripting or temporarily setting a named profile as the default\.

**Precedence of options**
+ If you specify an option by using one of the environment variables described in this topic, it overrides any value loaded from a profile in the configuration file\. 
+ If you specify an option by using a parameter on the CLI command line, it overrides any value from either the corresponding environment variable or a profile in the configuration file\.Supported environment variables

The AWS CLI supports the following environment variables\.

`AWS_ACCESS_KEY_ID`  
Specifies an AWS access key associated with an IAM user or role\.  
If defined, this environment variable overrides the value for the profile setting `aws_access_key_id`\. You can't specify the access key ID by using a command line option\.

`AWS_CA_BUNDLE`  
Specifies the path to a certificate bundle to use for HTTPS certificate validation\.  
If defined, this environment variable overrides the value for the profile setting `ca_bundle`\. You can override this environment variable by using the `--ca-bundle` command line parameter\.

`AWS_CONFIG_FILE`  
Specifies the location of the file that the AWS CLI uses to store configuration profiles\. The default path is `~/.aws/config`\)\.  
You can't specify this value in a named profile setting or by using a command line parameter\.

[`AWS_DEFAULT_OUTPUT`](cli-chap-configure.md#cli-quick-configuration-format)  
Specifies the [output format](cli-usage-output.md) to use\.  
If defined, this environment variable overrides the value for the profile setting `output`\. You can override this environment variable by using the `--output` command line parameter\.

[`AWS_DEFAULT_REGION`](cli-chap-configure.md#cli-quick-configuration-region)  
Specifies the AWS Region to send the request to\.  
If defined, this environment variable overrides the value for the profile setting `region`\. You can override this environment variable by using the `--region` command line parameter\.

[`AWS_PAGER`](cli-configure-files.md#cli-config-cli_pager)  
Specifies the pager program used for output\. By default, AWS CLI version 2 returns all output through your operating system’s default pager program\.  
To disable all use of an external paging program, set the variable to an empty string\.   
If defined, this environment variable overrides the value for the profile setting `cli_pager`\.

[`AWS_PROFILE`](cli-configure-profiles.md)  
Specifies the name of the CLI profile with the credentials and options to use\. This can be the name of a profile stored in a `credentials` or `config` file, or the value `default` to use the default profile\.   
If defined, this environment variable overrides the behavior of using the profile named `[default]` in the configuration file\. You can override this environment variable by using the `--profile` command line parameter\.

[`AWS_ROLE_SESSION_NAME`](cli-configure-role.md#cli-configure-role-session-name)  
Specifies a name to associate with the role session\. This value appears in CloudTrail logs for commands performed by the user of this profile\.  
If defined, this environment variable overrides the value for the profile setting `role_session_name`\. You can't specify a role session name as a command line parameter\.

`AWS_SECRET_ACCESS_KEY`  
Specifies the secret key associated with the access key\. This is essentially the "password" for the access key\.  
If defined, this environment variable overrides the value for the profile setting `aws_secret_access_key`\. You can't specify the access key ID as a command line option\.

`AWS_SESSION_TOKEN`  
Specifies the session token value that is required if you are using temporary security credentials that you retrieved directly from AWS STS operations\. For more information, see the [Output section of the assume\-role command](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html#output) in the *AWS CLI Command Reference*\.  
If defined, this environment variable overrides the value for the profile setting `aws_session_token`\. You can't specify the session token as a command line option\.

`AWS_SHARED_CREDENTIALS_FILE`  
Specifies the location of the file that the AWS CLI uses to store access keys\. The default path is `~/.aws/credentials`\)\.  
You can't specify this value in a named profile setting or by using a command line parameter\.

**Note**  
You can't specify AWS Single Sign\-On \(AWS SSO\) authentication by using environment variables\. Instead, you must use a named profile in the shared configuration file `.aws/config`\. For more information, see [Configuring the AWS CLI to use AWS Single Sign\-On ](cli-configure-sso.md)\. 

The following example shows how you could configure environment variables for the default user\. These values would override any values found in a named profile, or instance metadata\. Once set, you can override these values by specifying a parameter on the CLI command line, or by changing or removing the environment variable\. For more information about precedence and how the AWS CLI determines which credentials to use, see [Configuration Settings and Precedence](cli-chap-configure.md#config-settings-and-precedence)\.

**Linux or macOS**

```
$ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
$ export AWS_DEFAULT_REGION=us-west-2
```

Setting the environment variable changes the value used until the end of your shell session, or until you set the variable to a different value\. You can make the variables persistent across future sessions by setting them in your shell's startup script\.

**Windows Command Prompt**

```
C:\> setx AWS_ACCESS_KEY_ID AKIAIOSFODNN7EXAMPLE
C:\> setx AWS_SECRET_ACCESS_KEY wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
C:\> setx AWS_DEFAULT_REGION us-west-2
```

Using `[set](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1)` to set an environment variable changes the value used until the end of the current command prompt session, or until you set the variable to a different value\. Using [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx) to set an environment variable changes the value used in both the current command prompt session and all command prompt sessions that you create after running the command\. It does ***not*** affect other command shells that are already running at the time you run the command\.

**PowerShell**

```
PS C:\> $Env:AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
PS C:\> $Env:AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
PS C:\> $Env:AWS_DEFAULT_REGION="us-west-2"
```

If you set an environment variable at the PowerShell prompt as shown in the previous examples, it saves the value for only the duration of the current session\. To make the environment variable setting persistent across all PowerShell and Command Prompt sessions, store it by using the **System** application in **Control Panel**\. Alternatively, you can set the variable for all future PowerShell sessions by adding it to your PowerShell profile\. See the [PowerShell documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_environment_variables) for more information about storing environment variables or persisting them across sessions\.