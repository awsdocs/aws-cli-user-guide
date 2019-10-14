# Install the AWS CLI Using the Bundled Installer \(Linux, macOS, or Unix\)<a name="install-bundle"></a>

On Linux, macOS, or Unix, you can use the bundled installer to install the AWS Command Line Interface \(AWS CLI\)\. The bundled installer includes all dependencies and can be used offline\.

**Important**  
On January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI with Python 2\.6 or Python 3\.3](deprecate-python-26-33.md) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

**Important**  
The bundled installer doesn't support installing to paths that contain spaces\.

**Topics**
+ [Prerequisites](#install-bundle-other-os-prereq)
+ [Install the AWS CLI Using the Bundled Installer](#install-bundle-other)
+ [Install the AWS CLI without Sudo \(Linux, macOS, or Unix\)](#install-bundle-user)
+ [Uninstall the AWS CLI](#install-bundle-uninstall)

## Prerequisites<a name="install-bundle-other-os-prereq"></a>
+ Linux, macOS, or Unix
+ Python 2 version 2\.7\+ or Python 3 version 3\.4\+

Check your Python installation\.

```
$ python --version
```

If your computer doesn't already have Python installed, or you would like to install a different version of Python, follow the procedure in [Install the AWS CLI on Linux](install-linux.md)\.

## Install the AWS CLI Using the Bundled Installer<a name="install-bundle-other"></a>

The steps below enable you to install the AWS CLI from the command line on any build of Linux or macOS\.

To download it directly \(without using curl\) use this link:
+ [https://s3.amazonaws.com/aws-cli/awscli-bundle.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)

Here are a summary of the installation commands explained below that you can cut and paste to run as a single set of commands\.

```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

Follow these steps from the command line to install the AWS CLI using the bundled installer\.

**To install the AWS CLI using the bundled installer**

1. Download the AWS CLI Bundled Installer using the following command:

   ```
   $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
   ```

1. Unzip the package\.

   ```
   $ unzip awscli-bundle.zip
   ```
**Note**  
If you don't have `unzip`, use your Linux distribution's built\-in package manager to install it\.

1. Run the install program\.

   ```
   $ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```
**Note**  
By default, the install script runs under the system default version of Python\. If you have installed an alternative version of Python and want to use that to install the AWS CLI, run the install script with that version by absolute path to the Python executable\. For example:  

   ```
   $ sudo /usr/local/bin/python3.7 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

The installer installs the AWS CLI at `/usr/local/aws` and creates the symlink `aws` at the `/usr/local/bin` directory\. Using the `-b` option to create a symlink eliminates the need to specify the install directory in the user's `$PATH` variable\. This should enable all users to call the AWS CLI by typing `aws` from any directory\.

To see an explanation of the `-i` and `-b` options, use the `-h` option\.

```
$ ./awscli-bundle/install -h
```

## Install the AWS CLI without Sudo \(Linux, macOS, or Unix\)<a name="install-bundle-user"></a>

If you don't have `sudo` permissions or want to install the AWS CLI only for the current user, you can use a modified version of the previous commands\.

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
$ unzip awscli-bundle.zip
$ ./awscli-bundle/install -b ~/bin/aws
```

This installs the AWS CLI to the default location \(`~/.local/lib/aws`\) and creates a symbolic link \(symlink\) at `~/bin/aws`\. Make sure that `~/bin` is in your `PATH` environment variable for the symlink to work\.

```
$ echo $PATH | grep ~/bin     // See if $PATH contains ~/bin (output will be empty if it doesn't)
$ export PATH=~/bin:$PATH     // Add ~/bin to $PATH if necessary
```

**Tip**  
To ensure that your `$PATH` settings are retained between sessions, add the `export` line to your shell profile \(`~/.profile`, `~/.bash_profile`, and so on\)\.

## Uninstall the AWS CLI<a name="install-bundle-uninstall"></a>

The bundled installer doesn't put anything outside of the installation directory except the optional symlink, so uninstalling is as simple as deleting those two items\.

```
$ sudo rm -rf /usr/local/aws
$ sudo rm /usr/local/bin/aws
```