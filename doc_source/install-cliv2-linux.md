# Installing the AWS CLI version 2 on Linux<a name="install-cliv2-linux"></a>

This section describes how to install, update, and remove the AWS CLI version 2 on Linux\. The AWS CLI version 2 has no dependencies on other Python packages\. It has a self\-contained, embedded copy of Python included in the installer\.

**Important**  
AWS CLI versions 1 and 2 use the same `aws` command name\. If you have both versions installed, your computer uses the first one found in your search path\. If you previously installed AWS CLI version 1, we recommend that you do one of the following to use AWS CLI version 2:  
** Recommended** – Uninstall AWS CLI version 1 and use only AWS CLI version 2\. For uninstall instructions, determine the method you used to install AWS CLI version 1 and follow the appropriate uninstall instructions for your operating system in [Installing the AWS CLI version 1](install-cliv1.md)
Use your operating system's ability to create a symbolic link \(symlink\) or alias with a different name for one of the two `aws` commands\. For example, you can use a [symbolic link](https://www.linux.com/tutorials/understanding-linux-links/) or [alias](https://www.linux.com/tutorials/aliases-diy-shell-commands/) on Linux and macOS, or [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) on Windows\.

**Topics**
+ [Prerequisites for Linux](#cliv2-linux-prereq)
+ [Install the AWS CLI version 2 on Linux](#cliv2-linux-install)
+ [Update the AWS CLI version 2 on Linux](#cliv2-linux-upgrade)
+ [Uninstall the AWS CLI version 2 on Linux](#cliv2-linux-remove)
+ [Verify the integrity and authenticity of the downloaded installer files](#v2-install-linux-validate)

## Prerequisites for Linux<a name="cliv2-linux-prereq"></a>
+ You must be able to extract or "unzip" the downloaded package\. If your operating system doesn't have the built\-in `unzip` command, use an equivalent\.
+ The AWS CLI version 2 uses `glibc`, `groff`, and `less`\. These are included by default in most major distributions of Linux\.
+ We support the AWS CLI version 2 on 64\-bit versions of recent distributions of CentOS, Fedora, Ubuntu, Amazon Linux 1, and Amazon Linux 2\.
+ We support the AWS CLI version 2 on Linux ARM\.
+ Because AWS doesn't maintain third\-party repositories, we can’t guarantee that they contain the latest version of the AWS CLI\.

## Install the AWS CLI version 2 on Linux<a name="cliv2-linux-install"></a>

Follow these steps from the command line to install the AWS CLI on Linux\. 

We provide the steps in one easy to copy and paste group based on whether you use 64\-bit Linux or Linux ARM\. See the descriptions of each line in the steps that follow\.

------
#### [ Linux x86 \(64\-bit\) ]

**For the latest version of the AWS CLI,** use the following command block:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

 For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

**For the latest version of the AWS CLI, **use the following command block:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

 For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------

1. Download the installation file in one of the following ways:
   + **Use the `curl` command** – The `-o` option specifies the file name that the downloaded package is written to\. The options on the following example command write the downloaded file to the current directory with the local name `awscliv2.zip`\. 

------
#### [ Linux x86 \(64\-bit\) ]

     **For the current version of the AWS CLI,** use the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     ```

     **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
     ```

      For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

     **For the current version of the AWS CLI,** use the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"
     ```

     **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip" -o "awscliv2.zip"curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
     ```

      For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
   + **Downloading from the URL** – To download the installer with your browser, use one of the following URLs\. You can verify the integrity and authenticity of your downloaded installation file before you extract \(unzip\) the package\. See [Verify the integrity and authenticity of the downloaded installer files](#v2-install-linux-validate) for more information\.

------
#### [ Linux x86 \(64\-bit\) ]

     **For the latest version of the AWS CLI:** [https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)

     **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following url: [https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip)

      For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

     **For the latest version of the AWS CLI:** [https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip](https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip)

     **For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following url [https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip](https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip)

     For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------

1. Unzip the installer\. If your Linux distribution doesn't have a built\-in `unzip` command, use an equivalent to unzip it\. The following example command unzips the package and creates a directory named `aws` under the current directory\.

   ```
   $ unzip awscliv2.zip
   ```

1. Run the install program\. The installation command uses a file named `install` in the newly unzipped `aws` directory\. By default, the files are all installed to `/usr/local/aws-cli`, and a symbolic link is created in `/usr/local/bin`\. The command includes `sudo` to grant write permissions to those directories\. 

   ```
   $ sudo ./aws/install
   ```

   You can install without `sudo` if you specify directories that you already have write permissions to\. Use the following instructions for the `install` command to specify the installation location:
   + Ensure that the paths you provide to the `-i` and `-b` parameters contain no volume name or directory names that contain any space characters or other white space characters\. If there is a space, the installation fails\.
   + `--install-dir` or `-i` – This option specifies the directory to copy all of the files to\.

     The default value is `/usr/local/aws-cli`\.
   + `--bin-dir` or `-b` – This option specifies that the main `aws` program in the install directory is symbolically linked to the file `aws` in the specified path\. You must have write permissions to the specified directory\. Creating a symlink to a directory that is already in your path eliminates the need to add the install directory to the user's `$PATH` variable\. 

     The default value is `/usr/local/bin`\.

   ```
   $ sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
   ```

1. Confirm the installation\.

   ```
   $ aws --version
   aws-cli/2.0.47 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.0.0
   ```

## Update the AWS CLI version 2 on Linux<a name="cliv2-linux-upgrade"></a>

To update your copy of the AWS CLI version 2, from the Linux command line, follow these steps\.

1. Download the installation file in one of the following ways:

   **Using the `curl` command** – The options on the following example command write the downloaded file to the current directory with the local name `awscliv2.zip`\. 

   The `-o` option specifies the file name that the downloaded package is written to\. In this example, the file is written to `awscliv2.zip` in the current directory\.

------
#### [ Linux x86 \(64\-bit\) ]

   **For the latest version of the AWS CLI,** use the following command block:

   ```
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   ```

   **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

   **For the latest version of the AWS CLI, **use the following command block:

   ```
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip" -o "awscliv2.zip"
   ```

   **For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip" -o "awscliv2.zip"
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------

   **Downloading from the URL** – To download the installer using your browser, use one of the following URLs\. You can verify the integrity and authenticity of the installation file after you download it\. For more information before you unzip the package, see [Verify the integrity and authenticity of the downloaded installer files](#v2-install-linux-validate)\.

------
#### [ Linux x86 \(64\-bit\) ]

   **For the latest version of the AWS CLI:** [https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)

   **For a specific version of the AWS CLI:** Append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following link [https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip)\. For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

   **For the latest version of the AWS CLI:** [https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip](https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip)

   **For a specific version of the AWS CLI:** Append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following link: [https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip](https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip)\. For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------

1. Unzip the installer\. If your Linux distribution doesn't have a built\-in `unzip` command, use an equivalent to install it\. The following example command unzips the package and creates a directory named `aws` under the current directory\.

   ```
   $ unzip awscliv2.zip
   ```

1. To ensure that the update installs in the same location as your current AWS CLI version 2, locate the existing symlink and installation directory\. 
   + Use the `which` command to find your symlink\. This gives you the path to use with the `--bin-dir` parameter\.

     ```
     $ which aws
     /usr/local/bin/aws
     ```
   + Use the `ls` command to find the directory that your symlink points to\. This gives you the path to use with the `--install-dir` parameter\.

     ```
     $ ls -l /usr/local/bin/aws
     lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
     ```

1. Use your symlink and installer information to construct the `install` command with the `--update` parameter\.

   ```
   $ sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
   ```

1. Confirm the installation\.

   ```
   $ aws --version
   aws-cli/2.0.47 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.0.0
   ```

## Uninstall the AWS CLI version 2 on Linux<a name="cliv2-linux-remove"></a>

To uninstall the AWS CLI version 2, run the following commands\.

1. Locate the symlink and install paths\.
   + Use the `which` command to find the symlink\. This shows the path you used with the `--bin-dir` parameter\.

     ```
     $ which aws
     /usr/local/bin/aws
     ```
   + Use the `ls` command to find the directory that the symlink points to\. This gives you the path you used with the `--install-dir` parameter\.

     ```
     $ ls -l /usr/local/bin/aws
     lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
     ```

1. Delete the two symlinks in the `--bin-dir` directory\. If your user account has write permission to these directories, you don't need to use `sudo`\.

   ```
   $ sudo rm /usr/local/bin/aws
   $ sudo rm /usr/local/bin/aws_completer
   ```

1. Delete the `--install-dir` directory\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm -rf /usr/local/aws-cli
   ```

## Verify the integrity and authenticity of the downloaded installer files<a name="v2-install-linux-validate"></a>

The AWS CLI version 2 installer package `.zip` files are cryptographically signed using PGP signatures\. You can use the following steps to verify the signatures by using the `GnuPG` tool\. If there is any damage or alteration of the files, this verification fails and you should not proceed with installation\.

The following example assumes you downloaded the installer package and saved it locally as `awscliv2.zip`\. If you named it something else, substitute that name in the following steps\.

**To validate the files using the PGP signature**

1. Download and install the `gpg` command using your package manager\. For more information about `GnuPG`, see the [GnuPG website](https://www.gnupg.org/)\. 

1. To create the public key file, create a text file and paste in the following text\.

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

   For reference, the following are the details of the public key\.

   ```
   Key ID:           A6310ACC4672
   Type:             RSA
   Size:             4096/4096
   Created:          2019-09-18
   Expires:          2023-09-17
   User ID:          AWS CLI Team <aws-cli@amazon.com>
   Key fingerprint:  FB5D B77F D5C1 18B8 0511  ADA8 A631 0ACC 4672 475C
   ```

1. Import the AWS CLI public key with the following command, substituting *public\-key\-file\-name* with the file name of the public key you created\.

   ```
   $ gpg --import public-key-file-name
   gpg: /home/username/.gnupg/trustdb.gpg: trustdb created
   gpg: key A6310ACC4672475C: public key "AWS CLI Team <aws-cli@amazon.com>" imported
   gpg: Total number processed: 1
   gpg:               imported: 1
   ```

1. Download the AWS CLI signature file for the package you downloaded\. It has the same path and name as the `.zip` file it corresponds to, but has the extension `.sig`\. In the following examples, we save it to the current directory as a file named `awscliv2.sig`\.

------
#### [ Linux x86 \(64\-bit\) ]

   **For the latest version of the AWS CLI,** use the following command block:

   ```
   $ curl -o awscliv2.sig https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip.sig
   ```

   **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip.sig` resulting in the following command:

   ```
   $ curl -o awscliv2.sig https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip.sig
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------
#### [ Linux ARM ]

   **For the latest version of the AWS CLI, **use the following command block:

   ```
   $ curl -o awscliv2.sig https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip.sig
   ```

   **For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip.sig` resulting in the following command:

   ```
   $ curl -o awscliv2.sig https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip.sig
   ```

    For a list of versions, see the [AWS CLI version 2 changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst) on *GitHub*\.

------

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