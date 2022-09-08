--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Install, Update, and Uninstall the AWS CLI version 1 on Linux<a name="install-linux"></a>

You can install the AWS Command Line Interface \(AWS CLI\) version 1 and its dependencies on most Linux distributions by using the `pip` package manager or the bundled installer\.

Although the `awscli` package is available in repositories for other package managers such as `apt` and `yum`, these are not produced, managed, or supported by AWS\. We recommend that you install the AWS CLI from only the official AWS distribution points, as documented in this guide\.

**Topics**
+ [Prerequisites](#install-linux-prereqs)
+ [Install and uninstall the AWS CLI version 1 on Linux using the bundled installer](#install-linux-bundled)
+ [Install and uninstall the AWS CLI version 1 using pip](#install-linux-pip)

## Prerequisites<a name="install-linux-prereqs"></a>

You must have Python 3\.6 or later installed\. For installation instructions, see the [Downloading Python](https://wiki.python.org/moin/BeginnersGuide/Download) page in Python's *Beginner Guide*\.

**Warning**  
Python 2\.7 was deprecated by the [Python Software Foundation](https://www.python.org/psf-landing/) on January 1, 2020\. Starting with AWS CLI version 1\.20\.0, a minimum version of Python 3\.6 is required\.  
In order to use the AWS CLI version 1 with an older version of Python, you need to install an earlier version of the AWS CLI version 1\. To view the AWS CLI version 1 Python version support matrix, see [Python version requirements](cli-chap-install.md#cli-chap-install-python)\. 

## Install and uninstall the AWS CLI version 1 on Linux using the bundled installer<a name="install-linux-bundled"></a>

On Linux or macOS, you can use the bundled installer to install version 1 of the AWS CLI\. The bundled installer includes all dependencies and can be used offline\.

**Note**  
The bundled installer doesn't support installing to paths that contain spaces\.

**Topics**
+ [Install the AWS CLI version 1 using the bundled installer with `sudo`](#install-linux-bundled-sudo)
+ [Install the AWS CLI version 1 using the bundled installer without `sudo`](#install-linux-bundled-no-sudo)
+ [Uninstall the AWS CLI version 1 bundled installer](#install-linux-bundled-uninstall)

### Install the AWS CLI version 1 using the bundled installer with `sudo`<a name="install-linux-bundled-sudo"></a>

The following steps enable you to install the AWS CLI version 1 from the command line on any build of Linux or macOS\.

The following is a summary of the installation commands explained below that you can cut and paste to run as a single set of commands\.

**For the latest version of the AWS CLI,** use the following command block:

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

**For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-bundle-1.16.312.zip` resulting in the following command:

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

Follow these steps from the command line to install the AWS CLI version 1 using the bundled installer\.

**To install the AWS CLI version 1 using the bundled installer**

1. Download the AWS CLI version 1 bundled installer using one of the the following methods\.
   + Download using the `curl` command\.

     **For the latest version of the AWS CLI,** use the following command block:

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
     ```

     **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-bundle-1.16.312.zip` resulting in the following command:

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip" -o "awscli-bundle.zip"
     ```
   + Download using the direct link\.

     **For the latest version of the AWS CLI:** [https://s3.amazonaws.com/aws-cli/awscli-bundle.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)

     **For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-bundle-1.16.312.zip` resulting in the following url [https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip)

1. Extract the files from the package\. If you don't have `unzip` to extract the files, use your Linux distribution's built\-in package manager to install it\.

   ```
   $ unzip awscli-bundle.zip
   ```

1. Run the install program\. The installer installs the AWS CLI at `/usr/local/aws` and creates the symlink `aws` at the `/usr/local/bin` directory\. Using the `-b` option to create a symlink eliminates the need to specify the install directory in the user's `$PATH` variable\. This should enable all users to call the AWS CLI by entering `aws` from any directory\.

   ```
   $ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

   By default, the install script runs under the system default version of Python\. If you have installed an alternative version of Python and want to use that version to install the AWS CLI, run the install script with that version by absolute path to the Python executable, as follows\.

   ```
   $ sudo /usr/local/bin/python3.7 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

1. Verify that the AWS CLI installed correctly\.

   ```
   $ aws --version
   aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

   If you get an error, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### Install the AWS CLI version 1 using the bundled installer without `sudo`<a name="install-linux-bundled-no-sudo"></a>

If you don't have `sudo` permissions or want to install the AWS CLI only for the current user, you can use a modified version of the previous commands\. The first two commands are the same\. 

**For the latest version of the AWS CLI,** use the following command block:

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
./awscli-bundle/install -b ~/bin/aws
```

**For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-bundle-1.16.312.zip` resulting in the following command:

```
$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
./awscli-bundle/install -b ~/bin/aws
```

**To install the AWS CLI version 1 for current user**

1. Download the AWS CLI version 1 bundled installer in one of the following ways\.
   + Download using the `curl` command\.

     **For the latest version of the AWS CLI,** use the following command block:

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
     ```

     **For a specific version of the AWS CLI,** append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-bundle-1.16.312.zip` resulting in the following command:

     ```
     $ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle-1.16.312.zip" -o "awscli-bundle.zip"
     ```
   + Download using the direct link\.

     **For the latest version of the AWS CLI:** [https://s3.amazonaws.com/aws-cli/awscli-bundle.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)

     **For a specific version of the AWS CLI, **append a hyphen and the version number to the filename\. For this example the filename for version *1\.16\.312* would be `awscli-exe-linux-aarch64-2.0.30.zip` resulting in the following url [https://s3.amazonaws.com/aws-cli/awscli-bundle-2.0.30.zip](https://s3.amazonaws.com/aws-cli/awscli-bundle-2.0.30.zip)

1. Extract the files from the package by using `unzip`\. If you don't have `unzip`, use your Linux distribution's built\-in package manager to install it\.

   ```
   $ unzip awscli-bundle.zip
   ```

1. Run the install program\. The installer installs the AWS CLI at `/usr/local/aws` and creates the symlink `aws` at the `/usr/local/bin` directory\. The command uses the `-b` parameter to specify the directory where the installer places the `aws` symlink file\. You must have write permissions to the specified folder\.

   ```
   $ ./awscli-bundle/install -b ~/bin/aws
   ```

   This installs the AWS CLI to the default location \(`~/.local/lib/aws`\) and creates a symbolic link \(symlink\) at `~/bin/aws`\. Make sure that `~/bin` is in your `PATH` environment variable for the symlink to work\.

   ```
   $ echo $PATH | grep ~/bin     // See if $PATH contains ~/bin (output will be empty if it doesn't)
   $ export PATH=~/bin:$PATH     // Add ~/bin to $PATH if necessary
   ```

1. Ensure the directory that the AWS CLI version 1 is part of your `PATH` variable\.

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
   aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

   If you get an error, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### Uninstall the AWS CLI version 1 bundled installer<a name="install-linux-bundled-uninstall"></a>

1. If you installed the AWS CLI using the bundled installer, follow these instructions\. The bundled installer doesn't put anything outside of the installation directory except the optional symlink, so uninstalling is as simple as deleting those two items\.

   ```
   $ sudo rm -rf /usr/local/aws
   $ sudo rm /usr/local/bin/aws
   ```

1. **\(Optional\)** Remove the shared AWS SDK and AWS CLI settings information in the `.aws` folder\.
**Warning**  
These configuration and credentials settings are shared across all AWS SDKs and the AWS CLI\. If you remove this folder, they cannot be accessed by any AWS SDKs that are still on your system\.

   The default location of the `.aws` folder differs between platforms, by default the folder is located in *\~/\.aws/*\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm ~/.aws/
   ```

## Install and uninstall the AWS CLI version 1 using pip<a name="install-linux-pip"></a>

**Topics**
+ [Install pip](#install-linux-pip-pip)
+ [Install and update the AWS CLI version 1 using pip](#install-linux-awscli)
+ [Add the AWS CLI version 1 executable to your command line path](#install-linux-path)
+ [Uninstall the AWS CLI using pip](#post-install-uninstall)
+ [Troubleshooting AWS CLI install and uninstall errors](#install-linux-tshoot)

### Install pip<a name="install-linux-pip-pip"></a>

If you don't already have `pip` installed, you can install it by using the script that the *Python Packaging Authority* provides\. Run `pip --version` to see if your version of Linux already includes Python and `pip`\. We recommend that if you have Python version 3 or later installed, you use the `pip3` command\.

1. Use the `curl` command to download the installation script\. The following command uses the `-O` \(uppercase "O"\)parameter to specify that the downloaded file is to be stored in the current directory using the same name it has on the remote host\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

1. Run the script with the `python` or `python3` command to download and install the latest version of `pip` and other required support packages\. When you include the `--user` switch, the script installs `pip` to the path `~/.local/bin`\.

   ```
   $ python3 get-pip.py --user
   ```

1. Ensure the directory that contains `pip` is part of your `PATH` variable\.

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

1. To verify that `pip` or `pip3` is installed correctly, run the following command\.

   ```
   $ pip3 --version
   pip 19.2.3 from ~/.local/lib/python3.7/site-packages (python 3.7)
   ```

### Install and update the AWS CLI version 1 using pip<a name="install-linux-awscli"></a>

1. Use the `pip` or `pip3` command to install or update the AWS CLI\. We recommend that if you use Python version 3 or later that you use the `pip3` command\. The `--user` switch, `pip` installs the AWS CLI to `~/.local/bin`\. 

   **For the latest version of the AWS CLI,** use the following command block:

   ```
   $ pip3 install awscli --upgrade --user
   ```

   **For a specific version of the AWS CLI,** append two equals signs `=` and the version number to the filename\. For this example the filename for version *1\.16\.312* would be *==1\.16\.312* resulting in the following command:

   ```
   $ pip3 install awscli==1.16.312 --upgrade --user
   ```
**Note**  
Use appropriate quoting rules for your terminal\. In order to use the `=` character, you might need to use single or double quotes to escape properly\. The following example escapes using single quotes:  

   ```
   $ pip3 install 'awscli==1.16.312' --upgrade --user
   ```

1. Verify that the AWS CLI installed correctly\.

   ```
   $ aws --version
   aws-cli/1.25.55 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13
   ```

   If you get an error, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md)\.

### Add the AWS CLI version 1 executable to your command line path<a name="install-linux-path"></a>

After installing with `pip`, you might need to add the `aws` executable to your operating system' `PATH` environment variable\.

You can verify which folder `pip` installed the AWS CLI in by running the following command\.

```
$ which aws
/home/username/.local/bin/aws
```

You can reference this as `~/.local/bin/` because `/home/username` corresponds to `~` in Linux\.

If you omitted the `--user` switch and so didn't install in user mode, the executable might be in the `bin` folder of your Python installation\. If you don't know where Python is installed, run this command\.

```
$ which python
/usr/local/bin/python
```

The output might be the path to a symlink, not to the actual executable\. Run `ls -al` to see where it points\.

```
$ ls -al /usr/local/bin/python
/usr/local/bin/python -> ~/.local/Python/3.6/bin/python3.6
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

### Uninstall the AWS CLI using pip<a name="post-install-uninstall"></a>

1. If you installed the AWS CLI using `pip` or `pip3`, you need to uninstall the AWS CLI using the same package manager by running one of the following commands\.

   ```
   $ pip uninstall awscli
   ```

   ```
   $ pip3 uninstall awscli
   ```

1. **\(Optional\)** Remove the shared AWS SDK and AWS CLI settings information in the `.aws` folder\.
**Warning**  
These configuration and credentials settings are shared across all AWS SDKs and the AWS CLI\. If you remove this folder, they cannot be accessed by any AWS SDKs that are still on your system\.

   The default location of the `.aws` folder differs between platforms, by default the folder is located in *\~/\.aws/*\. If your user account has write permission to this directory, you don't need to use `sudo`\.

   ```
   $ sudo rm ~/.aws/
   ```

### Troubleshooting AWS CLI install and uninstall errors<a name="install-linux-tshoot"></a>

If you come across issues after installing or uninstalling the AWS CLI, see [Troubleshooting AWS CLI errors](cli-chap-troubleshooting.md) for troubleshooting steps\. For the most relevant troubleshooting steps, see [Command not found errors](cli-chap-troubleshooting.md#tshoot-install-not-found), [The "`aws --version`" command returns a different version than you installed](cli-chap-troubleshooting.md#tshoot-install-wrong-version), and [The "`aws --version`" command returns a version after uninstalling the AWS CLI](cli-chap-troubleshooting.md#tshoot-uninstall-1)\.