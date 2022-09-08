--------

--------

# Data protection in the AWS CLI<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS Command Line Interface\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with AWS CLI or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

## Data encryption<a name="security-data-encryption"></a>

A key feature of any secure service is that information is encrypted when it is not being actively used\.

### Encryption at rest<a name="security-data-encryption-at-rest"></a>

The AWS CLI does not itself store any customer data other than the credentials it needs to interact with the AWS services on the user's behalf\. 

If you use the AWS CLI to invoke an AWS service that transmits customer data to your local computer for storage, then refer to the Security & Compliance chapter in that service's User Guide for information on how that data is stored, protected, and encrypted\.

### Encryption in transit<a name="security-data-encryption-in-transit"></a>

By default, all data transmitted from the client computer running the AWS CLI and AWS service endpoints is encrypted by sending everything through a HTTPS/TLS connection\.

You don't need to do anything to enable the use of HTTPS/TLS\. It is always enabled unless you explicitly disable it for an individual command by using the `--no-verify-ssl` command line option\.