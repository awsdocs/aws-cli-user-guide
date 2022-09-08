--------

--------

# Enforcing a minimum version of TLS<a name="cli-security-enforcing-tls"></a>

To add increased security when communicating with AWS services, you should use TLS 1\.2 or later\. When you use the AWS CLI, Python is used to set the TLS version\.

AWS CLI version 2 uses an internal Python script that's compiled to use a minimum of TLS 1\.2 when the service it's talking to supports it\. As long as you use version 2 of the AWS CLI, no further steps are needed to enforce this minimum\.