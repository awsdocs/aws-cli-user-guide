--------

--------

# Using an HTTP proxy<a name="cli-configure-proxy"></a>

 To access AWS through proxy servers, you can configure the `HTTP_PROXY` and `HTTPS_PROXY` environment variables with either the DNS domain names or IP addresses and port numbers that your proxy servers use\.

**Topics**
+ [Using the examples](#cli-configure-proxy-using)
+ [Authenticating to a proxy](#cli-configure-proxy-auth)
+ [Using a proxy on Amazon EC2 instances](#cli-configure-proxy-ec2)

## Using the examples<a name="cli-configure-proxy-using"></a>

**Note**  
The following examples show the environment variable name in all uppercase letters\. However, if you specify a variable twice using different cases, the lowercase letters take precedence\. We recommend that you define each variable only once to avoid system confusion and unexpected behavior\.

The following examples show how you can use either the explicit IP address of your proxy or a DNS name that resolves to the IP address of your proxy\. Either can be followed by a colon and the port number to which queries should be sent\.

------
#### [ Linux or macOS ]

```
$ export HTTP_PROXY=http://10.15.20.25:1234
$ export HTTP_PROXY=http://proxy.example.com:1234
$ export HTTPS_PROXY=http://10.15.20.25:5678
$ export HTTPS_PROXY=http://proxy.example.com:5678
```

------
#### [ Windows Command Prompt ]

**To set for all sessions**

```
C:\> setx HTTP_PROXY http://10.15.20.25:1234
C:\> setx HTTP_PROXY http://proxy.example.com:1234
C:\> setx HTTPS_PROXY http://10.15.20.25:5678
C:\> setx HTTPS_PROXY http://proxy.example.com:5678
```

Using [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/setx) to set an environment variable changes the value used in both the current command prompt session and all command prompt sessions that you create after running the command\. It does ***not*** affect other command shells that are already running at the time you run the command\.

**To set for current session only**

Using `[set](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/set_1)` to set an environment variable changes the value used until the end of the current command prompt session, or until you set the variable to a different value\. 

```
C:\> set HTTP_PROXY=http://10.15.20.25:1234
C:\> set HTTP_PROXY=http://proxy.example.com:1234
C:\> set HTTPS_PROXY=http://10.15.20.25:5678
C:\> set HTTPS_PROXY=http://proxy.example.com:5678
```

------

## Authenticating to a proxy<a name="cli-configure-proxy-auth"></a>

**Note**  
The AWS CLI doesn't support NTLM proxies\. If you use an NTLM or Kerberos protocol proxy, you might be able to connect through an authentication proxy like [Cntlm](http://cntlm.sourceforge.net)\.

The AWS CLI supports HTTP Basic authentication\. Specify the user name and password in the proxy URL, as follows\. 

------
#### [ Linux or macOS ]

```
$ export HTTP_PROXY=http://username:password@proxy.example.com:1234
$ export HTTPS_PROXY=http://username:password@proxy.example.com:5678
```

------
#### [ Windows Command Prompt ]

**To set for all sessions**

```
C:\> setx HTTP_PROXY http://username:password@proxy.example.com:1234
C:\> setx HTTPS_PROXY http://username:password@proxy.example.com:5678
```

**To set for current session only**

```
C:\> set HTTP_PROXY=http://username:password@proxy.example.com:1234
C:\> set HTTPS_PROXY=http://username:password@proxy.example.com:5678
```

------

## Using a proxy on Amazon EC2 instances<a name="cli-configure-proxy-ec2"></a>

If you configure a proxy on an Amazon EC2 instance launched with an attached IAM role, ensure that you exempt the address used to access the [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\. To do this, set the `NO_PROXY` environment variable to the IP address of the instance metadata service, 169\.254\.169\.254\. This address does not vary\.

------
#### [ Linux or macOS ]

```
$ export NO_PROXY=169.254.169.254
```

------
#### [ Windows Command Prompt ]

**To set for all sessions**

```
C:\> setx NO_PROXY 169.254.169.254
```

**To set for current session only**

```
C:\> set NO_PROXY=169.254.169.254
```

------