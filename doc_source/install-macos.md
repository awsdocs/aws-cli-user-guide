# Install, Update, and Uninstall the AWS CLI version 1 on macOS<a name="install-macos"></a>

You can install the AWS Command Line Interface \(AWS CLI\) version 1 and its dependencies on macOS by using the bundled installer or `pip`\. 

**Topics**
+ [Prerequisites](#install-macosos-prereq)
+ [Install, update and uninstall the AWS CLI version 1 on macOS using the bundled installer](#install-macosos-bundled)
+ [Install, update and uninstall the AWS CLI version 1 using pip](#awscli-install-osx-pip)

## Prerequisites<a name="install-macosos-prereq"></a>

Before you can install the AWS CLI version 1 on macOS, be sure you have Python 2 version 2\.7 or later, or Python 3 version 3\.4 or later installed\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.

**Important**  
AWS CLI version 1 no longer supports Python versions 2\.6 and 3\.3\. All versions of the AWS CLI version 1 released after January 10th, 2020, starting with 1\.17, require Python 2\.7, Python 3\.4, or a later version\.  
This change does not affect the Windows MSI installer version of the AWS CLI version 1 and the AWS CLI version 2\.  
For more information, see [Using the AWS CLI version 1 with earlier versions of Python](deprecate-old-python-versions.md) in this guide, and the [deprecation announcement](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/) blog post\.

## Install, update and uninstall the AWS CLI version 1 on macOS using the bundled installer<a name="install-macosos-bundled"></a>

On Linux or macOS, you can use the bundled installer to install version 1 of the AWS Command Line Interface \(AWS CLI\)\. The bundled installer includes all dependencies and can be used offline\.

The bundled installer doesn't support installing to paths that contain spaces\.

**Topics**
+ [Install the AWS CLI version 1 using the bundled installer with `sudo`](#install-macosos-bundled-sudo)
+ [Install the AWS CLI version 1 using the bundled installer without `sudo`](#install-macosos-bundled-no-sudo)
+ [Uninstall the AWS CLI version 1 bundled installer](#install-macosos-bundled-uninstall)

### Install the AWS CLI version 1 using the bundled installer with `sudo`<a name="install-macosos-bundled-sudo"></a>

The following steps enable you to install the AWS CLI version 1 from the command line on any build of macOS\.

The following is a summary of the installation commands that you can cut and paste to run as a single set of commands\.

```
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

**To install the AWS CLI version 1 using the bundled installer**

1. Download the AWS CLI version 1 bundled installer in one of the following ways:
   + Download using the `curl` command\.

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
     ```
   + Download using the direct link: [https://s3.amazonaws.com/aws-cli/awscli-bundle.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)\.

1. Extract \(unzip\) the files from the package\. If you don't have `unzip`, use your macOs distribution's built\-in package manager to install it\.

   ```
   $ unzip awscli-bundle.zip
   ```

1. Run the install program\. The installer installs the AWS CLI at `/usr/local/aws` and creates the symlink `aws` at the `/usr/local/bin` folder\. Using the `-b` option to create a symlink eliminates the need to specify the install folder in the user's `$PATH` variable\. This should enable all users to call the AWS CLI by entering `aws` from any directory\.

   ```
   $ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

   By default, the install script runs under the system default version of Python\. If you have installed an alternative version of Python and want to use that to install the AWS CLI, run the install script with that version by absolute path to the Python executable, as follows\.

   ```
   $ sudo /usr/local/bin/python3.7 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

1. Verify that the AWS CLI installed correctly\.

   ```
   $ aws --version
   aws-cli/1.18.134 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

   If you get an error, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### Install the AWS CLI version 1 using the bundled installer without `sudo`<a name="install-macosos-bundled-no-sudo"></a>

If you don't have `sudo` permissions or want to install the AWS CLI only for the current user, you can use a modified version of the previous commands\. The first two commands are the same\. 

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
$ unzip awscli-bundle.zip
$ ./awscli-bundle/install -b ~/bin/aws
```

**To install the AWS CLI version 1 for the current user**

1. Download the AWS CLI version 1 bundled installer using one of the the following methods:
   + Download using the `curl` command\.

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
     ```
   + Download using the direct link: [https://s3.amazonaws.com/aws-cli/awscli-bundle.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)\.

1. Extract the files from the package\. If you don't have `unzip`, use your Linux distribution's built\-in package manager to install it\.

   ```
   $ unzip awscli-bundle.zip
   ```

1. Run the install program\. The installer installs the AWS CLI at `/usr/local/aws` and creates the symlink `aws` at the `/usr/local/bin` directory\. The command uses the `-b` parameter to specify the directory where the installer places the `aws` symlink file\. You must have write permissions to the specified directory\.

   ```
   $ ./awscli-bundle/install -b ~/bin/aws
   ```

   This installs the AWS CLI to the default location \(`~/.local/lib/aws`\) and creates a symbolic link \(symlink\) at `~/bin/aws`\. Make sure that `~/bin` is in your `$PATH` environment variable for the symlink to work\.

   ```
   $ echo $PATH | grep ~/bin     // See if $PATH contains ~/bin (output will be empty if it doesn't)
   $ export PATH=~/bin:$PATH     // Add ~/bin to $PATH if necessary
   ```

1. Ensure the folder that the AWS CLI version 1 is installed in is part of your `$PATH` variable\.

   1. Find your shell's profile script in your user folder\. If you're not sure which shell you have, run `echo $SHELL`\.

      ```
      $ ls -a ~
      .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
      ```
      + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`
      + **Zsh** – `.zshrc`
      + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`

   1. Add an export command at the end of your profile script that's similar to the following example\.

      ```
      export PATH=~/.local/bin:$PATH
      ```

      This command inserts the path, `~/.local/bin` in this example, at the front of the existing `PATH` variable\.

   1. Reload the profile into your current session to put those changes into effect\.

      ```
      $ source ~/.bash_profile
      ```

1. Verify that the AWS CLI installed correctly\.

   ```
   $ aws --version
   aws-cli/1.18.134 Python/3.7.4 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

   If you get an error, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### Uninstall the AWS CLI version 1 bundled installer<a name="install-macosos-bundled-uninstall"></a>

The bundled installer puts everything inside of the installation directory except the optional symlink, so to uninstall, you just need to delete those two items\.

```
$ sudo rm -rf /usr/local/aws
$ sudo rm /usr/local/bin/aws
```

## Install, update and uninstall the AWS CLI version 1 using pip<a name="awscli-install-osx-pip"></a>

You can use `pip` directly to install the AWS CLI\. 

**Topics**
+ [Install pip](#awscli-install-osx-pip-pip)
+ [Install and update the AWS CLI using pip](#awscli-install-osx-pip-install)
+ [Add the AWS CLI version 1 executable to your macOS command line path](#awscli-install-osx-path)
+ [Uninstall the AWS CLI using pip](#awscli-install-osx-pip-uninstall)

### Install pip<a name="awscli-install-osx-pip-pip"></a>

If you don't already have `pip` installed, you can install it by using the script that the *Python Packaging Authority* provides\. Run `pip --version` to see if your version of Linux already includes Python and `pip`\. We recommend that if you have Python version 3 or later installed, you use the `pip3` command\.

1. Use the `curl` command to download the installation script\. The following command uses the `-O` \(uppercase "O"\) parameter to specify that the downloaded file is to be stored in the current folder using the same name it has on the remote host\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

1. Run the script with the `python` or `python3` command to download and install the latest version of `pip` and other required support packages\. When you include the `--user` switch, the script installs `pip` to the path `~/.local/bin`\.

   ```
   $ python3 get-pip.py --user
   ```

### Install and update the AWS CLI using pip<a name="awscli-install-osx-pip-install"></a>

1. Use the `pip` or `pip3` command to install the AWS CLI\. We recommend that if you use Python version 3 or later, that you use the `pip3` command\.

   ```
   $ pip3 install awscli --upgrade --user
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   aws-cli/1.18.134 Python/3.7.4 Darwin/18.7.0 botocore/1.13
   ```

   If the program isn't found, [add it to your command line path](#awscli-install-osx-path)\.

### Add the AWS CLI version 1 executable to your macOS command line path<a name="awscli-install-osx-path"></a>

After installing with `pip`, you may need to add the `aws` program to your operating system's `PATH` environment variable\. The location of the program depends on where Python is installed\.

**Example AWS CLI install location \- macOS with Python 3\.6 and `pip` \(user mode\)**  

```
~/Library/Python/3.7/bin
```
Substitute the version of Python that you have for the version in the previous example\.

If you don't know where Python is installed, run `which python`\.

```
$ which python
/usr/local/bin/python
```

The output might be the path to a symlink, not the actual program\. Run `ls -al` to see where it points\.

```
$ ls -al /usr/local/bin/python
~/Library/Python/3.7/bin/python3.7
```

`pip` installs programs in the same folder that contains the Python application\. Add this folder to your `PATH` variable\.

**To modify your `PATH` variable**

1. Find your shell's profile script in your user directory\. If you're not sure which shell you have, run `echo $SHELL`\.

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

### Uninstall the AWS CLI using pip<a name="awscli-install-osx-pip-uninstall"></a>

If you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ pip3 uninstall awscli
```