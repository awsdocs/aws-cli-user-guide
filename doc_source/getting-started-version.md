--------

--------

# Installing past releases of the AWS CLI version 2<a name="getting-started-version"></a>

This topic describes how to install the past releases of the AWS Command Line Interface version 2 \(AWS CLI\) on supported operating systems\. For information on the AWS CLI version 2 releases, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on GitHub\.

AWS CLI version 2 installation instructions:

## Linux<a name="versioned-linux"></a>

### Installation requirements<a name="versioned-linux-reqs"></a>
+ You know which release of the AWS CLI version 2 you'd like to install\. For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.
+ You must be able to extract or "unzip" the downloaded package\. If your operating system doesn't have the built\-in `unzip` command, use an equivalent\.
+ The AWS CLI version 2 uses `glibc`, `groff`, and `less`\. These are included by default in most major distributions of Linux\.
+ We support the AWS CLI version 2 on 64\-bit versions of recent distributions of CentOS, Fedora, Ubuntu, Amazon Linux 1, Amazon Linux 2 and Linux ARM\.
+ Because AWS doesn't maintain third\-party repositories, we can’t guarantee that they contain the latest version of the AWS CLI\.

### Installation instructions<a name="versioned-linux-instructions"></a>

Follow these steps from the command line to install the AWS CLI on Linux\. 

We provide the steps in one easy to copy and paste group based on whether you use 64\-bit Linux or Linux ARM\. See the descriptions of each line in the steps that follow\.

------
#### [ Linux x86 \(64\-bit\) ]

To specify a version, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
```

 For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

------
#### [ Linux ARM ]

To specify a version, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip" -o "awscliv2.zip"
```

 For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

------

1. Download the installation file in one of the following ways:

------
#### [ Linux x86 \(64\-bit\) ]
   + **Use the `curl` command** – The `-o` option specifies the file name that the downloaded package is written to\. The options on the following example command write the downloaded file to the current directory with the local name `awscliv2.zip`\. 

     To specify a version, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-x86_64-2.0.30.zip` resulting in the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip"
     unzip awscliv2.zip
     sudo ./aws/install
     ```

      For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.
   + **Downloading from the URL** – 

     In your browser, download your specific version of the AWS CLI by appending a hyphen and the version number to the filename\. 

     ```
     https://awscli.amazonaws.com/awscli-exe-linux-x86_64-version.number.zip
     ```

     For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following link: [awscli-exe-linux-aarch64-2.0.30.zip](awscli-exe-linux-aarch64-2.0.30.zip)

------
#### [ Linux ARM ]
   + **Use the `curl` command** – The `-o` option specifies the file name that the downloaded package is written to\. The options on the following example command write the downloaded file to the current directory with the local name `awscliv2.zip`\. 

     To specify a version, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following command:

     ```
     $ curl "https://awscli.amazonaws.com/awscli-exe-linux-aarch64-2.0.30.zip" -o "awscliv2.zip"
     unzip awscliv2.zip
     sudo ./aws/install
     ```
   + **Downloading from the URL** – 

     In your browser, download your specific version of the AWS CLI by appending a hyphen and the version number to the filename\. 

     ```
     https://awscli.amazonaws.com/awscli-exe-linux-aarch64-version.number.zip
     ```

     For this example the filename for version *2\.0\.30* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following link: [awscli-exe-linux-aarch64-2.0.30.zip](awscli-exe-linux-aarch64-2.0.30.zip)

------

