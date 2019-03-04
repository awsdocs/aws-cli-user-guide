# Install the AWS CLI on Linux<a name="install-linux"></a>

You can install the AWS Command Line Interface \(AWS CLI\) and its dependencies on most Linux distributions by using `pip`, a package manager for Python\.

**Important**  
The `awscli` package is available in repositories for other package managers such as `apt` and `yum`, but you're not assured of getting the latest version unless you get it from `pip` or use the [bundled installer](install-bundle.md)\.

If you already have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\. Run `pip --version` to see if your version of Linux already includes Python and `pip`\. We recommend that if you have Python version 3\+ installed, that you use the `pip3` command\.

```
$ pip3 --version
```

If you don't already have `pip` installed, check which version of Python is installed\.

```
$ python --version
```

**or**

```
$ python3 --version
```

If you don't already have Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+, you must first [install Python](install-linux-python.md)\. If you do have Python installed, proceed to installing `pip` and the AWS CLI\.

**Topics**
+ [Install `pip`](#install-linux-pip)
+ [Install the AWS CLI with `pip`](#install-linux-awscli)
+ [Add the AWS CLI Executable to Your Command Line Path](#install-linux-path)
+ [Installing Python on Linux](install-linux-python.md)
+ [Install the AWS CLI on Amazon Linux](install-linux-al2017.md)

## Install `pip`<a name="install-linux-pip"></a>

If you don't already have `pip` installed, you can install it by using the script that the *Python Packaging Authority* provides\.

**To install pip**

1. Use the `curl` command to download the installation script\.

   ```
   $ curl -O https://bootstrap.pypa.io/get-pip.py
   ```

1. Run the script with Python to download and install the latest version of `pip` and other required support packages\.

   ```
   $ python get-pip.py --user
   ```

   or

   ```
   $ python3 get-pip.py --user
   ```

   When you include the `--user` switch, the script installs `pip` to the path `~/.local/bin`\.

1. Ensure the folder that contains `pip` is part of your `PATH` variable\.

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

1. Now you can test to verify that `pip` is installed correctly\.

   ```
   $ pip3 --version
   pip 19.0.3 from ~/.local/lib/python3.7/site-packages (python 3.7)
   ```

## Install the AWS CLI with `pip`<a name="install-linux-awscli"></a>

Use `pip` to install the AWS CLI\.

```
$ pip3 install awscli --upgrade --user
```

When you use the `--user` switch, `pip` installs the AWS CLI to `~/.local/bin`\. 

Verify that the AWS CLI installed correctly\.

```
$ aws --version
aws-cli/1.16.116 Python/3.6.8 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.106
```

If you get an error, see [Troubleshooting AWS CLI Errors](cli-chap-troubleshooting.md)\.

To upgrade to the latest version, run the installation command again\.

```
$ pip3 install awscli --upgrade --user
```

## Add the AWS CLI Executable to Your Command Line Path<a name="install-linux-path"></a>

After installing with `pip`, you might need to add the `aws` executable to your operating system' `PATH` environment variable\.

You can verify which folder `pip` installed the AWS CLI to by running the following command\.

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

If this is the same folder you added to the path in step 3 in [Install `pip`](#install-linux-pip), you're done\. Otherwise, perform those same steps 3a–3c again, adding this additional folder to the path\.