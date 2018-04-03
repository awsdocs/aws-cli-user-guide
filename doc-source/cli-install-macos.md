# Install the AWS Command Line Interface on macOS<a name="cli-install-macos"></a>

If you have pip, follow the instructions in the main installation topic\. Run `pip --version` to see if your version of macOS already includes Python and pip\.

```
$ pip --version
```


+ [Install Python, pip, and the AWS CLI on macOS](#awscli-install-osx-pip)
+ [Adding the AWS CLI Executable to your Command Line Path](#awscli-install-osx-path)

## Install Python, pip, and the AWS CLI on macOS<a name="awscli-install-osx-pip"></a>

You can install the latest version of Python and pip and then use them to install the AWS CLI\.

**To install the AWS CLI on macOS**

1. Download and install Python 3\.6 from the [downloads page](https://www.python.org/downloads/release/python-361/) of [Python\.org](https://www.python.org)\.

1. Install `pip` with the script provided by the Python Packaging Authority\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   $ python3 get-pip.py --user
   ```

1. Use `pip` to install the AWS CLI\.

   ```
   $ pip install awscli --upgrade --user
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   $ aws --version
   AWS CLI 1.11.84 (Python 3.6.1)
   ```

   If the executable is not found, add it to your command line path\.

To upgrade to the latest version, run the installation command again:

```
$ pip install awscli --upgrade --user
```

## Adding the AWS CLI Executable to your Command Line Path<a name="awscli-install-osx-path"></a>

After installing with `pip`, you may need to add the `aws` executable to your OS's `PATH` environment variable\. The location of the executable depends on where Python is installed\.

**Example AWS CLI install location \- macOS with Python 3\.6 and pip \(user mode\)**  

```
~/Library/Python/3.6/bin
```

If you don't know where Python is installed, run `which python`\.

```
$ which python
/usr/local/bin/python
```

The output may be the path to a symlink, not the actual executable\. Run `ls -al` to see where it points\.

```
$ ls -al /usr/local/bin/python
~/Library/Python/3.6/bin/python3.6
```

`pip` installs executables to the same folder that contains the Python executable\. Add this folder to your PATH variable\.

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
