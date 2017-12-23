# Create New IAM Users and Groups<a name="cli-iam-new-user-group"></a>

This section describes how to create a new IAM group and a new IAM user and then add the user to the group\.

**To create an IAM group and add a new IAM user to it**

1. First, use the `create-group` command to create the group\.

   ```
   $ aws iam create-group --group-name MyIamGroup
   {
       "Group": {
           "GroupName": "MyIamGroup",
           "CreateDate": "2012-12-20T03:03:52.834Z",
           "GroupId": "AKIAI44QH8DHBEXAMPLE",
           "Arn": "arn:aws:iam::123456789012:group/MyIamGroup",
           "Path": "/"
       }
   }
   ```

1. Next, use the `create-user` command to create the user\.

   ```
   $ aws iam create-user --user-name MyUser
   {
       "User": {
           "UserName": "MyUser",
           "Path": "/",
           "CreateDate": "2012-12-20T03:13:02.581Z",
           "UserId": "AKIAIOSFODNN7EXAMPLE",
           "Arn": "arn:aws:iam::123456789012:user/MyUser"
       }
   }
   ```

1. Finally, use the `add-user-to-group` command to add the user to the group\.

   ```
   $ aws iam add-user-to-group --user-name MyUser --group-name MyIamGroup
   ```

1. To verify that the `MyIamGroup` group contains the `MyUser`, use the `get-group` command\.

   ```
   $ aws iam get-group --group-name MyIamGroup
   {
       "Group": {
           "GroupName": "MyIamGroup",
           "CreateDate": "2012-12-20T03:03:52Z",
           "GroupId": "AKIAI44QH8DHBEXAMPLE",
           "Arn": "arn:aws:iam::123456789012:group/MyIamGroup",
           "Path": "/"
       },
       "Users": [
           {
               "UserName": "MyUser",
               "Path": "/",
               "CreateDate": "2012-12-20T03:13:02Z",
               "UserId": "AKIAIOSFODNN7EXAMPLE",
               "Arn": "arn:aws:iam::123456789012:user/MyUser"
           }
       ],
       "IsTruncated": "false"
   }
   ```

You can also view IAM users and groups with the AWS Management Console\.