# Sourcing credentials with an external process<a name="cli-configure-sourcing-external"></a>

**Warning**  
This topic discusses sourcing credentials from an external process\. This could be a security risk if the command to generate the credentials becomes accessible by non\-approved processes or users\. We recommend that you use the supported, secure alternatives provided by the AWS CLI and AWS to reduce the risk of compromising your credentials\. Ensure that you secure the `config` file and any supporting files and tools to prevent disclosure\.  
Ensure that your custom credential tool does not write any secret information to `StdErr` because the SDKs and CLI can capture and log such information, potentially exposing it to unauthorized users\.

If you have a method to generate or look up credentials that isn't directly supported by the AWS CLI, you can configure the CLI to use it by configuring the `credential_process` setting in the `config` file\. 

For example, you might include an entry similar to the following in the `config` file\.

```
[profile developer]
credential_process = /opt/bin/awscreds-custom --username helen
```

**Syntax**  
To create this string in a way that is compatible with any operating system, follow these rules:
+ If the path or file name contains a space, surround the complete path and file name with double\-quotation marks \(" "\)\. The path and file name can consist of only the characters: A\-Z a\-z 0\-9 \- \_ \. space
+ If a parameter name or a parameter value contains a space, surround that element with double\-quotation marks \(" "\)\. Surround only the name or value, not the pair\.
+ Do not include any environment variables in the strings\. For example, you can't include `$HOME` or `%USERPROFILE%`\.
+ Do not specify the home folder as `~`\. You must specify the full path\.

**Example for Windows**

```
credential_process = "C:\Path\To\credentials.cmd" parameterWithoutSpaces "parameter with spaces"
```

**Example for Linux or macOS**

```
credential_process = "/Users/Dave/path/to/credentials.sh" parameterWithoutSpaces "parameter with spaces"
```

**Expected output from the Credentials program**

The AWS CLI runs the command as specified in the profile and then reads data from `STDOUT`\. The command you specify must generate JSON output on `STDOUT` that matches the following syntax\.

```
{
  "Version": 1,
  "AccessKeyId": "an AWS access key",
  "SecretAccessKey": "your AWS secret access key",
  "SessionToken": "the AWS session token for temporary credentials", 
  "Expiration": "ISO8601 timestamp when the credentials expire"
}
```

**Note**  
As of this writing, the `Version` key must be set to `1`\. This might increment over time as the structure evolves\.

The `Expiration` key is an [ISO8601](https://wikipedia.org/wiki/ISO_8601) formatted timestamp\. If the `Expiration` key is not present in the tool's output, the CLI assumes that the credentials are long\-term credentials that do not refresh\. Otherwise the credentials are considered temporary credentials and are refreshed automatically by rerunning the `credential_process` command before they expire\.

**Note**  
The AWS CLI does ***not*** cache external process credentials the way it does assume\-role credentials\. If caching is required, you must implement it in the external process\.

The external process can return a non\-zero return code to indicate that an error occurred while retrieving the credentials\.