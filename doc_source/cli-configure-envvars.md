--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Environment variables to configure the AWS CLI<a name="cli-configure-envvars"></a>

Environment variables provide another way to specify configuration options and credentials, and can be useful for scripting or temporarily setting a named profile as the default\.

**Precedence of options**
+ If you specify an option by using one of the environment variables described in this topic, it overrides any value loaded from a profile in the configuration file\. 
+ If you specify an option by using a parameter on the AWS CLI command line, it overrides any value from either the corresponding environment variable or a profile in the configuration file\.

For more information about precedence and how the AWS CLI determines which credentials to use, see [Configuration settings and precedence](cli-configure-quickstart.md#cli-configure-quickstart-precedence)\.

**Topics**
+ [How to set environment variables](#envvars-set)
+ [AWS CLI supported environment variables](#envvars-list)

## How to set environment variables<a name="envvars-set"></a>

The following examples show how you can configure environment variables for the default user\.

------
#### [ Linux or macOS ]

```
$ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
$ export AWS_DEFAULT_REGION=us-west-2
```

Setting the environment variable changes the value used until the end of your shell session, or until you set the variable to a different value\. You can make the variables persistent across future sessions by setting them in your shell's startup script\.

------
#### [ Windows Command Prompt ]

**To set for all sessions**

```
C:\> setx AWS_ACCESS_KEY_ID AKIAIOSFODNN7EXAMPLE
C:\> setx AWS_SECRET_ACCESS_KEY wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
C:\> setx AWS_DEFAULT_REGION us-west-2
```

Using [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx) to set an environment variable changes the value used in both the current command prompt session and all command prompt sessions that you create after running the command\. It does ***not*** affect other command shells that are already running at the time you run the command\.

**To set for current session only**

Using `[set](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1)` to set an environment variable changes the value used until the end of the current command prompt session, or until you set the variable to a different value\. 

```
C:\> set AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
C:\> set AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
C:\> set AWS_DEFAULT_REGION=us-west-2
```

------
#### [ PowerShell ]

```
PS C:\> $Env:AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
PS C:\> $Env:AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
PS C:\> $Env:AWS_DEFAULT_REGION="us-west-2"
```

If you set an environment variable at the PowerShell prompt as shown in the previous examples, it saves the value for only the duration of the current session\. To make the environment variable setting persistent across all PowerShell and Command Prompt sessions, store it by using the **System** application in **Control Panel**\. Alternatively, you can set the variable for all future PowerShell sessions by adding it to your PowerShell profile\. See the [PowerShell documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_environment_variables) for more information about storing environment variables or persisting them across sessions\.

------

## AWS CLI supported environment variables<a name="envvars-list"></a>

The AWS CLI supports the following environment variables\.

`AWS_ACCESS_KEY_ID`  
Specifies an AWS access key associated with an IAM user or role\.  
If defined, this environment variable overrides the value for the profile setting `aws_access_key_id`\. You can't specify the access key ID by using a command line option\.

`AWS_CA_BUNDLE`  
Specifies the path to a certificate bundle to use for HTTPS certificate validation\.  
If defined, this environment variable overrides the value for the profile setting `ca\_bundle`\. You can override this environment variable by using the `\-\-ca\-bundle` command line parameter\.

`AWS_CONFIG_FILE`  
Specifies the location of the file that the AWS CLI uses to store configuration profiles\. The default path is `~/.aws/config`\.  
You can't specify this value in a named profile setting or by using a command line parameter\.

`AWS_DATA_PATH`  
A list of additional directories to check outside of the built\-in search path of `~/.aws/models` when loading AWS CLI data\. Setting this environment variable indicates additional directories to check first before falling back to the built\-in search path\. Multiple entries should be separated with the `os.pathsep` character, which is `:` on Linux or macOS and `;` on Windows\.

[`AWS_DEFAULT_OUTPUT`](cli-configure-quickstart.md#cli-configure-quickstart-format)  
Specifies the [output format](cli-usage-output.md) to use\.  
If defined, this environment variable overrides the value for the profile setting `output`\. You can override this environment variable by using the `--output` command line parameter\.

[`AWS_DEFAULT_REGION`](cli-configure-quickstart.md#cli-configure-quickstart-region)  
Specifies the AWS Region to send the request to\.  
If defined, this environment variable overrides the value for the profile setting `region` and \. You can override this environment variable by using the `--region` command line parameter\.

`AWS_EC2_METADATA_DISABLED`  
Disables the use of the Amazon EC2 instance metadata service \(IMDS\)\.   
If set to true, user credentials or configuration \(like the Region\) are not requested from IMDS\.

[`AWS_MAX_ATTEMPTS`](cli-configure-files.md#cli-config-max_attempts)  
Specifies a value of maximum retry attempts the AWS CLI retry handler uses, where the initial call counts toward the value that you provide\. For more information on retries, see [AWS CLI retries](cli-configure-retries.md)\.  
If defined, this environment variable overrides the value for the profiles setting `max_attempts`\.

`AWS_METADATA_SERVICE_NUM_ATTEMPTS`  
When attempting to retrieve credentials on an Amazon EC2 instance that has been configured with an IAM role, the AWS CLI attempts to retrieve credentials once from the instance metadata service before stopping\. If you know your commands will run on an Amazon EC2 instance, you can increase this value to make AWS CLI retry multiple times before giving up\.

`AWS_METADATA_SERVICE_TIMEOUT`  
The number of seconds before a connection to the instance metadata service should time out\. When attempting to retrieve credentials on an Amazon EC2 instance that is configured with an IAM role, a connection to the instance metadata service times out after 1 second by default\. If you know you're running on an Amazon EC2 instance with an IAM role configured, you can increase this value if needed\.

[`AWS_PROFILE`](cli-configure-profiles.md)  
Specifies the name of the AWS CLI profile with the credentials and options to use\. This can be the name of a profile stored in a `credentials` or `config` file, or the value `default` to use the default profile\.   
If defined, this environment variable overrides the behavior of using the profile named `[default]` in the configuration file\. You can override this environment variable by using the `--profile` command line parameter\.

[`AWS_RETRY_MODE`](cli-configure-files.md#cli-config-retry_mode)  
Specifies which retry mode AWS CLI uses\. There are three retry modes available: legacy \(default\), standard, and adaptive\. For more information on retries, see [AWS CLI retries](cli-configure-retries.md)\.  
If defined, this environment variable overrides the value for the profiles setting `retry_mode`\.

`AWS_ROLE_ARN`  
Specifies the Amazon Resource Name \(ARN\) of an IAM role with a web identity provider that you want to use to run the AWS CLI commands\.  
Used with the `AWS_WEB_IDENTITY_TOKEN_FILE` and `AWS_ROLE_SESSION_NAME` environment variables\.  
If defined, this environment variable overrides the value for the profile setting [`role_arn`](cli-configure-files.md#cli-config-role_arn)\. You can't specify a role session name as a command line parameter\.  
This environment variable only applies to an assumed role with web identity provider it does not apply to the general assume role provider configuration\.
For more information on using web identities, see [Assume role with web identity](cli-configure-role.md#cli-configure-role-oidc)\.

`AWS_ROLE_SESSION_NAME`  
Specifies the name to attach to the role session\. This value is provided to the `RoleSessionName` parameter when the AWS CLI calls the `AssumeRole` operation, and becomes part of the assumed role user ARN: ` arn:aws:sts::123456789012:assumed-role/role_name/role_session_name`\. This is an optional parameter\. If you do not provide this value, a session name is generated automatically\. This name appears in AWS CloudTrail logs for entries associated with this session\.  
If defined, this environment variable overrides the value for the profile setting [`role_session_name`](cli-configure-files.md#cli-config-role_session_name)\.  
Used with the `AWS_ROLE_ARN` and `AWS_WEB_IDENTITY_TOKEN_FILE` environment variables\.  
For more information on using web identities, see [Assume role with web identity](cli-configure-role.md#cli-configure-role-oidc)\.  
This environment variable only applies to an assumed role with web identity provider it does not apply to the general assume role provider configuration\.

`AWS_SECRET_ACCESS_KEY`  
Specifies the secret key associated with the access key\. This is essentially the "password" for the access key\.  
If defined, this environment variable overrides the value for the profile setting `aws_secret_access_key`\. You can't specify the secret access key ID as a command line option\.

`AWS_SESSION_TOKEN`  
Specifies the session token value that is required if you are using temporary security credentials that you retrieved directly from AWS STS operations\. For more information, see the [Output section of the assume\-role command](https://docs.aws.amazon.com/cli/latest/reference/sts/assume-role.html#output) in the *AWS CLI Command Reference*\.  
If defined, this environment variable overrides the value for the profile setting `aws_session_token`\.

`AWS_SHARED_CREDENTIALS_FILE`  
Specifies the location of the file that the AWS CLI uses to store access keys\. The default path is `~/.aws/credentials`\.  
You can't specify this value in a named profile setting or by using a command line parameter\.

[`AWS_STS_REGIONAL_ENDPOINTS`](cli-configure-files.md#cli-config-sts_regional_endpoints)  
Specifies how the AWS CLI determines the AWS service endpoint that the AWS CLI client uses to talk to the AWS Security Token Service \(AWS STS\)\. The default value for AWS CLI version 1 is `legacy`\.  
You can specify one of two values:  
+ **`legacy`** – Uses the global STS endpoint, `sts.amazonaws.com`, for the following AWS Regions: `ap-northeast-1`, `ap-south-1`, `ap-southeast-1`, `ap-southeast-2`, `aws-global`, `ca-central-1`, `eu-central-1`, `eu-north-1`, `eu-west-1`, `eu-west-2`, `eu-west-3`, `sa-east-1`, `us-east-1`, `us-east-2`, `us-west-1`, and `us-west-2`\. All other Regions automatically use their respective Regional endpoint\.
+ **`regional`** – The AWS CLI always uses the AWS STS endpoint for the currently configured Region\. For example, if the client is configured to use `us-west-2`, all calls to AWS STS are made to the Regional endpoint `sts.us-west-2.amazonaws.com` instead of the global `sts.amazonaws.com` endpoint\. To send a request to the global endpoint while this setting is enabled, you can set the Region to `aws-global`\.

[`AWS_WEB_IDENTITY_TOKEN_FILE`](#cli-configure-envvars)  
Specifies the path to a file that contains an OAuth 2\.0 access token or OpenID Connect ID token that is provided by an identity provider\. The AWS CLI loads the contents of this file and passes it as the `WebIdentityToken` argument to the `AssumeRoleWithWebIdentity` operation\.  
Used with the `AWS_ROLE_ARN` and `AWS_ROLE_SESSION_NAME` environment variables\.  
If defined, this environment variable overrides the value for the profile setting `web_identity_token_file`\.   
For more information on using web identities, see [Assume role with web identity](cli-configure-role.md#cli-configure-role-oidc)\.  
This environment variable only applies to an assumed role with web identity provider it does not apply to the general assume role provider configuration\.