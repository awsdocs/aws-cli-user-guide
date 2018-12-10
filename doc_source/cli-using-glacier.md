# Using Amazon S3 Glacier with the AWS Command Line Interface<a name="cli-using-glacier"></a>

 You can upload a large file to Glacier by splitting it into smaller parts and uploading them from the command line\. This topic describes the process of creating a vault, splitting a file, and configuring and executing a multipart upload to Glacier with the AWS CLI\. 

**Note**  
This tutorial uses several command line tools that typically come pre\-installed on Unix\-like operating systems including Linux and OS X\. Windows users can use the same tools by installing [Cygwin](https://www.cygwin.com/) and running the commands from the Cygwin terminal\. Windows native commands and utilities that perform the same functions are noted where available\.

**Topics**
+ [Create an Glacier Vault](#cli-using-glacier-vault)
+ [Prepare a File for Uploading](#cli-using-glacier-prep)
+ [Initiate a Multipart Upload and Upload Files](#cli-using-glacier-initiate)
+ [Complete the Upload](#cli-using-glacier-complete)

## Create an Glacier Vault<a name="cli-using-glacier-vault"></a>

Create a vault with the `aws glacier create-vault` command\. The following command creates a vault named `myvault`\.

```
$ aws glacier create-vault --account-id - --vault-name myvault
{
    "location": "/123456789012/vaults/myvault"
}
```

**Note**  
All glacier commands require an account ID parameter\. Use a hyphen to specify the current account\.

## Prepare a File for Uploading<a name="cli-using-glacier-prep"></a>

Create a file for the test upload\. The following commands create a file that contains exactly 3 MiB \(3 x 1024 x 1024 bytes\) of random data\.

**Linux, macOS, or Unix**

```
$ dd if=/dev/urandom of=largefile bs=3145728 count=1
1+0 records in
1+0 records out
3145728 bytes (3.1 MB) copied, 0.205813 s, 15.3 MB/s
```

`dd` is a utility that copies a number of bytes from an input file to an output file\. The above example uses the device file `/dev/urandom` as a source of random data\. `fsutil` performs a similar function in Windows:

**Windows**

```
C:\temp>fsutil file createnew largefile 3145728
File C:\temp\largefile is created
```

Next, split the file into 1 MiB \(1048576 byte\) chunks\.

```
$ split --bytes=1048576 --verbose largefile chunk
creating file `chunkaa'
creating file `chunkab'
creating file `chunkac'
```

**Note**  
[HJ\-Split](http://www.hjsplit.org/) is a free file splitter for Windows and many other platforms\.

## Initiate a Multipart Upload and Upload Files<a name="cli-using-glacier-initiate"></a>

Create a multipart upload in Glacier by using the `aws glacier initiate-multipart-upload` command\.

```
$ aws glacier initiate-multipart-upload --account-id - --archive-description "multipart upload test" --part-size 1048576 --vault-name myvault
{
    "uploadId": "19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ",
    "location": "/123456789012/vaults/myvault/multipart-uploads/19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
}
```

Glacier requires the size of each part in bytes \(1 MiB in this example\), your vault name, and an account ID in order to configure the multipart upload\. The AWS CLI outputs an upload ID when the operation is complete\. Save the upload ID to a shell variable for later use\.

**Linux, macOS, or Unix**

```
$ UPLOADID="19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
```

**Windows**

```
C:\temp> set UPLOADID="19gaRezEXAMPLES6Ry5YYdqthHOC_kGRCT03L9yetr220UmPtBYKk-OssZtLqyFu7sY1_lR7vgFuJV6NtcV5zpsJ"
```

Next, use the `aws glacier upload-multipart-part` command to upload each part\.

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
The above example uses the dollar sign \("$"\) to dereference the `UPLOADID` shell variable\. On the Windows command line, use two percent signs \(i\.e\. `%UPLOADID%`\)\.

You must specify the byte range of each part when you upload it so it can be reassembled in the proper order by Glacier\. Each piece is 1048576 bytes, so the first piece occupies bytes 0\-1048575, the second 1048576\-2097151, and the third 2097152\-3145727\.

## Complete the Upload<a name="cli-using-glacier-complete"></a>

Glacier requires a tree hash of the original file in order to confirm that all of the uploaded pieces reached AWS intact\. To calculate a tree hash, you split the file into 1 MiB parts and calculate a binary SHA\-256 hash of each piece\. Then you split the list of hashes into pairs, combine the two binary hashes in each pair, and take hashes of the results\. Repeat this process until there is only one hash left\. If there is an odd number of hashes at any level, promote it to the next level without modifying it\.

The key to calculating a tree hash correctly when using command line utilities is to store each hash in binary format and only convert to hexadecimal at the last step\. Combining or hashing the hexadecimal version of any hash in the tree will cause an incorrect result\.

**Note**  
Windows users can use the `type` command in place of `cat`\. OpenSSL is available for Windows at [OpenSSL\.org](https://www.openssl.org/related/binaries.html)\.

**To calculate a tree hash**

1. Split the original file into 1 MiB parts if you haven't already\.

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

1. Combine the parent hash of chunks aa and ab with the hash of chunk ac and hash the result, this time outputing hexadecimal\. Store the result in a shell variable\.

   ```
   $ cat hash12hash hash3 > hash123
   $ openssl dgst -sha256 hash123
   SHA256(hash123)= 9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67
   $ TREEHASH=9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67
   ```

Finally, complete the upload with the `aws glacier complete-multipart-upload` command\. This command takes the original file's size in bytes, the final tree hash value in hexadecimal, and your account ID and vault name\.

```
$ aws glacier complete-multipart-upload --checksum $TREEHASH --archive-size 3145728 --upload-id $UPLOADID --account-id - --vault-name myvault
{
    "archiveId": "d3AbWhE0YE1m6f_fI1jPG82F8xzbMEEZmrAlLGAAONJAzo5QdP-N83MKqd96Unspoa5H5lItWX-sK8-QS0ZhwsyGiu9-R-kwWUyS1dSBlmgPPWkEbeFfqDSav053rU7FvVLHfRc6hg",
    "checksum": "9628195fcdbcbbe76cdde932d4646fa7de5f219fb39823836d81f0cc0e18aa67",
    "location": "/123456789012/vaults/myvault/archives/d3AbWhE0YE1m6f_fI1jPG82F8xzbMEEZmrAlLGAAONJAzo5QdP-N83MKqd96Unspoa5H5lItWX-sK8-QS0ZhwsyGiu9-R-kwWUyS1dSBlmgPPWkEbeFfqDSav053rU7FvVLHfRc6hg"
}
```

You can also check the status of the vault using `aws glacier describe-vault`:

```
$ aws glacier describe-vault --account-id - --vault-name myvault
{
    "SizeInBytes": 3178496,
    "VaultARN": "arn:aws:glacier:us-west-2:123456789012:vaults/myvault",
    "LastInventoryDate": "2015-04-07T00:26:19.028Z",
    "NumberOfArchives": 1,
    "CreationDate": "2015-04-06T21:23:45.708Z",
    "VaultName": "myvault"
}
```

**Note**  
Vault status is updated about once per day\. See [Working with Vaults](https://docs.aws.amazon.com/amazonglacier/latest/dev/working-with-vaults.html) for more information

It is now safe to remove the part and hash files you created:

```
$ rm chunk* hash*
```

For more information on multipart uploads, see [Uploading Large Archives in Parts](https://docs.aws.amazon.com/amazonglacier/latest/dev/uploading-archive-mpu.html) and [Computing Checksums](https://docs.aws.amazon.com/amazonglacier/latest/dev/checksum-calculations.html) in the Amazon S3 Glacier Developer Guide\. 