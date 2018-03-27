# Environment Variables<a name="cli-environment"></a>

Environment variables override configuration and credential files and can be useful for scripting or temporarily setting a named profile as the default\.

The AWS CLI supports the following environment variables\.

+ `AWS_ACCESS_KEY_ID` – AWS access key\.

+ `AWS_SECRET_ACCESS_KEY` – AWS secret key\. Access and secret key variables override credentials stored in credential and config files\.

+ `AWS_SESSION_TOKEN` – Specify a session token if you are using temporary security credentials\.

+ `AWS_DEFAULT_REGION` – AWS region\. This variable overrides the default region of the in\-use profile, if set\.

+ `AWS_DEFAULT_OUTPUT` – Change the AWS CLI's output formatting to `json`, `text`, or `table`\.

+ `AWS_PROFILE` – name of the CLI profile to use\. This can be the name of a profile stored in a credential or config file, or `default` to use the default profile\.

+ `AWS_CA_BUNDLE` – Specify the path to a certificate bundle to use for HTTPS certificate validation\.

+ `AWS_SHARED_CREDENTIALS_FILE` – Change the location of the file that the AWS CLI uses to store access keys\.

+ `AWS_CONFIG_FILE` – Change the location of the file that the AWS CLI uses to store configuration profiles\.

The following example shows how you would configure environment variables for the default user from earlier in this guide\. 

**Linux, macOS, or Unix**

```
$ export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
$ export AWS_DEFAULT_REGION=us-west-2
```

**Windows**

```
> setx AWS_ACCESS_KEY_ID AKIAIOSFODNN7EXAMPLE
> setx AWS_SECRET_ACCESS_KEY wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
> setx AWS_DEFAULT_REGION us-west-2
```
