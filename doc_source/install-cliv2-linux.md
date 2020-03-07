# Installing the AWS CLI version 2 on Linux<a name="install-cliv2-linux"></a>

This section describes how to install, upgrade, and remove the AWS CLI version 2 on Linux\.

**Topics**
+ [Prerequisites](#cliv2-linux-prereq)
+ [Installing](#cliv2-linux-install)
+ [Upgrading](#cliv2-linux-upgrade)
+ [Uninstalling](#cliv2-linux-remove)
+ [Verifying the Integrity and Authenticity of the Downloaded Files](#v2-install-linux-validate)

## Prerequisites<a name="cliv2-linux-prereq"></a>
+ The AWS CLI version 2 has no dependencies on other software packages\. It has a self\-contained, embedded copy of all dependencies included in the installer\. You no longer need to install and maintain Python to use the AWS CLI\.
+ You must be able to "unzip" the downloaded package\. If your operating system doesn't have a built\-in `unzip` command, use your favorite package manager to download it or an equivalent\.
+ ****We support the AWS CLI version 2 on recent distributions of CentOS, Fedora, Ubuntu, Amazon Linux 1, and Amazon Linux 2\.

## Installing<a name="cliv2-linux-install"></a>

Follow these steps from the command line to install the AWS CLI on Linux\. The only difference in the following commands is the name of the file that you download\. Everything else is the same\.

**Important**  
Ensure that the paths you install to contain no volume or folder names that contain any spaces or the installation fails\.

We provide the steps in one easy to copy and paste group\. See the descriptions of each line in the steps that follow\. 

You can verify that integrity and authenticity of the installation file after you download it and before you extract the files from the package\. For more information, see [Verifying the Integrity and Authenticity of the Downloaded Files](#v2-install-linux-validate)\.

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

1. You can download the file using the `curl` command\. The options on the following example command cause the downloaded file to be written to the current directory with the local name `awscliv2.zip`\.

   ```
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```

   In this example, the `-o` option specifies the file name that the downloaded package is written to\. In the previous example, the file is written to `awscliv2.zip` in the current folder\.

   Alternatively, you can use your browser to download the installer from the following URL: `https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip`

   In this example the latest version of the CLI is downloaded\. A version can be specified by appending it just before the file extension: `https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.x.y.zip`

   You can verify the integrity and authenticity of the installation file after you download it\. For more information, see [Verifying the Integrity and Authenticity of the Downloaded Files](#v2-install-linux-validate) before you unzip the package\.

1. Unzip the installer\. The following example command unzips the package to the current folder\. If your Linux distribution doesn't have a built\-in `unzip` command, use your favorite package manager, or an equivalent, to install it\.

   ```
   $ unzip awscliv2.zip
   ```

   This creates a folder named `aws` under the current folder\.

1. Run the install program\.

   ```
   $ sudo ./aws/install
   ```

   The installation command is a file named `install` found in the newly unzipped `aws` folder\. By default, the files are all installed to `/usr/local/aws`, and a symlink is created in `/usr/local/bin`\. The command includes `sudo` to grant write permissions to those folders\. You can install without `sudo` if you specify folders that you already have write permissions to\. 

   You can use the following parameters with the `install` command to specify those folders:
**Important**  
Ensure that the paths you provide to the `-i` and `-b` parameters contain no volume name or folder names that contain any space characters or other white space characters\. If there is a space, the installation fails\.
   + `--install-dir` or `-i`

     This option specifies the folder to copy all of the files to\. This example installs the files to a folder named `/usr/local/aws-cli`\. You must have write permissions to `/usr/local` to create this folder\. 

     The default value is `/usr/local/aws-cli`\.
   + `--bin-dir` or `-b`

     This option specifies that the main `aws` program in the install folder is symlinked to the file `aws` in the specified path\. This example creates the symlink `/usr/local/bin/aws`\. You must have write permissions to the specified folder\. Creating a symlink to a folder that is already in your path eliminates the need to add the install directory to the user's `$PATH` variable\. 

     The default value is `/usr/local/bin`\.

1. Confirm the installation\.

   ```
   $ aws --version
   aws-cli/2.0.0 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.0.0
   ```

## Upgrading<a name="cliv2-linux-upgrade"></a>

To upgrade your copy of the AWS CLI version 2, run the same steps that you used to install it, but this time include the `--update` or `-u` option on the `install` command line\. If the installer finds an existing version of the AWS CLI version 2 in the target installation folder and the `--update` option isn't used, the install fails\.

Find the symlink that the installer created\. This gives you the path to use with the `--bin-dir` parameter\.

```
$ which aws
/usr/local/bin/aws
```

Use that to find the folder that the symlink points to\. This gives you the path to use with the `--install-dir` parameter\.

```
$ ls -l /usr/local/bin/aws
lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
```

Then use that information to construct the install command\.

```
$ sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
```

## Uninstalling<a name="cliv2-linux-remove"></a>

To uninstall the AWS CLI version 2, run the following commands, substituting the paths you used to install\.

Find the symlinks that you created in the `--bin-dir` folder\.

```
$ which aws
/usr/local/bin/aws
```

Use that to find the `--install-dir` folder that the symlink points to\.

```
$ ls -l /usr/local/bin/aws
lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
```

Now delete the two symlinks in the `--bin-dir` folder\. If your user account has write permission to these folders, you don't need to use `sudo`\.

```
$ sudo rm /usr/local/bin/aws
$ sudo rm /usr/local/bin/aws2_completer
```

Finally, you can delete the `--install-dir` folder\. Again, if your user account has write permission to this folder, you don't need to use `sudo`\.

```
$ sudo rm -rf /usr/local/aws-cli
```

## Verifying the Integrity and Authenticity of the Downloaded Files<a name="v2-install-linux-validate"></a>

The AWS CLI version 2 installer package \.zip files are cryptographically signed using PGP signatures\. You can use the following steps to verify the signatures by using the `GnuPG` tool\. If there is any damage or alteration of the files, this verification fails and you should not proceed with installation\.

The following example assumes you downloaded the installer package and saved it locally as `awscliv2.zip`\. If you named it something else, substitute that name in the following steps\.

Steps 1, 2, and 3 are prerequisite steps that you need to perform only once\. You should perform steps 4 and 5 every time you download a new copy of the installer package\.

**To validate the files using the PGP signature**

1. Download and install the `gpg` command using your favorite package manager\. For more information about GnuPG, see the [GnuPG website](https://www.gnupg.org/)\. 

1. Create a text file and paste in the following text\.

   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   
   mQINBF2Cr7UBEADJZHcgusOJl7ENSyumXh85z0TRV0xJorM2B/JL0kHOyigQluUG
   ZMLhENaG0bYatdrKP+3H91lvK050pXwnO/R7fB/FSTouki4ciIx5OuLlnJZIxSzx
   PqGl0mkxImLNbGWoi6Lto0LYxqHN2iQtzlwTVmq9733zd3XfcXrZ3+LblHAgEt5G
   TfNxEKJ8soPLyWmwDH6HWCnjZ/aIQRBTIQ05uVeEoYxSh6wOai7ss/KveoSNBbYz
   gbdzoqI2Y8cgH2nbfgp3DSasaLZEdCSsIsK1u05CinE7k2qZ7KgKAUIcT/cR/grk
   C6VwsnDU0OUCideXcQ8WeHutqvgZH1JgKDbznoIzeQHJD238GEu+eKhRHcz8/jeG
   94zkcgJOz3KbZGYMiTh277Fvj9zzvZsbMBCedV1BTg3TqgvdX4bdkhf5cH+7NtWO
   lrFj6UwAsGukBTAOxC0l/dnSmZhJ7Z1KmEWilro/gOrjtOxqRQutlIqG22TaqoPG
   fYVN+en3Zwbt97kcgZDwqbuykNt64oZWc4XKCa3mprEGC3IbJTBFqglXmZ7l9ywG
   EEUJYOlb2XrSuPWml39beWdKM8kzr1OjnlOm6+lpTRCBfo0wa9F8YZRhHPAkwKkX
   XDeOGpWRj4ohOx0d2GWkyV5xyN14p2tQOCdOODmz80yUTgRpPVQUtOEhXQARAQAB
   tCFBV1MgQ0xJIFRlYW0gPGF3cy1jbGlAYW1hem9uLmNvbT6JAlQEEwEIAD4WIQT7
   Xbd/1cEYuAURraimMQrMRnJHXAUCXYKvtQIbAwUJB4TOAAULCQgHAgYVCgkICwIE
   FgIDAQIeAQIXgAAKCRCmMQrMRnJHXJIXEAChLUIkg80uPUkGjE3jejvQSA1aWuAM
   yzy6fdpdlRUz6M6nmsUhOExjVIvibEJpzK5mhuSZ4lb0vJ2ZUPgCv4zs2nBd7BGJ
   MxKiWgBReGvTdqZ0SzyYH4PYCJSE732x/Fw9hfnh1dMTXNcrQXzwOmmFNNegG0Ox
   au+VnpcR5Kz3smiTrIwZbRudo1ijhCYPQ7t5CMp9kjC6bObvy1hSIg2xNbMAN/Do
   ikebAl36uA6Y/Uczjj3GxZW4ZWeFirMidKbtqvUz2y0UFszobjiBSqZZHCreC34B
   hw9bFNpuWC/0SrXgohdsc6vK50pDGdV5kM2qo9tMQ/izsAwTh/d/GzZv8H4lV9eO
   tEis+EpR497PaxKKh9tJf0N6Q1YLRHof5xePZtOIlS3gfvsH5hXA3HJ9yIxb8T0H
   QYmVr3aIUes20i6meI3fuV36VFupwfrTKaL7VXnsrK2fq5cRvyJLNzXucg0WAjPF
   RrAGLzY7nP1xeg1a0aeP+pdsqjqlPJom8OCWc1+6DWbg0jsC74WoesAqgBItODMB
   rsal1y/q+bPzpsnWjzHV8+1/EtZmSc8ZUGSJOPkfC7hObnfkl18h+1QtKTjZme4d
   H17gsBJr+opwJw/Zio2LMjQBOqlm3K1A4zFTh7wBC7He6KPQea1p2XAMgtvATtNe
   YLZATHZKTJyiqA==
   =vYOk
   -----END PGP PUBLIC KEY BLOCK-----
   ```

   Here are the details of the public key for reference\.

   ```
   Key ID:           A6310ACC4672
   Type:             RSA
   Size:             4096/4096
   Created:          2019-09-18
   Expires:          2023-09-17
   User ID:          AWS CLI Team &aws-cli@amazon.com>
   Key fingerprint:  FB5D B77F D5C1 18B8 0511  ADA8 A631 0ACC 4672 475C
   ```

1. Import the AWS CLI public key with the following command, substituting *public\-key\-file\-name* with whatever you named the file in step 2\.

   ```
   $ gpg --import public-key-file-name
   gpg: /home/username/.gnupg/trustdb.gpg: trustdb created
   gpg: key A6310ACC4672475C: public key "AWS CLI Team <aws-cli@amazon.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1
   ```

1. Download the AWS CLI signature file for the package you downloaded\. It has the same path and name as the \.zip file it corresponds to, but has the extension `.sig`\. In the following examples, we save it to the current folder as a file named `awscliv2.sig`\.

   ```
   $ curl -o awscliv2.sig [https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip.sig](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip.sig)
   ```

1. Verify the signature, passing both the downloaded `.sig` and `.zip` file names as parameters to the `gpg` command\.

   ```
   $ gpg --verify awscliv2.sig awscliv2.zip
   ```

   The output should look similar to the following\.

   ```
   gpg: Signature made Mon Nov  4 19:00:01 2019 PST
   gpg:                using RSA key FB5D B77F D5C1 18B8 0511 ADA8 A631 0ACC 4672 475C
   gpg: Good signature from "AWS CLI Team <aws-cli@amazon.com>" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: FB5D B77F D5C1 18B8 0511  ADA8 A631 0ACC 4672 475C
   ```
**Important**  
The warning in the output is expected and doesn't indicate a problem\. It occurs because there isn't a chain of trust between your personal PGP key \(if you have one\) and the AWS CLI PGP key\. For more information, see [Web of trust](https://wikipedia.org/wiki/Web_of_trust)\.
