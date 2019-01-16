# Install the AWS CLIon macOS<a name="install-macos"></a>

The recommended way to install the AWS Command Line Interface \(AWS CLI\) on macOS is to use the bundled installer\. The bundled installer includes all dependencies and you can use it offline\.

**Important**  
The bundled installer doesn't support installing to paths that contain spaces\.

**Topics**
+ [Prerequisites](#install-bundle-macos-os-prereq)
+ [Install the AWS CLI Using the Bundled Installer](#install-bundle-macos)
+ [Install the AWS CLI on macOS Using pip](#awscli-install-osx-pip)
+ [Add the AWS CLI Executable to Your Command Line Path](#awscli-install-osx-path)

## Prerequisites<a name="install-bundle-macos-os-prereq"></a>
+ Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+

Check your Python installation\.

```
$ python --version
```

If your computer doesn't already have Python installed, or if you want to install a different version of Python, follow the procedure in [Install the AWS CLI on Linux](install-linux.md)\.

## Install the AWS CLI Using the Bundled Installer<a name="install-bundle-macos"></a>

Follow these steps from the command line to install the AWS CLI using the bundled installer\.

**To install the AWS CLI using the bundled installer**

1. Download the [AWS CLI Bundled Installer](https://s3.amazonaws.com/aws-cli/awscli-bundle.zip)\.

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
By default, the install script runs under the system's default version of Python\. If you have installed an alternative version of Python and want to use that to install the AWS CLI, run the install script and specify that version by including the absolute path to the Python application\. For example:  

   ```
   $ sudo /usr/local/bin/python3.6 awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
   ```

This command installs the AWS CLI to `/usr/local/aws` and creates the symlink `aws` in the `/usr/local/bin` directory\. Using the `-b` option to create a symlink eliminates the need to specify the install directory in the user's `$PATH` variable\. This should enable all users to call the AWS CLI by typing `aws` from any directory\.

To see an explanation of the `-i` and `-b` options, use the `-h` option\.

```
$ ./awscli-bundle/install -h
```

## Install the AWS CLI on macOS Using pip<a name="awscli-install-osx-pip"></a>

You can also use `pip` directly to install the AWS CLI\. If you don't have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\. Run `pip --version` to see if your version of macOS already includes Python and `pip`\.

```
$ pip --version
```

**To install the AWS CLI on macOS**

1. Download and install Python 3\.6 from the [downloads page](https://www.python.org/downloads/mac-osx/) of [Python\.org](https://www.python.org)\.

1. Download and run the `pip` installation script provided by the Python Packaging Authority\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   $ python3 get-pip.py --user
   ```

1. Use your newly installed `pip` to install the AWS CLI\.

   ```
   $ pip install awscli --upgrade --user
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   AWS CLI 1.16.71 (Python 3.7.1)
   ```

   If the program isn't found, [add it to your command line path](#awscli-install-osx-path)\.

To upgrade to the latest version, run the installation command again\.

```
$ pip install awscli --upgrade --user
```

## Add the AWS CLI Executable to Your Command Line Path<a name="awscli-install-osx-path"></a>

After installing with `pip`, you might need to add the `aws` program to your operating system's `PATH` environment variable\. The location of the program depends on where Python is installed\.

**Example AWS CLI install location \- macOS with Python 3\.7 and `pip` \(user mode\)**  

```
~/Library/Python/3.7/bin
```

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

`pip` installs programs in the same folder that contains the Python application\. Add this folder to your PATH variable\.

**To modify your `PATH` variable \(Linux, macOS, or Unix\)**

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

1. Load the profile into your current session\.

   ```
   $ source ~/.bash_profile
   ```