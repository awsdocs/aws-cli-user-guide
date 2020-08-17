# Using high\-level \(s3\) commands with the AWS CLI<a name="cli-services-s3-commands"></a>

This topic describes how you can manage Amazon S3 buckets and objects using the [https://docs.aws.amazon.com/cli/latest/reference/s3/](https://docs.aws.amazon.com/cli/latest/reference/s3/) commands in the AWS CLI\. 

The high\-level `aws s3` commands simplify managing Amazon S3 objects\. These commands enable you to manage the contents of Amazon S3 within itself and with local directories\.

**Note**  
When you use `aws s3` commands to upload large objects to an Amazon S3 bucket, the AWS CLI automatically performs a multipart upload\. You can't resume a failed upload when using these `aws s3` commands\.   
If the multipart upload fails due to a timeout, or if you manually canceled in the AWS CLI, the AWS CLI stops the upload and cleans up any files that were created\. This process can take several minutes\.   
If the multipart upload or cleanup process is canceled by a kill command or system failure, the created files remain in the Amazon S3 bucket\. To clean up the multipart upload, use the [s3api abort\-multipart\-upload](https://docs.aws.amazon.com/cli/latest/reference/s3api/abort-multipart-upload.html) command\.  
For more information, see [Multipart upload overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html) in the *Amazon Simple Storage Service Developer Guide*\.

**Topics**
+ [Prerequisites](#using-s3-commands-before)
+ [Create a bucket](#using-s3-commands-managing-buckets-creating)
+ [List buckets and objects](#using-s3-commands-listing-buckets)
+ [Delete buckets](#using-s3-commands-delete-buckets)
+ [Delete objects](#using-s3-commands-delete-objects)
+ [Move objects](#using-s3-commands-managing-objects-move)
+ [Copy objects](#using-s3-commands-managing-objects-copy)
+ [Sync objects](#using-s3-commands-managing-objects-sync)
+ [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)
+ [References](#using-s3-commands-managing-buckets-references)

## Prerequisites<a name="using-s3-commands-before"></a>

To run the `s3` commands, you need to:
+ Install and configure the AWS CLI\. For more information, see [Installing the AWS CLI](cli-chap-install.md) and [Configuration basics](cli-configure-quickstart.md)\. 
+ Understand these Amazon S3 terms:
  + **Bucket** – A top\-level Amazon S3 folder\.
  + **Prefix** – An Amazon S3 folder in a bucket\.
  + **Object** – Any item that's hosted in an Amazon S3 bucket\.

## Create a bucket<a name="using-s3-commands-managing-buckets-creating"></a>

Use the [https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html) command to make a bucket\. Bucket names must be ***globally*** unique \(unique across all of Amazon S3\) and should be DNS compliant\. 

Bucket names can contain lowercase letters, numbers, hyphens, and periods\. Bucket names can start and end only with a letter or number, and cannot contain a period next to a hyphen or another period\. 

**Syntax**

```
$ aws s3 mb <target> [--options]
```

### s3 mb examples<a name="using-s3-commands-managing-buckets-creating-examples"></a>

The following example creates the `s3://bucket-name` bucket\.

```
$ aws s3 mb s3://bucket-name
```

## List buckets and objects<a name="using-s3-commands-listing-buckets"></a>

To list your buckets, folders, or objects, use the [https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html) command\. Using the command without a target or options lists all buckets\. 

**Syntax**

```
$ aws s3 ls <target> [--options]
```

For a few common options to use with this command, and examples, see [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)\. For a complete list of available options, see [https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html) in the *AWS CLI Command Reference*\.

### s3 ls examples<a name="using-s3-commands-managing-objects-list-examples"></a>

The following example lists all of your Amazon S3 buckets\.

```
$ aws s3 ls
2018-12-11 17:08:50 my-bucket
2018-12-14 14:55:44 my-bucket2
```

The following command lists all objects and prefixes in a bucket\. In this example output, the prefix `example/` has one file named `MyFile1.txt`\.

```
$ aws s3 ls s3://bucket-name
                           PRE example/
2018-12-04 19:05:48          3 MyFile1.txt
```

You can filter the output to a specific prefix by including it in the command\. The following command lists the objects in *bucket\-name/example/* \(that is, objects in *bucket\-name* filtered by the prefix *example/*\)\.

```
$ aws s3 ls s3://bucket-name/example/
2018-12-06 18:59:32          3 MyFile1.txt
```

## Delete buckets<a name="using-s3-commands-delete-buckets"></a>

To delete a bucket, use the [https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html) command\. 

**Syntax**

```
$ aws s3 rb <target> [--options]
```

### s3 rb examples<a name="using-s3-commands-removing-buckets-examples"></a>

The following example removes the `s3://bucket-name` bucket\.

```
$ aws s3 rb s3://bucket-name
```

By default, the bucket must be empty for the operation to succeed\. To remove a bucket that's not empty, you need to include the `--force` option\. If you're using a versioned bucket that contains previously deleted—but retained—objects, this command does *not* allow you to remove the bucket\. You must first remove all of the content\.

The following example deletes all objects and prefixes in the bucket, and then deletes the bucket\.

```
$ aws s3 rb s3://bucket-name --force
```

## Delete objects<a name="using-s3-commands-delete-objects"></a>

To delete objects in a bucket or your local directory, use the [https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) command\. 

**Syntax**

```
$ aws s3 rm  <target> [--options]
```

For a few common options to use with this command, and examples, see [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)\. For a complete list of options, see [https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) in the *AWS CLI Command Reference*\.

### s3 rm examples<a name="using-s3-commands-delete-objects-examples"></a>

The following example deletes all objects from `s3://bucket-name/example`\.

```
$ aws s3 rm s3://bucket-name/example
```

## Move objects<a name="using-s3-commands-managing-objects-move"></a>

Use the [https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html) command to move objects from a bucket or a local directory\. 

**Syntax**

```
$ aws s3 mv <source> <target> [--options]
```

For a few common options to use with this command, and examples, see [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)\. For a complete list of available options, see [https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html) in the *AWS CLI Command Reference*\.

### s3 mv examples<a name="using-s3-commands-managing-objects-move-examples"></a>

The following example moves all objects from `s3://bucket-name/example` to `s3://my-bucket/`\.

```
$ aws s3 mv s3://bucket-name/example s3://my-bucket/
```

The following example moves a local file from your current working directory to the Amazon S3 bucket with the `s3 cp` command\.

```
$ aws s3 mv filename.txt s3://bucket-name
```

The following example moves a file from your Amazon S3 bucket to your current working directory, where `./` specifies your current working directory\.

```
$ aws s3 mv s3://bucket-name/filename.txt ./
```

## Copy objects<a name="using-s3-commands-managing-objects-copy"></a>

Use the [https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) command to copy objects from a bucket or a local directory\. 

**Syntax**

```
$ aws s3 cp <source> <target> [--options]
```

You can use the dash parameter for file streaming to standard input \(`stdin`\) or standard output \(`stdout`\)\. 

**Warning**  
If you're using PowerShell, the shell might alter the encoding of a CRLF or add a CRLF to piped input or output, or redirected output\.

The `s3 cp` command uses the following syntax to upload a file stream from `stdin` to a specified bucket\.

**Syntax**

```
$ aws s3 cp - <target> [--options]
```

The `s3 cp` command uses the following syntax to download an Amazon S3 file stream for `stdout`\.

**Syntax**

```
$ aws s3 cp <target> [--options] -
```

For a few common options to use with this command, and examples, see [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)\. For the complete list of options, see [https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html) in the *AWS CLI Command Reference*\.

### `s3 cp` examples<a name="using-s3-commands-managing-objects-copy-examples"></a>

The following example copies all objects from `s3://bucket-name/example` to `s3://my-bucket/`\.

```
$ aws s3 cp s3://bucket-name/example s3://my-bucket/
```

The following example copies a local file from your current working directory to the Amazon S3 bucket with the `s3 cp` command\.

```
$ aws s3 cp filename.txt s3://bucket-name
```

The following example copies a file from your Amazon S3 bucket to your current working directory, where `./` specifies your current working directory\.

```
$ aws s3 cp s3://bucket-name/filename.txt ./
```

The following example uses the cat text editor to stream the text "hello world" to the `s3://bucket-name/filename.txt` file\.

```
$ cat "hello world" | aws s3 cp - s3://bucket-name/filename.txt
```

The following example streams the `s3://bucket-name/filename.txt` file to `stdout` and prints the contents to the console\.

```
$ aws s3 cp s3://bucket-name/filename.txt -
hello world
```

The following example streams the contents of `s3://bucket-name/pre` to `stdout`, uses the `bzip2` command to compress the files, and uploads the new compressed file named `key.bz2` to `s3://bucket-name`\.

```
$ aws s3 cp s3://bucket-name/pre - | bzip2 --best | aws s3 cp - s3://bucket-name/key.bz2
```

## Sync objects<a name="using-s3-commands-managing-objects-sync"></a>

The [https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html) command synchronizes the contents of a bucket and a directory, or the contents of two buckets\. Typically, `s3 sync` copies missing or outdated files or objects between the source and target\. However, you can also supply the `--delete` option to remove files or objects from the target that are not present in the source\. 

**Syntax**

```
$ aws s3 sync <source> <target> [--options]
```

For a few common options to use with this command, and examples, see [Frequently used options for s3 commands](#using-s3-commands-managing-objects-param)\. For a complete list of options, see [https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html) in the *AWS CLI Command Reference*\.

### s3 sync examples<a name="using-s3-commands-managing-objects-sync-examples"></a>

The following example synchronizes the contents of an Amazon S3 prefix named *path* in the bucket named *my\-bucket* with the current working directory\. 

`s3 sync` updates any files that have a size or modified time that are different from files with the same name at the destination\. The output displays specific operations performed during the sync\. Notice that the operation recursively synchronizes the subdirectory `MySubdirectory` and its contents with `s3://my-bucket/path/MySubdirectory`\.

```
$ aws s3 sync . s3://my-bucket/path
upload: MySubdirectory\MyFile3.txt to s3://my-bucket/path/MySubdirectory/MyFile3.txt
upload: MyFile2.txt to s3://my-bucket/path/MyFile2.txt
upload: MyFile1.txt to s3://my-bucket/path/MyFile1.txt
```

The following example, which extends the previous one, shows how to use the `--delete` option\.

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

When using the `--delete` option, the `--exclude` and `--include` options can filter files or objects to delete during an `s3 sync` operation\. In this case, the parameter string must specify files to exclude from, or include for, deletion in the context of the target directory or bucket\. The following shows an example\.

```
Assume local directory and s3://my-bucket/path currently in sync and each contains 3 files:
MyFile1.txt
MyFile2.rtf
MyFile88.txt
'''

// Sync with delete, excluding files that match a pattern. MyFile88.txt is deleted, while remote MyFile1.txt is not.
$ aws s3 sync . s3://my-bucket/path --delete --exclude "my-bucket/path/MyFile?.txt"
delete: s3://my-bucket/path/MyFile88.txt
'''

// Sync with delete, excluding MyFile2.rtf - local file is NOT deleted
$ aws s3 sync s3://my-bucket/path . --delete --exclude "./MyFile2.rtf"
download: s3://my-bucket/path/MyFile1.txt to MyFile1.txt
'''

// Sync with delete, local copy of MyFile2.rtf is deleted
$ aws s3 sync s3://my-bucket/path . --delete
delete: MyFile2.rtf
```

## Frequently used options for s3 commands<a name="using-s3-commands-managing-objects-param"></a>

The following options are frequently used for the commands described in this topic\. For a complete list of options you can use on a command, see the specific command in the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/s3/)\.

**acl**  
`s3 sync` and `s3 cp` can use the `--acl` option\. This enables you to set the access permissions for files copied to Amazon S3\. The `--acl` option accepts `private`, `public-read`, and `public-read-write` values\. For more information, see [Canned ACL](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl) in the *Amazon Simple Storage Service Developer Guide*\.  

```
$ aws s3 sync . s3://my-bucket/path --acl public-read
```

**exclude**  
When you use the `s3 cp`, `s3 mv`, `s3 sync`, or `s3 rm` command, you can filter the results by using the `--exclude` or `--include` option\. The `--exclude` option sets rules to only exclude objects from the command, and the options apply in the order specified\. This is shown in the following example\.  

```
Local directory contains 3 files:
MyFile1.txt
MyFile2.rtf
MyFile88.txt

// Exclude all .txt files, resulting in only MyFile2.rtf being copied
$ aws s3 cp . s3://my-bucket/path --exclude "*.txt"

// Exclude all .txt files but include all files with the "MyFile*.txt" format, resulting in, MyFile1.txt, MyFile2.rtf, MyFile88.txt being copied
$ aws s3 cp . s3://my-bucket/path --exclude "*.txt" --include "MyFile*.txt"

// Exclude all .txt files, but include all files with the "MyFile*.txt" format, but exclude all files with the "MyFile?.txt" format resulting in, MyFile2.rtf and MyFile88.txt being copied
$ aws s3 cp . s3://my-bucket/path --exclude "*.txt" --include "MyFile*.txt" --exclude "MyFile?.txt"
```

**include**  
When you use the `s3 cp`, `s3 mv`, `s3 sync`, or `s3 rm` command, you can filter the results using the `--exclude` or `--include` option\. The `--include` option sets rules to only include objects specified for the command, and the options apply in the order specified\. This is shown in the following example\.  

```
Local directory contains 3 files:
MyFile1.txt
MyFile2.rtf
MyFile88.txt

// Exclude all .txt files, resulting in MyFile1.txt and MyFile88.txt being copied
$ aws s3 cp . s3://my-bucket/path --include "*.txt"

// Exclude all .txt files but include all files with the "MyFile*.txt" format, resulting in, MyFile1.txt, MyFile2.rtf, MyFile88.txt being copied
$ aws s3 cp . s3://my-bucket/path --exclude "*.txt" --include "MyFile*.txt"

// Exclude all .txt files, but include all files with the "MyFile*.txt" format, but exclude all files with the "MyFile?.txt" format resulting in, MyFile2.rtf and MyFile88.txt being copied
$ aws s3 cp . s3://my-bucket/path --exclude "*.txt" --include "MyFile*.txt" --exclude "MyFile?.txt"
```

**grant**  
The `s3 cp`, `s3 mv`, and `s3 sync` commands include a `--grants` option that you can use to grant permissions on the object to specified users or groups\. Set the `--grants` option to a list of permissions using the following syntax\. Replace `Permission`, `Grantee_Type`, and `Grantee_ID` with your own values\.  
**Syntax**  

```
--grants Permission=Grantee_Type=Grantee_ID
         [Permission=Grantee_Type=Grantee_ID ...]
```
Each value contains the following elements:  
+ *Permission* – Specifies the granted permissions\. Can be set to `read`, `readacl`, `writeacl`, or `full`\.
+ *Grantee\_Type* – Specifies how to identify the grantee\. Can be set to `uri`, `emailaddress`, or `id`\.
+ *Grantee\_ID* – Specifies the grantee based on *Grantee\_Type*\.
  + `uri` – The group's URI\. For more information, see [Who is a grantee?](https://docs.aws.amazon.com/AmazonS3/latest/dev/ACLOverview.html#SpecifyingGrantee)
  + `emailaddress` – The account's email address\.
  + `id` – The account's canonical ID\.
For more information about Amazon S3 access control, see [Access control](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAuthAccess.html)\.  
The following example copies an object into a bucket\. It grants `read` permissions on the object to everyone, and `full` permissions \(`read`, `readacl`, and `writeacl`\) to the account associated with `user@example.com`\.   

```
$ aws s3 cp file.txt s3://my-bucket/ --grants read=uri=http://acs.amazonaws.com/groups/global/AllUsers full=emailaddress=user@example.com
```
You can also specify a nondefault storage class \(`REDUCED_REDUNDANCY` or `STANDARD_IA`\) for objects that you upload to Amazon S3\. To do this, use the `--storage-class` option\.  

```
$ aws s3 cp file.txt s3://my-bucket/ --storage-class REDUCED_REDUNDANCY
```

**recursive**  
When you use this option, the command is performed on all files or objects under the specified directory or prefix\. The following example deletes `s3://my-bucket/path` and all of its contents\.  

```
$ aws s3 rm s3://my-bucket/path --recursive
```

## References<a name="using-s3-commands-managing-buckets-references"></a>

**AWS CLI reference:**
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/](https://docs.aws.amazon.com/cli/latest/reference/s3/)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mb.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html](https://docs.aws.amazon.com/cli/latest/reference/s3/mv.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html](https://docs.aws.amazon.com/cli/latest/reference/s3/rb.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html)

**Service reference:**
+ [Working with Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) in the *Amazon Simple Storage Service Developer Guide*
+ [Working with Amazon S3 objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html) in the *Amazon Simple Storage Service Developer Guide*
+ [Listing keys hierarchically using a prefix and delimiter](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AmazonS3/latest/dev/ListingKeysHierarchy.html) in the *Amazon Simple Storage Service Developer Guide*
+ [Abort multipart uploads to an S3 bucket using the AWS SDK for \.NET \(low\-level\)](https://docs.aws.amazon.com/https://docs.aws.amazon.com/AmazonS3/latest/dev/LLAbortMPUnet.html) in the *Amazon Simple Storage Service Developer Guide*