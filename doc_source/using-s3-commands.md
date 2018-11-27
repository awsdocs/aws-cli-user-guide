# Using High\-Level s3 Commands with the AWS Command Line Interface<a name="using-s3-commands"></a>

This section describes how you can manage Amazon S3 buckets and objects using high\-level `aws s3` commands\.

## Managing Buckets<a name="using-s3-commands-managing-buckets"></a>

High\-level `aws s3` commands support commonly used bucket operations, such as creating, removing, and listing buckets\.

### Creating Buckets<a name="using-s3-commands-managing-buckets-creating"></a>

Use the `aws s3 mb` command to create a new bucket\. Bucket names must be unique and should be DNS compliant\. Bucket names can contain lowercase letters, numbers, hyphens and periods\. Bucket names can only start and end with a letter or number, and cannot contain a period next to a hyphen or another period\. 

```
$ aws s3 mb s3://bucket-name
```

### Removing Buckets<a name="using-s3-commands-removing-buckets"></a>

To remove a bucket, use the `aws s3 rb` command\.

```
$ aws s3 rb s3://bucket-name
```

By default, the bucket must be empty for the operation to succeed\. To remove a non\-empty bucket, you need to include the `--force` option\.

```
$ aws s3 rb s3://bucket-name --force
```

This will first delete all objects and subfolders in the bucket and then remove the bucket\.

**Note**  
If you are using a versioned bucket that contains previously deleted—but retained—objects, this command will *not* allow you to remove the bucket\.

### Listing Buckets<a name="using-s3-commands-listing-buckets"></a>

To list all buckets or their contents, use the `aws s3 ls` command\. Here are some examples of common usage\.

The following command lists all buckets\.

```
$ aws s3 ls
2013-07-11 17:08:50 my-bucket
2013-07-24 14:55:44 my-bucket2
```

The following command lists all objects and folders \(prefixes\) in a bucket\.

```
$ aws s3 ls s3://bucket-name
                           PRE path/
2013-09-04 19:05:48          3 MyFile1.txt
```

The following command lists the objects in *bucket\-name*/`path` \(in other words, objects in *bucket\-name* filtered by the prefix `path/`\)\.

```
$ aws s3 ls s3://bucket-name/path/
2013-09-06 18:59:32          3 MyFile2.txt
```

## Managing Objects<a name="using-s3-commands-managing-objects"></a>

The high\-level `aws s3` commands make it convenient to manage Amazon S3 objects as well\. The object commands include `aws s3 cp`, `aws s3 ls`, `aws s3 mv`, `aws s3 rm`, and `sync`\. The `cp`, `ls`, `mv`, and `rm` commands work similarly to their Unix counterparts and enable you to work seamlessly across your local directories and Amazon S3 buckets\. The `sync` command synchronizes the contents of a bucket and a directory, or two buckets\.

