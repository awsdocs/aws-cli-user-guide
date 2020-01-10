# Install the AWS CLI version 1 on macOS<a name="install-macos"></a>

The recommended way to install version 1 of the AWS Command Line Interface \(AWS CLI\) on macOS is to use the bundled installer\. The bundled installer includes all dependencies and you can use it offline\.

**Important**  
On January 10th, 2020, AWS CLI version 1, which requires a separate installation of Python to operate, stopped supporting Python versions 2\.6 and 3\.3\. All builds of AWS CLI version 1 released after January 10th, 2020, starting with version 1\.17, require Python 2\.7, Python 3\.4, or a later version to successfully use the AWS CLI\.  
This change does not affect the following versions of the AWS CLI:  
**Windows MSI installer version of AWS CLI version 1\.** The Windows MSI installer for AWS CLI version 1 includes and uses its own embedded copy of Python, independent of any other Python version that you might have installed\. If you're using an MSI installer\-based AWS CLI, no changes are required\.
**AWS CLI version 2\.** All installers for AWS CLI version 2 include and use an embedded copy of Python, independent of any other Python version that you might have installed\. If you're using AWS CLI version 2, no changes are required\.
For more information, see [Using the AWS CLI version 1 with Earlier Versions of Python](deprecate-old-python-versions.md) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

**Important**  
The bundled installer doesn't support installing to paths that contain spaces\.

**Topics**
+ [Prerequisites](#install-bundle-macos-os-prereq)
+ [Install the AWS CLI version 1 Using the Bundled Installer](#install-bundle-macos)
+ [Install the AWS CLI version 1 on macOS Using pip](#awscli-install-osx-pip)
+ [Add the AWS CLI version 1 Executable to Your macOS Command Line Path](#awscli-install-osx-path)

## Prerequisites<a name="install-bundle-macos-os-prereq"></a>
+ Python 2 version 2\.7\+ or Python 3 version 3\.4\+

Check your Python installation\.

```
$ python --version
```

If your computer doesn't already have Python installed, or if you want to install a different version of Python, follow the procedure in [Install the AWS CLI version 1 on Linux](install-linux.md)\.

## Install the AWS CLI version 1 Using the Bundled Installer<a name="install-bundle-macos"></a>

Follow these steps from the command line to install the AWS CLI version 1 using the bundled installer\.

**To install the AWS CLI version 1 using the bundled installer**

1. Here are the steps described below in one easy to copy\-and\-paste group\. See the descriptions of each line in the steps that follow\.

   ```
   curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
   unzip awscli-bundle.zip
   sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```
**Note**  
If you don't have `unzip`, use your favorite package manager or an equivalent to install it\.

1. Run the install program\. This command installs the AWS CLI to `/usr/local/aws` and creates the symlink `aws` in the `/usr/local/bin` directory\. Using the `-b` option to create a symlink eliminates the need to specify the install directory in the user's `$PATH` variable\. This should enable all users to call the AWS CLI by typing `aws` from any directory\.

   ```
   $ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```
**Note**  
By default, the install script runs under the system's default version of Python\. If you have installed an alternative version of Python and want to use that to install the AWS CLI, run the install script and specify that version by including the absolute path to the Python application, as shown in the following example\.  

   ```
   $ sudo /usr/local/bin/python3.7 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

To see an explanation of the `-i` and `-b` options, use the `-h` option\.

```
$ ./awscli-bundle/install -h
```

## Install the AWS CLI version 1 on macOS Using pip<a name="awscli-install-osx-pip"></a>

You can also use `pip` directly to install the AWS CLI\. If you don't have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\. Run `pip3 --version` to see if your version of macOS already includes Python and `pip3`\.

```
$ pip3 --version
```

**To install the AWS CLI on macOS**

1. Download and install the latest version of Python from the [downloads page](https://www.python.org/downloads/mac-osx/) of [Python\.org](https://www.python.org)\.

1. Download and run the `pip3` installation script provided by the Python Packaging Authority\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   $ python3 get-pip.py --user
   ```

1. Use your newly installed `pip3` to install the AWS CLI\. We recommend that if you use Python version 3\+, that you use the `pip3` command\.

   ```
   $ pip3 install awscli --upgrade --user
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   AWS CLI 1.16.273 (Python 3.7.3)
   ```

   If the program isn't found, [add it to your command line path](#awscli-install-osx-path)\.

To upgrade to the latest version, run the installation command again\.

```
$ pip3 install awscli --upgrade --user
```

## Add the AWS CLI version 1 Executable to Your macOS Command Line Path<a name="awscli-install-osx-path"></a>

After installing with `pip`, you might need to add the `aws` program to your operating system's `PATH` environment variable\. The location of the program depends on where Python is installed\.

**Example AWS CLI install location \- macOS with Python 3\.6 and `pip` \(user mode\)**  

```
~/Library/Python/3.7/bin
```
Substitute the version of Python that you have for the version in the example above\.

If you don't know where Python is installed, run `which python`\.

```
$ which python
/usr/local/bin/python
```

The output might be the path to a symlink, not the actual program\. Run `ls -al` to see where it points\.

```
$ ls -al /usr/local/bin/python
~/Library/Python/3.7/bin/python3.6
```

`pip` installs programs in the same folder that contains the Python application\. Add this folder to your `PATH` variable\.

**To modify your `PATH` variable \(Linux or macOS\)**

1. Find your shell's profile script in your user folder\. If you're not sure which shell you have, run `echo $SHELL`\.

   ```
   $ ls -a ~
   .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
   ```
   + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`
   + **Zsh** – `.zshrc`
   + **Tcsh** – `.tcshrc`, `.cshrc`, or `.login`

1. Add an export command to your profile script\.

   ```
   export PATH=~/.local/bin:$PATH
   ```

   This command adds a path, `~/.local/bin` in this example, to the current `PATH` variable\.

1. Load the updated profile into your current session\.

   ```
   $ source ~/.bash_profile
   ```