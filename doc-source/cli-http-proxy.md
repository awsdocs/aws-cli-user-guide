# Using an HTTP Proxy<a name="cli-http-proxy"></a>

 If you need to access AWS through proxy servers, you should configure the `HTTP_PROXY` and `HTTPS_PROXY` environment variables with the IP addresses for your proxy servers\. 

**Linux, macOS, or Unix**

```
$ export HTTP_PROXY=http://a.b.c.d:n
$ export HTTPS_PROXY=http://w.x.y.z:m
```

**Windows**

```
> set HTTP_PROXY=http://a.b.c.d:n
> set HTTPS_PROXY=http://w.x.y.z:m
```

 In these examples, `http://a.b.c.d:n` and `http://w.x.y.z:m` are the IP addresses and ports for the HTTP and HTTPS proxies\. 

## Authenticating to a Proxy<a name="cli-http-proxy-auth"></a>

 The AWS CLI supports HTTP Basic authentication\. Specify a username and password in the proxy URL like this: 

**Linux, macOS, or Unix**

```
$ export HTTP_PROXY=http://username:password@a.b.c.d:n
$ export HTTPS_PROXY=http://username:password@w.x.y.z:m
```

**Windows**

```
> set HTTP_PROXY=http://username:password@a.b.c.d:n
> set HTTPS_PROXY=http://username:password@w.x.y.z:m
```

**Note**  
The AWS CLI does not support NTLM proxies\. If you use an NTLM or Kerberos proxy, you may be able to connect through an authentication proxy like [Cntlm](cntlm.sourceforge.net)\.

## Using a proxy on EC2 Instances<a name="cli-http-proxy-ec2"></a>

 If you configure a proxy on an ec2 instance launched with an IAM role, you should also set the `NO_PROXY` environment variable with the IP address 169\.254\.169\.254, so that the AWS CLI can access the [Instance Meta Data Service \(IMDS\)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AESDG-chapter-instancedata.html)\. 

**Linux, macOS, or Unix**

```
$ export NO_PROXY=169.254.169.254
```

**Windows**

```
> set NO_PROXY=169.254.169.254
```