**Note**  
All high\-level commands that involve uploading objects into an Amazon S3 bucket \(`aws s3 cp`, `aws s3 mv`, and `aws s3 sync`\) automatically perform a multipart upload when the object is large\.  
Failed uploads cannot be resumed when using these commands\. If the multipart upload fails due to a timeout or is manually cancelled by pressing CTRL\+C, the AWS CLI cleans up any files created and aborts the upload\. This process can take several minutes\.   
If the process is interrupted by a kill command or system failure, the in\-progress multipart upload remains in Amazon S3 and must be cleaned up manually in the AWS Management Console or with the [s3api abort\-multipart\-upload](https://docs.aws.amazon.com/cli/latest/reference/s3api/abort-multipart-upload.html) command\.

The `cp`, `mv`, and `sync` commands include a `--grants` option that can be used to grant permissions on the object to specified users or groups\. You set the `--grants` option to a list of permissions using following syntax:

```
--grants Permission=Grantee_Type=Grantee_ID
         [Permission=Grantee_Type=Grantee_ID ...]
```

Each value contains the following elements:
+ *Permission* – Specifies the granted permissions, and can be set to `read`, `readacl`, `writeacl`, or `full`\.
+ *Grantee\_Type* – Specifies how the grantee is to be identified, and can be set to `uri`, `emailaddress`, or `id`\.
+ *Grantee\_ID* – Specifies the grantee based on *Grantee\_Type*\.
  + `uri` – The group's URI\. For more information, see [Who Is a Grantee?](https://docs.aws.amazon.com/AmazonS3/latest/dev/ACLOverview.html#SpecifyingGrantee)
  + `emailaddress` – The account's email address\.
  + `id` – The account's canonical ID\.

For more information on Amazon S3 access control, see [Access Control](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAuthAccess.html)\.

The following example copies an object into a bucket\. It grants `read` permissions on the object to everyone and `full` permissions \(`read`, `readacl`, and `writeacl`\) to the account associated with `user@example.com`\. 

```
$ aws s3 cp file.txt s3://my-bucket/ --grants read=uri=http://acs.amazonaws.com/groups/global/AllUsers full=emailaddress=user@example.com
```

To specify a non\-default storage class \(`REDUCED_REDUNDANCY` or `STANDARD_IA`\) for objects that you upload to Amazon S3, use the `--storage-class` option:

```
$ aws s3 cp file.txt s3://my-bucket/ --storage-class REDUCED_REDUNDANCY
```

The `sync` command has the following form\. Possible source\-target combinations are:
+ Local file system to Amazon S3
+ Amazon S3 to local file system
+ Amazon S3 to Amazon S3

```
$ aws s3 sync <source> <target> [--options]
```

The following example synchronizes the contents of an Amazon S3 folder named *path* in *my\-bucket* with the current working directory\. `s3 sync` updates any files that have a different size or modified time than files with the same name at the destination\. The output displays specific operations performed during the sync\. Notice that the operation recursively synchronizes the subdirectory *MySubdirectory* and its contents with *s3://my\-bucket/path/MySubdirectory*\.

```
$ aws s3 sync . s3://my-bucket/path
upload: MySubdirectory\MyFile3.txt to s3://my-bucket/path/MySubdirectory/MyFile3.txt
upload: MyFile2.txt to s3://my-bucket/path/MyFile2.txt
upload: MyFile1.txt to s3://my-bucket/path/MyFile1.txt
```

Normally, `sync` only copies missing or outdated files or objects between the source and target\. However, you may supply the `--delete` option to remove files or objects from the target not present in the source\.

The following example, which extends the previous one, shows how this works\.

```
// Delete local file
$ rm ./MyFile1.txt

// Attempt sync without --delete option - nothing happens
$ aws s3 sync . s3://my-bucket/path

// Sync with deletion - object is deleted from bucket
$ aws s3 sync . s3://my-bucket/path --delete
delete: s3://my-bucket/path/MyFile1.txt

// Delete object from bucket
$ aws s3 rm s3://my-bucket/path/MySubdirectory/MyFile3.txt
delete: s3://my-bucket/path/MySubdirectory/MyFile3.txt

// Sync with deletion - local file is deleted
$ aws s3 sync s3://my-bucket/path . --delete
delete: MySubdirectory\MyFile3.txt

// Sync with Infrequent Access storage class
$ aws s3 sync . s3://my-bucket/path --storage-class STANDARD_IA
```

The `--exclude` and `--include` options allow you to specify rules to filter the files or objects to be copied during the sync operation\. By default, all items in a specified directory are included in the sync\. Therefore, `--include` is only needed when specifying exceptions to the `--exclude` option \(for example, `--include` effectively means "don't exclude"\)\. The options apply in the order that is specified, as demonstrated in the following example\.

```
Local directory contains 3 files:
MyFile1.txt
MyFile2.rtf
MyFile88.txt
'''
$ aws s3 sync . s3://my-bucket/path --exclude '*.txt'
upload: MyFile2.rtf to s3://my-bucket/path/MyFile2.rtf
'''
$ aws s3 sync . s3://my-bucket/path --exclude '*.txt' --include 'MyFile*.txt'
upload: MyFile1.txt to s3://my-bucket/path/MyFile1.txt
upload: MyFile88.txt to s3://my-bucket/path/MyFile88.txt
upload: MyFile2.rtf to s3://my-bucket/path/MyFile2.rtf
'''
$ aws s3 sync . s3://my-bucket/path --exclude '*.txt' --include 'MyFile*.txt' --exclude 'MyFile?.txt'
upload: MyFile2.rtf to s3://my-bucket/path/MyFile2.rtf
upload: MyFile88.txt to s3://my-bucket/path/MyFile88.txt
```

The `--exclude` and `--include` options can also filter files or objects to be deleted during a sync operation with the `--delete` option\. In this case, the parameter string must specify files to be excluded from, or included for, deletion in the context of the target directory or bucket\. The following shows an example\.

```
Assume local directory and s3://my-bucket/path currently in sync and each contains 3 files:
MyFile1.txt
MyFile2.rtf
MyFile88.txt
'''
// Delete local .txt files
$ rm *.txt

// Sync with delete, excluding files that match a pattern. MyFile88.txt is deleted, while remote MyFile1.txt is not.
$ aws s3 sync . s3://my-bucket/path --delete --exclude 'my-bucket/path/MyFile?.txt'
delete: s3://my-bucket/path/MyFile88.txt
'''
// Delete MyFile2.rtf
$ aws s3 rm s3://my-bucket/path/MyFile2.rtf

// Sync with delete, excluding MyFile2.rtf - local file is NOT deleted
$ aws s3 sync s3://my-bucket/path . --delete --exclude './MyFile2.rtf'
download: s3://my-bucket/path/MyFile1.txt to MyFile1.txt
'''
// Sync with delete, local copy of MyFile2.rtf is deleted
$ aws s3 sync s3://my-bucket/path . --delete
delete: MyFile2.rtf
```

The `sync` command also accepts an `--acl` option, by which you may set the access permissions for files copied to Amazon S3\. The option accepts `private`, `public-read`, and `public-read-write` values\.

```
$ aws s3 sync . s3://my-bucket/path --acl public-read
```

As previously mentioned, the `s3` command set includes `cp`, `mv`, `ls`, and `rm`, and they work in similar ways to their Unix counterparts\. The following are some examples\.

```
// Copy MyFile.txt in current directory to s3://my-bucket/path
$ aws s3 cp MyFile.txt s3://my-bucket/path/

// Move all .jpg files in s3://my-bucket/path to ./MyDirectory
$ aws s3 mv s3://my-bucket/path ./MyDirectory --exclude '*' --include '*.jpg' --recursive

// List the contents of my-bucket
$ aws s3 ls s3://my-bucket

// List the contents of path in my-bucket
$ aws s3 ls s3://my-bucket/path/

// Delete s3://my-bucket/path/MyFile.txt
$ aws s3 rm s3://my-bucket/path/MyFile.txt

// Delete s3://my-bucket/path and all of its contents
$ aws s3 rm s3://my-bucket/path --recursive
```

When the `--recursive` option is used on a directory/folder with `cp`, `mv`, or `rm`, the command walks the directory tree, including all subdirectories\. These commands also accept the `--exclude`, `--include`, and `--acl` options as the `sync` command does\.