1. **\(Optional\) Verifying the integrity of your downloaded zip file**

   If you chose to manually download the AWS CLI installer package `.zip` in the above steps, you can use the following steps to verify the signatures by using the `GnuPG` tool\.

   The AWS CLI installer package `.zip` files are cryptographically signed using PGP signatures\. If there is any damage or alteration of the files, this verification fails and you should not proceed with installation\.

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

       For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

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

       For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

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
   $ ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
   ```
**Note**  
To update your current installation of the AWS CLI version 2 to a newer version, add your existing symlink and installer information to construct the `install` command with the `--update` parameter\.  

   ```
   $ sudo ./aws/install --bin-dir /usr/local/bin --install-dir /usr/local/aws-cli --update
   ```
To locate the existing symlink and installation directory, use the following steps:  
Use the `which` command to find your symlink\. This gives you the path to use with the `--bin-dir` parameter\.  

      ```
      $ which aws
      /usr/local/bin/aws
      ```
Use the `ls` command to find the directory that your symlink points to\. This gives you the path to use with the `--install-dir` parameter\.  

      ```
      $ ls -l /usr/local/bin/aws
      lrwxrwxrwx 1 ec2-user ec2-user 49 Oct 22 09:49 /usr/local/bin/aws -> /usr/local/aws-cli/v2/current/bin/aws
      ```

1. Confirm the installation with the following command\. 

   ```
   $ aws --version
   aws-cli/2.7.24 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.4.5
   ```

   If the `aws` command cannot be found, you might need to restart your terminal or follow the troubleshooting in [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### \(Optional\) Verifying the integrity of your downloaded zip file<a name="versioned-linux-verify"></a>

If you chose to manually download the AWS CLI version 2 installer package `.zip` in the above steps, you use can use the following steps to verify the signatures by using the `GnuPG` tool\. 

The AWS CLI version 2 installer package `.zip` files are cryptographically signed using PGP signatures\. If there is any damage or alteration of the files, this verification fails and you should not proceed with installation\.

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

    For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

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

    For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

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

## macOS<a name="versioned-macos"></a>

### Installation requirements<a name="versioned-macos-reqs"></a>
+ You know which release of the AWS CLI version 2 you'd like to install\. For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.
+ We support the AWS CLI version 2 on Apple\-supported versions of 64\-bit macOS\.
+ Because AWS doesn't maintain third\-party repositories, we can’t guarantee that they contain the latest version of the AWS CLI\.

### Installation instructions<a name="versioned-macos-install"></a>

You can install the AWS CLI version 2 on macOS in the following ways\.

------
#### [ GUI installer ]

The following steps show how to install or update to the latest version of the AWS CLI version 2 by using the standard macOS user interface and your browser\. If you are updating to the latest version, use the same installation method that you used for your current version\.

1. In your browser, download your specific version of the AWS CLI by appending a hyphen and the version number to the filename\. 

   ```
   https://awscli.amazonaws.com/AWSCLIV2-version.number.pkg
   ```

   For this example, the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following link: [https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg](https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg)\. 

1. Run your downloaded file and follow the on\-screen instructions\. You can choose to install the AWS CLI version 2 in the following ways:
   + **For all users on the computer \(requires `sudo`\)**
     + You can install to any folder, or choose the recommended default folder of `/usr/local/aws-cli`\.
     + The installer automatically creates a symlink at `/usr/local/bin/aws` that links to the main program in the installation folder you chose\.
   + **For only the current user \(doesn't require `sudo`\)**
     + You can install to any folder to which you have write permission\.
     + Due to standard user permissions, after the installer finishes, you must manually create a symlink file in your `$PATH` that points to the `aws` and `aws_completer` programs by using the following commands at the command prompt\. If your `$PATH` includes a folder you can write to, you can run the following command without `sudo` if you specify that folder as the target's path\. If you don't have a writable folder in your `$PATH`, you must use `sudo` in the commands to get permissions to write to the specified target folder\. The default location for a symlink is `/usr/local/bin/`\.

       ```
       $ sudo ln -s /folder/installed/aws-cli/aws /usr/local/bin/aws
       $ sudo ln -s /folder/installed/aws-cli/aws_completer /usr/local/bin/aws_completer
       ```
**Note**  
You can view debug logs for the installation by pressing **Cmd\+L** anywhere in the installer\. This opens a log pane that enables you to filter and save the log\. The log file is also automatically saved to `/var/log/install.log`\.

1. To verify that the shell can find and run the `aws` command in your `$PATH`, use the following commands\. 

   ```
   $ which aws
   /usr/local/bin/aws 
   $ aws --version
   aws-cli/2.7.24 Python/3.8.8 Darwin/18.7.0 botocore/2.4.5
   ```

   If the `aws` command cannot be found, you might need to restart your terminal or follow the troubleshooting in [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

------
#### [ Command line installer \- All users ]

If you have `sudo` permissions, you can install the AWS CLI version 2 for all users on the computer\. We provide the steps in one easy to copy and paste group\. See the descriptions of each line in the following steps\. 

For a specific version of the AWS CLI, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

```
$ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
$ sudo installer -pkg AWSCLIV2.pkg -target /
```

1. Download the file using the `curl` command\. The `-o` option specifies the file name that the downloaded package is written to\. In this example, the file is written to `AWSCLIV2.pkg` in the current folder\.

   For a specific version of the AWS CLI, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
   ```

    For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

