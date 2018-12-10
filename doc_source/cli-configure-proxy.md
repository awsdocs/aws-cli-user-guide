# Using an HTTP Proxy<a name="cli-configure-proxy"></a>

 If you need to access AWS through proxy servers, you can configure the `HTTP_PROXY` and `HTTPS_PROXY` environment variables with the IP addresses and port numbers used by your proxy servers\. 

**Linux, macOS, or Unix**

```
$ export HTTP_PROXY=http://a.b.c.d:n
$ export HTTPS_PROXY=http://w.x.y.z:m
```

**Windows**

```
C:\> set HTTP_PROXY=http://a.b.c.d:n
C:\> set HTTPS_PROXY=http://w.x.y.z:m
```

 In these examples, `http://a.b.c.d:n` and `http://w.x.y.z:m` are the IP addresses and port numbers for the HTTP and HTTPS proxies\. 

## Authenticating to a Proxy<a name="cli-configure-proxy-auth"></a>

The AWS CLI supports HTTP Basic authentication\. Specify the username and password in the proxy URL like this: 

**Linux, macOS, or Unix**

```
$ export HTTP_PROXY=http://username:password@a.b.c.d:n
$ export HTTPS_PROXY=http://username:password@w.x.y.z:m
```

**Windows**

```
C:\> set HTTP_PROXY=http://username:password@a.b.c.d:n
C:\> set HTTPS_PROXY=http://username:password@w.x.y.z:m
```

**Note**  
The AWS CLI does not support NTLM proxies\. If you use an NTLM or Kerberos proxy, you might be able to connect through an authentication proxy like [Cntlm](http://cntlm.sourceforge.net)\.

## Using a proxy on EC2 Instances<a name="cli-configure-proxy-ec2"></a>

If you configure a proxy on an Amazon EC2 instance launched with an attached IAM role, ensure that you exempt the address used to access the [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)\. To do this, set the `NO_PROXY` environment variable to the instance metadata service's IP address 169\.254\.169\.254\. 

**Linux, macOS, or Unix**

```
$ export NO_PROXY=169.254.169.254
```

**Windows**

```
C:\> set NO_PROXY=169.254.169.254
```