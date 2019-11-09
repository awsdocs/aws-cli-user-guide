# Install the AWS CLI version 1 on Linux<a name="install-linux"></a>

**Important**  
On January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI version 1 with Python 2\.6 or Python 3\.3](deprecate-python-26-33.md) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

You can install version 1 of the AWS Command Line Interface \(AWS CLI\) and its dependencies on most Linux distributions by using `pip`, a package manager for Python\.

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

If you don't already have Python 2 version 2\.7\+ or Python 3 version 3\.4\+, you must first [install Python](install-linux-python.md)\. If you do have Python installed, proceed to installing `pip` and the AWS CLI\.

**Topics**
+ [Install `pip`](#install-linux-pip)
+ [Install the AWS CLI version 1 with `pip`](#install-linux-awscli)
+ [Upgrading to the latest version of the AWS CLI version 1](#install-linux-awscli-upgrade)
+ [Add the AWS CLI version 1 Executable to Your Command Line Path](#install-linux-path)
+ [Installing Python on Linux](install-linux-python.md)
+ [Install the AWS CLI version 1 on Amazon Linux](install-linux-al2017.md)

## Install `pip`<a name="install-linux-pip"></a>

If you don't already have `pip` installed, you can install it by using the script that the *Python Packaging Authority* provides\.

**To install pip**

1. Use the `curl` command to download the installation script\. The following command uses the `-O` \(capital letter O\) parameter to specify that the downloaded file is to be stored in the current folder using the same name it has on the remote host\.

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
   pip 19.2.3 from ~/.local/lib/python3.7/site-packages (python 3.7)
   ```

## Install the AWS CLI version 1 with `pip`<a name="install-linux-awscli"></a>

Use `pip` to install the AWS CLI\.

```
$ pip3 install awscli --upgrade --user
```

When you use the `--user` switch, `pip` installs the AWS CLI to `~/.local/bin`\. 

Verify that the AWS CLI installed correctly\.

```
$ aws --version
aws-cli/1.16.273 Python/3.7.3 Linux/4.14.133-113.105.amzn2.x86_64 botocore/1.13.0
```

If you get an error, see [Troubleshooting AWS CLI Errors](cli-chap-troubleshooting.md)\.

## Upgrading to the latest version of the AWS CLI version 1<a name="install-linux-awscli-upgrade"></a>

We recommend that you regularly check to see if there is a new version of the AWS CLI and upgrade to it when you can\.

Use the `pip list -o` command to check which packages are "outdated':

```
$ aws --version
aws-cli/1.16.170 Python/3.7.3 Linux/4.14.123-111.109.amzn2.x86_64 botocore/1.12.160

$ pip3 list -o
Package    Version  Latest   Type 
---------- -------- -------- -----
awscli     1.16.170 1.16.198 wheel
botocore   1.12.160 1.12.188 wheel
```

Because the previous command shows that there is a newer version of the AWS CLI version 1 available, you can run `pip install --upgrade` to get the latest version:

```
$ pip3 install --upgrade --user awscli
Collecting awscli
  Downloading https://files.pythonhosted.org/packages/dc/70/b32e9534c32fe9331801449e1f7eacba6a1992c2e4af9c82ac9116661d3b/awscli-1.16.198-py2.py3-none-any.whl (1.7MB)
     |████████████████████████████████| 1.7MB 1.6MB/s 
Collecting botocore==1.12.188 (from awscli)
  Using cached https://files.pythonhosted.org/packages/10/cb/8dcfb3e035a419f228df7d3a0eea5d52b528bde7ca162f62f3096a930472/botocore-1.12.188-py2.py3-none-any.whl
Requirement already satisfied, skipping upgrade: docutils>=0.10 in ./venv/lib/python3.7/site-packages (from awscli) (0.14)
Requirement already satisfied, skipping upgrade: rsa<=3.5.0,>=3.1.2 in ./venv/lib/python3.7/site-packages (from awscli) (3.4.2)
Requirement already satisfied, skipping upgrade: colorama<=0.3.9,>=0.2.5 in ./venv/lib/python3.7/site-packages (from awscli) (0.3.9)
Requirement already satisfied, skipping upgrade: PyYAML<=5.1,>=3.10; python_version != "2.6" in ./venv/lib/python3.7/site-packages (from awscli) (3.13)
Requirement already satisfied, skipping upgrade: s3transfer<0.3.0,>=0.2.0 in ./venv/lib/python3.7/site-packages (from awscli) (0.2.0)
Requirement already satisfied, skipping upgrade: jmespath<1.0.0,>=0.7.1 in ./venv/lib/python3.7/site-packages (from botocore==1.12.188->awscli) (0.9.4)
Requirement already satisfied, skipping upgrade: urllib3<1.26,>=1.20; python_version >= "3.4" in ./venv/lib/python3.7/site-packages (from botocore==1.12.188->awscli) (1.24.3)
Requirement already satisfied, skipping upgrade: python-dateutil<3.0.0,>=2.1; python_version >= "2.7" in ./venv/lib/python3.7/site-packages (from botocore==1.12.188->awscli) (2.8.0)
Requirement already satisfied, skipping upgrade: pyasn1>=0.1.3 in ./venv/lib/python3.7/site-packages (from rsa<=3.5.0,>=3.1.2->awscli) (0.4.5)
Requirement already satisfied, skipping upgrade: six>=1.5 in ./venv/lib/python3.7/site-packages (from python-dateutil<3.0.0,>=2.1; python_version >= "2.7"->botocore==1.12.188->awscli) (1.12.0)
Installing collected packages: botocore, awscli
  Found existing installation: botocore 1.12.160
    Uninstalling botocore-1.12.160:
      Successfully uninstalled botocore-1.12.160
  Found existing installation: awscli 1.16.170
    Uninstalling awscli-1.16.170:
      Successfully uninstalled awscli-1.16.170
Successfully installed awscli-1.16.198 botocore-1.12.188
```

## Add the AWS CLI version 1 Executable to Your Command Line Path<a name="install-linux-path"></a>

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