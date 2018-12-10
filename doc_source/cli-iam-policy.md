# Set an IAM Policy for an IAM User<a name="cli-iam-policy"></a>

The following commands show how to assign an IAM policy to an IAM user\. The policy specified here provides the user with "Power User Access"\. This policy is identical to the **Power User Access** policy template provided in the IAM console\. In this example, the policy is saved to a file, `MyPolicyFile.json`:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "NotAction": "iam:*",
      "Resource": "*"
    }
  ]
}
```

To specify the policy, use the `put-user-policy` command\.

```
$ aws iam put-user-policy --user-name MyUser --policy-name MyPowerUserRole --policy-document file://C:\Temp\MyPolicyFile.json
```

Verify the policy has been assigned to the user with the `list-user-policies` command\.

```
$ aws iam list-user-policies --user-name MyUser
{
    "PolicyNames": [
        "MyPowerUserRole"
    ],
    "IsTruncated": "false"
}
```

## Additional Resources<a name="w3aac16c15c11c15"></a>

For more information, see [Resources for Learning About Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies-additional-resources.html)\. This topic provides links to an overview of permissions and policies and links to examples of policies for accessing Amazon S3, Amazon EC2, and other services\.