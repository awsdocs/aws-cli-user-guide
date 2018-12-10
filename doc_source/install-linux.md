# Install the AWS Command Line Interface on Linux<a name="install-linux"></a>

You can install the AWS Command Line Interface and its dependencies on most Linux distributions with `pip`, a package manager for Python\.

**Important**  
The `awscli` package is available in repositories for other package managers such as APT and yum, but you are not guaranteed to get the latest version unless you get it from `pip` or use the [bundled installer](install-bundle.md)\.

If you already have pip, follow the instructions in the main [installation topic](cli-chap-install.md)\. Run `pip --version` to see if your version of Linux already includes Python and pip\.

```
$ pip --version
```

If you don't have pip, check to see which version of Python is installed\.

```
$ python --version
```

**or**

```
$ python3 --version
```

If you don't already have Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+, you must [install Python](install-linux-python.md)\. If you do already have Python installed, then proceed to installing pip and the AWS CLI\.

**Topics**
+ [Installing Pip](#install-linux-pip)
+ [Installing the AWS CLI with Pip](#install-linux-awscli)
+ [Adding the AWS CLI Executable to your Command Line Path](#install-linux-path)
+ [Installing Python on Linux](install-linux-python.md)
+ [Installing the AWS Command Line Interface on Amazon Linux](install-linux-al2017.md)

## Installing Pip<a name="install-linux-pip"></a>

If you don't already have `pip` installed, you can install it with the script provided by the *Python Packaging Authority*\.

**To install pip**

1. Use the `curl` command to download the installation script:

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

1. The script downloads and installs the latest version of `pip` and another required package named `setuptools`\. Run the script with Python:

   ```
   $ python get-pip.py --user
   ```

1. Add the executable path to your PATH variable: `~/.local/bin`

   1. Find your shell's profile script in your user folder\. If you are not sure which shell you have, run `echo $SHELL`\.

      ```
      $ ls -a ~
      .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
      ```
      + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`\.
      + **Zsh** – `.zshrc`
      + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`\.

   1. Add an export command at the end of your profile script\.

      ```
      export PATH=~/.local/bin:$PATH
      ```

      This command adds the path, `~/.local/bin` in this example, to the current PATH variable\.

   1. Reload the profile into your current session to put those changes into effect\.

      ```
      $ source ~/.bash_profile
      ```

1. Now you can test to verify that pip is installed correctly\.

   ```
   $ pip --version
   pip 18.1 from ~/.local/lib/python3.7/site-packages (python 3.7)
   ```

## Installing the AWS CLI with Pip<a name="install-linux-awscli"></a>

Use `pip` to install the AWS CLI\.

```
$ pip install awscli --upgrade --user
```

Verify that the AWS CLI installed correctly\.

```
$ aws --version
aws-cli/1.16.67 Python/3.7.1 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.57
```

If you get an error, see [Troubleshooting AWS CLI Errors](troubleshooting.md)\.

To upgrade to the latest version, run the installation command again:

```
$ pip install awscli --upgrade --user
```

## Adding the AWS CLI Executable to your Command Line Path<a name="install-linux-path"></a>

After installing with `pip`, you might need to add the `aws` executable to your OS's `PATH` environment variable\.

**Example AWS CLI install location \- Linux with pip \(user mode\)**  

```
~/.local/bin
```

If you didn't install in user mode, the executable might be in the `bin` folder of your Python installation\. If you don't know where Python is installed, run `which python`\.

```
$ which python
/usr/local/bin/python
```

The output may be the path to a symlink, not the actual executable\. Run `ls -al` to see where it points\.

```
$ ls -al /usr/local/bin/python
~/.local/Python/3.7/bin/python3.7
```

If this is the same folder you added to the path in step 3 in [Installing Pip](#install-linux-pip), then you are done\. Otherwise, perform those same steps 3a thru 3c again, adding this folder to the path\.