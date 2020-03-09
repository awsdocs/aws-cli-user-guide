# Enforcing a TLS 1\.2 Minimum<a name="cli-security-enforcing-tls"></a>

To add increased security when communicating with AWS services, you should configure your AWS Command Line Interface to use TLS 1\.2 or later\. When you use the the AWS CLI, Python is used to set the TLS version\.

Based on your AWS CLI version, the steps you perform to enforce a TLS minimum of 1\.2 varies\.

**Topics**
+ [Configuring the AWS CLI version 1 to Enforce a TLS 1\.2 Minimum](#enforcing-tls-v1)
+ [Configuring the AWS CLI version 2 to Enforce a TLS 1\.2 Minimum](#enforcing-tls-v2)

## Configuring the AWS CLI version 1 to Enforce a TLS 1\.2 Minimum<a name="enforcing-tls-v1"></a>

In order to ensure the AWS CLI version 1 uses no lower than TLS 1\.2, you may need to recompile OpenSSL to enforce this minimum and then recompile Python to use the newly built OpenSSL\. 

### Determine Your Currently Supported Protocols<a name="enforcing-tls-supported"></a>

First create a self\-signed certificate to use for the test server and the SDK using OpenSSL:

```
$ openssl req -subj '/CN=localhost' -x509 -newkey rsa:4096 -nodes -keyout key.pem -out cert.pem -days 365
```

Then spin up a test server using OpenSSL:

```
$ openssl s_server -key key.pem -cert cert.pem -www
```

In a new terminal screen, create a virtual environment and install the SDK:

```
$ python3 -m venv test-env
source test-env/bin/activate
pip install botocore
```

Create a new Python script called check\.py that uses the SDK’s underlying HTTP library:

```
$ import urllib3
URL = 'https://localhost:4433/'

http = urllib3.PoolManager(
    ca_certs='cert.pem',
    cert_reqs='CERT_REQUIRED',
)
r = http.request('GET', URL)
print(r.data.decode('utf-8'))
```

Run your new script:

```
$ python check.py
```

This displays details about the connection made\. Search for "Protocol : " in the output\. If the output is "TLSv1\.2" or higher, the SDK defaults to TLS v1\.2 and higher\. If it is lower, you need to recompile OpenSSL and recompile Python\.

However, even if your installation of Python defaults to TLS v1\.2 or higher, it is still possible for Python to renegotiate to a version lower than TLS v1\.2 if the server does not support TLS v1\.2\+\. To check that Python does not automatically renegotiate to lower versions, restart the test server with the following:

```
$ openssl s_server -key key.pem -cert cert.pem -no_tls1_3 -no_tls1_2 -www
```

Note if you are using an older version of OpenSSL, you may not have the `-no_tls_3` flag available\. If this is the case, remove the flag because the version of OpenSSL you are using does not support TLS v1\.3\. Then rerun the Python script:

```
$ python check.py
```

If your installation of Python correctly does not renegotiate for versions lower than TLS 1\.2, you should receive an SSL error:

```
$ urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='localhost', port=4433): Max retries exceeded with url: / (Caused by SSLError(SSLError(1, '[SSL: UNSUPPORTED_PROTOCOL] unsupported protocol (_ssl.c:1108)')))
```

If you are able to make a connection, then you need to recompile OpenSSL and Python to disable negotiation of protocols lower than TLS v1\.2\.

### Compile OpenSSL and Python<a name="enforcing-tls-compile"></a>

 In order to ensure the SDK or CLI does not negotiate for anything lower than TLS 1\.2, you need to recompile OpenSSL and Python\. First copy the content below to create a script and run it:

```
#!/usr/bin/env bash
set -e

OPENSSL_VERSION="1.1.1d"
OPENSSL_PREFIX="/opt/openssl-with-min-tls1_2"
PYTHON_VERSION="3.8.1"
PYTHON_PREFIX="/opt/python-with-min-tls1_2"


curl -O "https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz"
tar -xzf "openssl-$OPENSSL_VERSION.tar.gz"
cd openssl-$OPENSSL_VERSION
./config --prefix=$OPENSSL_PREFIX no-ssl3 no-tls1 no-tls1_1 no-shared
make > /dev/null
sudo make install_sw > /dev/null


cd /tmp
curl -O "https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tgz"
tar -xzf "Python-$PYTHON_VERSION.tgz"
cd Python-$PYTHON_VERSION
./configure --prefix=$PYTHON_PREFIX --with-openssl=$OPENSSL_PREFIX --disable-shared > /dev/null
make > /dev/null
sudo make install > /dev/null
```

This compiles a version of Python that has a statically linked OpenSSL that does not automatically negotiate anything below TLS 1\.2\. This also installs OpenSSL in the `/opt/openssl-with-min-tls1_2` directory and install Python in the `/opt/python-with-min-tls1_2` directory\. Once you run this script, confirm installation of the new version of Python:

```
$ /opt/python-with-min-tls1_2/bin/python3 --version
```

This should print out:

```
$ Python 3.8.1
```

To confirm this new version of Python does not negotiate lower than TLS 1\.2, rerun the steps from “Determine protocols supported” using the newly installed Python version \(i\.e\. /opt/python\-with\-min\-tls1\_2/bin/python3\)\.

## Configuring the AWS CLI version 2 to Enforce a TLS 1\.2 Minimum<a name="enforcing-tls-v2"></a>

AWS CLI version 2 uses an internal Python script that is compiled to use a minimum of TLS 1\.2 when the service it's talking to supports it\. No further steps are needed to enforce this minimum\.