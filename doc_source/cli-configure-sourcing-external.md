# Sourcing Credentials with an External Process<a name="cli-configure-sourcing-external"></a>

**Warning**  
The following topic discusses sourcing credentials from an external process\. This could be a security risk if the command to generate the credentials became accessible by non\-approved processes or users\. We recommend that you use the supported, secure alternatives provided by the CLI and AWS to reduce the risk of compromising your credentials\. Ensure that you secure the `config` file and any supporting files and tools to prevent disclosure\.  
Ensure that your custom credential tool does not write any secret information to StdErr because the SDKs and CLI can capture and log such information, potentially exposing it to unauthorized users\.

If you have a method to generate or lookup credentials that isn't directly supported by the AWS CLI, you can configure the CLI to use it by configuring the `credential_process` setting in the `config` file\. 

For example, you might include an entry similar to the following in the config file:

```
[profile developer]
credential_process = /opt/bin/awscreds-custom --username helen
```

The AWS CLI runs the command exactly as specified in the profile then reads data from STDOUT\. The command you specify must then generate JSON output on STDOUT that matches the following syntax:

```
{
  "Version": 1,
  "AccessKeyId": "an AWS access key",
  "SecretAccessKey": "your AWS secret access key",
  "SessionToken": "the AWS session token for temporary credentials", 
  "Expiration": "ISO8601 timestamp when the credentials expire"
}
```

As of this writing, the `Version` key must be set to `1`\. This might increment over time as the structure evolves\.

The `Expiration` key is an [ISO8601](https://wikipedia.org/wiki/ISO_8601) formatted timestamp\. If the `Expiration` key is not present in the tool's output, the CLI assumes that the credentials are long term credentials that do not refresh\. Otherwise the credentials are considered temporary credentials and are refreshed automatically by re\-running the `credential_process` command before they expire\.

**Note**  
The AWS CLI does ***not*** cache external process credentials the way it does assume\-role credentials\. If caching is required, then you must implement it in the external process\.

The external process can return a non\-zero return code to indicate that an error occurred while retrieving the credentials\.