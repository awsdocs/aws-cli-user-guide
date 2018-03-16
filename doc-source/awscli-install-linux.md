# Install the AWS Command Line Interface on Linux<a name="awscli-install-linux"></a>

You can install the AWS Command Line Interface and its dependencies on most Linux distributions with `pip`, a package manager for Python\.

**Important**  
The `awscli` package is available in repositories for other package managers such as APT and yum, but it is not guaranteed to be the latest version unless you get it from `pip` or use the bundled installer

If you already have pip, follow the instructions in the main installation topic\. Run `pip --version` to see if your version of Linux already includes Python and pip\.

```
$ pip --version
```

If you don't have pip, check to see which version of Python is installed\.

```
$ python --version
```

or

```
$ python3 --version
```

If you don't have Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+, install Python\. Otherwise, install pip and the AWS CLI\.


+ [Installing Python on Linux](awscli-install-linux-python.md)
+ [Installing the AWS Command Line Interface on Amazon Linux 2017](awscli-install-linux-al2017.md)
+ [Installing Pip](#awscli-install-linux-pip)
+ [Installing the AWS CLI with Pip](#awscli-install-linux-awscli)
+ [Adding the AWS CLI Executable to your Command Line Path](#awscli-install-linux-path)

## Installing Pip<a name="awscli-install-linux-pip"></a>

If you don't have `pip`, install `pip` with the script provided by the Python Packaging Authority\.

**To install pip**

1. Download the installation script from [pypa\.io](https://www.pypa.io/):

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

   The script downloads and installs the latest version of `pip` and another required package named `setuptools`\. 

1. Run the script with Python:

   ```
   $ python get-pip.py --user
   ```

   or

   ```
   $ python3 get-pip.py --user
   ```

1. Add the executable path to your PATH variable: `~/.local/bin`

**To modify your PATH variable \(Linux, macOS, or Unix\)**

   1. Find your shell's profile script in your user folder\. If you are not sure which shell you have, run `echo $SHELL`\.

      ```
      $ ls -a ~
      .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
      ```

      + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`\.

      + **Zsh** – `.zshrc`

      + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`\.

   1. Add an export command to your profile script\.

      ```
      export PATH=~/.local/bin:$PATH
      ```

      This command adds a path, `~/.local/bin` in this example, to the current PATH variable\.

   1. Load the profile into your current session\.

      ```
      $ source ~/.bash_profile
      ```

1. Verify that pip is installed correctly\.

   ```
   $ pip --version
   pip 8.1.2 from ~/.local/lib/python3.4/site-packages (python 3.4)
   ```

## Installing the AWS CLI with Pip<a name="awscli-install-linux-awscli"></a>

Use `pip` to install the AWS CLI\.

```
$ pip install awscli --upgrade --user
```

Verify that the AWS CLI installed correctly\.

```
$ aws --version
aws-cli/1.11.84 Python/3.6.2 Linux/4.4.0-59-generic botocore/1.5.47
```

If you get an error, see \.

To upgrade to the latest version, run the installation command again:

```
$ pip install awscli --upgrade --user
```

## Adding the AWS CLI Executable to your Command Line Path<a name="awscli-install-linux-path"></a>

After installing with `pip`, you may need to add the `aws` executable to your OS's `PATH` environment variable\.

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
~/.local/Python/3.6/bin/python3.6
```

**To modify your PATH variable \(Linux, macOS, or Unix\)**

1. Find your shell's profile script in your user folder\. If you are not sure which shell you have, run `echo $SHELL`\.

   ```
   $ ls -a ~
   .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
   ```

   + **Bash** – `.bash_profile`, `.profile`, or `.bash_login`\.

   + **Zsh** – `.zshrc`

   + **Tcsh** – `.tcshrc`, `.cshrc` or `.login`\.

1. Add an export command to your profile script\.

   ```
   export PATH=~/.local/bin:$PATH
   ```

   This command adds a path, `~/.local/bin` in this example, to the current PATH variable\.

1. Load the profile into your current session\.

   ```
   $ source ~/.bash_profile
   ```