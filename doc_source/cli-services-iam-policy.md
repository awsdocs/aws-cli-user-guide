# Attaching an IAM managed policy to an IAM user<a name="cli-services-iam-policy"></a>

This topic describes how to use AWS Command Line Interface \(AWS CLI\) commands to attach an AWS Identity and Access Management \(IAM\) policy to an IAM user\. The policy in this example provides the user with "Power User Access"\. For more information on the IAM service, see the [AWS Identity and Access Management User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\.

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

**To attach an IAM managed policy to an IAM user**

1. Determine the Amazon Resource Name \(ARN\) of the policy to attach\. The following command uses `list-policies` to find the ARN of the policy with the name `PowerUserAccess`\. It then stores that ARN in an environment variable\.

   ```
   $ export POLICYARN=$(aws iam list-policies --query 'Policies[?PolicyName==`PowerUserAccess`].{ARN:Arn}' --output text)       ~
   $ echo $POLICYARN
   arn:aws:iam::aws:policy/PowerUserAccess
   ```

1. To attach the policy, use the [https://docs.aws.amazon.com/cli/latest/reference/iam/attach-user-policy.html](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-user-policy.html) command, and reference the environment variable that holds the policy ARN\.

   ```
   $ aws iam attach-user-policy --user-name MyUser --policy-arn $POLICYARN
   ```

1. Verify that the policy is attached to the user by running the [https://docs.aws.amazon.com/cli/latest/reference/iam/list-attached-user-policies.html](https://docs.aws.amazon.com/cli/latest/reference/iam/list-attached-user-policies.html) command\.

   ```
   $ aws iam list-attached-user-policies --user-name MyUser
   {
       "AttachedPolicies": [
           {
               "PolicyName": "PowerUserAccess",
               "PolicyArn": "arn:aws:iam::aws:policy/PowerUserAccess"
           }
       ]
   }
   ```

For more information, see [Access Management Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/policies-additional-resources.html)\. This topic provides links to an overview of permissions and policies, and links to examples of policies for accessing Amazon S3, Amazon EC2, and other services\.