1. Run the standard macOS `installer` program, specifying the downloaded `.pkg` file as the source\. Use the `-pkg` parameter to specify the name of the package to install, and the `-target /` parameter for which drive to install the package to\. The files are installed to `/usr/local/aws-cli`, and a symlink is automatically created in `/usr/local/bin`\. You must include `sudo` on the command to grant write permissions to those folders\. 

   ```
   $ sudo installer -pkg ./AWSCLIV2.pkg -target /
   ```

   After installation is complete, debug logs are written to `/var/log/install.log`\.

1. To verify that the shell can find and run the `aws` command in your `$PATH`, use the following commands\. 

   ```
   $ which aws
   /usr/local/bin/aws 
   $ aws --version
   aws-cli/2.7.24 Python/3.8.8 Darwin/18.7.0 botocore/2.4.5
   ```

   If the `aws` command cannot be found, you might need to restart your terminal or follow the troubleshooting in [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

------
#### [ Command line \- Current user ]

1. To specify which folder the AWS CLI is installed to, you must create an XML file\. This file is an XML\-formatted file that looks like the following example\. Leave all values as shown, except you must replace the path */Users/myusername* in line 9 with the path to the folder you want the AWS CLI version 2 installed to\. *The folder must already exist, or the command fails\.* This XML example specifies that the installer installs the AWS CLI in the folder `/Users/myusername`, where it creates a folder named `aws-cli`\.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
     <array>
       <dict>
         <key>choiceAttribute</key>
         <string>customLocation</string>
         <key>attributeSetting</key>
         <string>/Users/myusername</string>
         <key>choiceIdentifier</key>
         <string>default</string>
       </dict>
     </array>
   </plist>
   ```

1. Download the `pkg` installer using the `curl` command\. The `-o` option specifies the file name that the downloaded package is written to\. In this example, the file is written to `AWSCLIV2.pkg` in the current folder\.

   For the specific version of the AWS CLI, append a hyphen and the version number to the filename\. For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.pkg` resulting in the following command:

   ```
   $ curl "https://awscli.amazonaws.com/AWSCLIV2-2.0.30.pkg" -o "AWSCLIV2.pkg"
   ```

    For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

1. Run the standard macOS `installer` program with the following options:
   + Specify the name of the package to install by using the `-pkg` parameter\.
   + Specify installing to a *current user only* by setting the `-target` parameter to `CurrentUserHomeDirectory`\.
   + Specify the path \(relative to the current folder\) and name of the XML file that you created in the `-applyChoiceChangesXML` parameter\.

   The following example installs the AWS CLI in the folder `/Users/myusername/aws-cli`\.

   ```
   $ installer -pkg AWSCLIV2.pkg \
               -target CurrentUserHomeDirectory \
               -applyChoiceChangesXML choices.xml
   ```

1. Because standard user permissions typically don't allow writing to folders in your `$PATH`, the installer in this mode doesn't try to add the symlinks to the `aws` and `aws_completer` programs\. For the AWS CLI to run correctly, you must manually create the symlinks after the installer finishes\. If your `$PATH` includes a folder you can write to and you specify the folder as the target's path, you can run the following command without `sudo`\. If you don't have a writable folder in your `$PATH`, you must use `sudo` for permissions to write to the specified target folder\. The default location for a symlink is `/usr/local/bin/`\.

   ```
   $ sudo ln -s /folder/installed/aws-cli/aws /usr/local/bin/aws
   $ sudo ln -s /folder/installed/aws-cli/aws_completer /usr/local/bin/aws_completer
   ```

   After installation is complete, debug logs are written to `/var/log/install.log`\.

1. To verify that the shell can find and run the `aws` command in your `$PATH`, use the following commands\. 

   ```
   $ which aws
   /usr/local/bin/aws 
   $ aws --version
   aws-cli/2.7.24 Python/3.8.8 Darwin/18.7.0 botocore/2.4.5
   ```

   If the `aws` command cannot be found, you might need to restart your terminal or follow the troubleshooting in [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

------

## Windows<a name="versioned-windows"></a>

### Installation requirements<a name="versioned-windows-reqs"></a>
+ You know which release of the AWS CLI version 2 you'd like to install\. For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.
+ A 64\-bit version of Windows XP or later\.
+ Admin rights to install software

### Installation instructions<a name="versioned-windows-install"></a>

To update your current installation of AWS CLI version 2 on Windows, download a new installer each time you update to overwrite previous versions\. AWS CLI is updated regularly\. To see when the latest version was released, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\. 

1. Download and run the AWS CLI MSI installer for Windows \(64\-bit\) in one of the following ways:
   + **Downloading and running the MSI installer:** To create your download link for a specific version of the AWS CLI, append a hyphen and the version number to the filename\.

     ```
     https://awscli.amazonaws.com/AWSCLIV2-version.number.msi
     ```

     For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.msi` resulting in the following link: [https://awscli.amazonaws.com/AWSCLIV2-2.0.30.msi](https://awscli.amazonaws.com/AWSCLIV2-2.0.30.msi)\. 
   + **Using the msiexec command:** Alternatively, you can use the MSI installer by adding the link to the `msiexec` command\. For a specific version of the AWS CLI, append a hyphen and the version number to the filename\.

     ```
     C:\> msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2-version.number.msi
     ```

     For this example the filename for version *2\.0\.30* would be `AWSCLIV2-2.0.30.msi` resulting in the following link [https://awscli.amazonaws.com/AWSCLIV2-2.0.30.msi](https://awscli.amazonaws.com/AWSCLIV2-2.0.30.msi)\. 

     ```
     C:\> msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2-2.0.30.msi
     ```

     For various parameters that can be used with `msiexec`, see [msiexec](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/msiexec) on the *Microsoft Docs* website\.

   For a list of versions, see the [AWS CLI version 2 Changelog](https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst?plain=1) on *GitHub*\.

1. To confirm the installation, open the **Start** menu, search for `cmd` to open a command prompt window, and at the command prompt use the `aws --version` command\. 

   ```
   C:\> aws --version
   aws-cli/2.7.24 Python/3.8.8 Windows/10 exe/AMD64 prompt/off
   ```

   If Windows is unable to find the program, you might need to close and reopen the command prompt window to refresh the path, or follow the troubleshooting in [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

## Troubleshooting AWS CLI install and uninstall errors<a name="getting-started-version-tshoot"></a>

If you come across issues after installing or uninstalling the AWS CLI, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md) for troubleshooting steps\. For the most relevant troubleshooting steps, see [Command not found errors](cli-chap-troubleshooting.md#tshoot-install-not-found), [The "`aws --version`" command returns a different version than you installed](cli-chap-troubleshooting.md#tshoot-install-wrong-version), and [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\.

## Next steps<a name="getting-started-version-next"></a>

After completing the steps in [Prerequisites to use the AWS CLI version 2](getting-started-prereqs.md) and installing the AWS CLI, you should perform a [Quick setup](getting-started-quickstart.md)\.