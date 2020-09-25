# Using Amazon S3 Glacier with the AWS CLI<a name="cli-services-glacier"></a>

You can access the features of Amazon S3 Glacier using the AWS Command Line Interface \(AWS CLI\)\. To list the AWS CLI commands for S3 Glacier, use the following command\.

```
aws glacier help
```

This topic shows examples of AWS CLI commands that perform common tasks for S3 Glacier\. The examples demonstrate how to use the AWS CLI to upload a large file to S3 Glacier by splitting it into smaller parts and uploading them from the command line\.

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

**Note**  
This tutorial uses several command line tools that typically come preinstalled on Unix\-like operating systems, including Linux and macOS\. Windows users can use the same tools by installing [Cygwin](https://www.cygwin.com/) and running the commands from the Cygwin terminal\. We note Windows native commands and utilities that perform the same functions where available\.

**Topics**
+ [Create an Amazon S3 Glacier vault](#cli-services-glacier-vault)
+ [Prepare a file for uploading](#cli-services-glacier-prep)
+ [Initiate a multipart upload and upload files](#cli-services-glacier-initiate)
+ [Complete the upload](#cli-services-glacier-complete)

## Create an Amazon S3 Glacier vault<a name="cli-services-glacier-vault"></a>

Create a vault with the `[create\-vault](https://docs.aws.amazon.com/cli/latest/reference/glacier/create-vault.html)` command\.

```
$ aws glacier create-vault --account-id - --vault-name myvault
{
    "location": "/123456789012/vaults/myvault"
}
```

**Note**  
All S3 Glacier commands require an account ID parameter\. Use the hyphen character \(`--account-id -`\) to use the current account\.

## Prepare a file for uploading<a name="cli-services-glacier-prep"></a>

Create a file for the test upload\. The following commands create a file named *largefile* that contains exactly 3 MiB of random data\.

**Linux or macOS**

```
$ dd if=/dev/urandom of=largefile bs=3145728 count=1
1+0 records in
1+0 records out
3145728 bytes (3.1 MB) copied, 0.205813 s, 15.3 MB/s
```

`dd` is a utility that copies a number of bytes from an input file to an output file\. The previous example uses the system device file `/dev/urandom` as a source of random data\. `fsutil` performs a similar function in Windows\.

**Windows**

```
C:\> fsutil file createnew largefile 3145728
File C:\temp\largefile is created
```

Next, split the file into 1 MiB \(1,048,576 byte\) chunks\.

```
$ split --bytes=1048576 --verbose largefile chunk
creating file `chunkaa'
creating file `chunkab'
creating file `chunkac'
```

**Note**  
[HJ\-Split](http://www.hjsplit.org/) is a free file splitter for Windows and many other platforms\.

## Initiate a multipart upload and upload files<a name="cli-services-glacier-initiate"></a>

Create a multipart upload in Amazon S3 Glacier by using the `[initiate\-multipart\-upload](https://docs.aws.amazon.com/cli/latest/reference/glacier/initiate-multipart-upload.html)` command\.

```
$ aws glacier initiate-multipart-upload --account-id - --archive-description "multipart upload test" --part-size 1048576 --vault-name myvault
{
    "uploadId": "19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ",
    "location": "/123456789012/vaults/myvault/multipart-uploads/19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
}
```

S3 Glacier requires the size of each part in bytes \(1 MiB in this example\), your vault name, and an account ID to configure the multipart upload\. The AWS CLI outputs an upload ID when the operation is complete\. Save the upload ID to a shell variable for later use\.

**Linux or macOS**

```
$ UPLOADID="19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
```

**Windows**

```
C:\> set UPLOADID="19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
```

Next, use the `[upload\-multipart\-part](https://docs.aws.amazon.com/cli/latest/reference/glacier/upload-multipart-part.html)` command to upload each of the three parts\.

```
$ aws glacier upload-multipart-part --upload-id $UPLOADID --body chunkaa --range 'bytes 0-1048575/*' --account-id - --vault-name myvault
{
    "checksum": "e1f2a7cd6e047fa606fe2f0280350f69b9f8cfa602097a9a026360a7edc1f553"
}
$ aws glacier upload-multipart-part --upload-id $UPLOADID --body chunkab --range 'bytes 1048576-2097151/*' --account-id - --vault-name myvault
{
    "checksum": "e1f2a7cd6e047fa606fe2f0280350f69b9f8cfa602097a9a026360a7edc1f553"
}
$ aws glacier upload-multipart-part --upload-id $UPLOADID --body chunkac --range 'bytes 2097152-3145727/*' --account-id - --vault-name myvault
{
    "checksum": "e1f2a7cd6e047fa606fe2f0280350f69b9f8cfa602097a9a026360a7edc1f553"
}
```

**Note**  
The previous example uses the dollar sign \(`$`\) to reference the contents of the `UPLOADID` shell variable on Linux\. On the Windows command line, use a percent sign \(%\) on either side of the variable name \(for example, `%UPLOADID%`\)\.

You must specify the byte range of each part when you upload it so that S3 Glacier can reassemble it in the correct order\. Each piece is 1,048,576 bytes, so the first piece occupies bytes 0\-1048575, the second 1048576\-2097151, and the third 2097152\-3145727\.

## Complete the upload<a name="cli-services-glacier-complete"></a>

Amazon S3 Glacier requires a tree hash of the original file to confirm that all of the uploaded pieces reached AWS intact\. 

To calculate a tree hash, you must split the file into 1 MiB parts and calculate a binary SHA\-256 hash of each piece\. Then you split the list of hashes into pairs, combine the two binary hashes in each pair, and take hashes of the results\. Repeat this process until there is only one hash left\. If there is an odd number of hashes at any level, promote it to the next level without modifying it\.

The key to calculating a tree hash correctly when using command line utilities is to store each hash in binary format and convert to hexadecimal only at the last step\. Combining or hashing the hexadecimal version of any hash in the tree will cause an incorrect result\.

**Note**  
Windows users can use the `type` command in place of `cat`\. OpenSSL is available for Windows at [OpenSSL\.org](https://www.openssl.org/related/binaries.html)\.

**To calculate a tree hash**

1. If you haven't already, split the original file into 1 MiB parts\.

   ```
   $ split --bytes=1048576 --verbose largefile chunk
   creating file `chunkaa'
   creating file `chunkab'
   creating file `chunkac'
   ```

1. Calculate and store the binary SHA\-256 hash of each chunk\.

   ```
   $ openssl dgst -sha256 -binary chunkaa > hash1
   $ openssl dgst -sha256 -binary chunkab > hash2
   $ openssl dgst -sha256 -binary chunkac > hash3
   ```

1. Combine the first two hashes and take the binary hash of the result\.

   ```
   $ cat hash1 hash2 > hash12
   $ openssl dgst -sha256 -binary hash12 > hash12hash
   ```

1. Combine the parent hash of chunks `aa` and `ab` with the hash of chunk `ac` and hash the result, this time outputting hexadecimal\. Store the result in a shell variable\.

   ```
   $ cat hash12hash hash3 > hash123
   $ openssl dgst -sha256 hash123
   SHA256(hash123)= 9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67
   $ TREEHASH=9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67
   ```

Finally, complete the upload with the `[complete\-multipart\-upload](https://docs.aws.amazon.com/cli/latest/reference/glacier/complete-multipart-upload.html)` command\. This command takes the original file's size in bytes, the final tree hash value in hexadecimal, and your account ID and vault name\.

```
$ aws glacier complete-multipart-upload --checksum $TREEHASH --archive-size 3145728 --upload-id $UPLOADID --account-id - --vault-name myvault
{
    "archiveId": "d3AbWhE0YE1m6f_fI1jPG82F8xzbMEEZmrAlLGAAONJAzo5QdP-N83MKqd96Unspoa5H5lItWX-sK8-QS0ZhwsyGiu9-R-kwWUyS1dSBlmgPPWkEbeFfqDSav053rU7FvVLHfRc6hg",
    "checksum": "9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67",
    "location": "/123456789012/vaults/myvault/archives/d3AbWhE0YE1m6f_fI1jPG82F8xzbMEEZmrAlLGAAONJAzo5QdP-N83MKqd96Unspoa5H5lItWX-sK8-QS0ZhwsyGiu9-R-kwWUyS1dSBlmgPPWkEbeFfqDSav053rU7FvVLHfRc6hg"
}
```

You can also check the status of the vault using the `[describe\-vault](https://docs.aws.amazon.com/cli/latest/reference/glacier/describe-vault.html)` command\.

```
$ aws glacier describe-vault --account-id - --vault-name myvault
{
    "SizeInBytes": 3178496,
    "VaultARN": "arn:aws:glacier:us-west-2:123456789012:vaults/myvault",
    "LastInventoryDate": "2018-12-07T00:26:19.028Z",
    "NumberOfArchives": 1,
    "CreationDate": "2018-12-06T21:23:45.708Z",
    "VaultName": "myvault"
}
```

**Note**  
Vault status is updated about once per day\. See [Working with Vaults](https://docs.aws.amazon.com/amazonglacier/latest/dev/working-with-vaults.html) for more information\.

Now it's safe to remove the chunk and hash files that you created\.

```
$ rm chunk* hash*
```

For more information on multipart uploads, see [Uploading Large Archives in Parts](https://docs.aws.amazon.com/amazonglacier/latest/dev/uploading-archive-mpu.html) and [Computing Checksums](https://docs.aws.amazon.com/amazonglacier/latest/dev/checksum-calculations.html) in the *Amazon S3 Glacier Developer Guide*\. 