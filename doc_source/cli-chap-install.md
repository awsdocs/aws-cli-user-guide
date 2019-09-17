# Installing the AWS CLI<a name="cli-chap-install"></a>

**Ways to install the AWS Command Line Interface \(AWS CLI\)**
+ [`Using pip`](#install-tool-pip)
+ [Using a virtual environment](#install-tool-venv)
+ [Using a bundled installer](#install-tool-bundled)

**Prerequisites**
+ Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+
+ Windows, Linux, macOS, or Unix

**Note**  
Earlier versions of Python might not work with all AWS services\. If you see `InsecurePlatformWarning` or deprecation notices when you install or use the AWS CLI, update to a newer version\.

You can find the version number of the most recent CLI at: [https://github.com/aws/aws-cli/blob/master/CHANGELOG.rst](https://github.com/aws/aws-cli/blob/master/CHANGELOG.rst)\.

In this guide, the commands shown assume you have Python v3 installed and the `pip` commands shown use the `pip3` version\.

## Installing the AWS CLI Using pip<a name="install-tool-pip"></a>

The primary distribution method for the AWS CLI on Linux, Windows, and macOS is `pip`\. This is a package manager for Python that provides an easy way to install, upgrade, and remove Python packages and their dependencies\.

**Installing the current AWS CLI Version**  
The AWS CLI is updated frequently with support for new services and commands\. To determine whether you have the latest version, see the [releases page on GitHub](https://github.com/aws/aws-cli/releases)\.

If you already have `pip` and a supported version of Python, you can install the AWS CLI by using the following command\. If you have Python version 3\+ installed, we recommend that you use the **pip3** command\.

```
$ pip3 install awscli --upgrade --user
```

The `--upgrade` option tells `pip3` to upgrade any requirements that are already installed\. The `--user` option tells `pip3` to install the program to a subdirectory of your user directory to avoid modifying libraries used by your operating system\.

**Upgrading to the latest version of the AWS CLI**

We recommend that you regularly check to see if there is a new version of the AWS CLI and upgrade to it when you can\.

Use the `pip3 list -o` command to check which packages are "outdated':

```
$ aws --version
aws-cli/1.16.170 Python/3.7.3 Linux/4.14.123-111.109.amzn2.x86_64 botocore/1.12.160

$ pip3 list -o
Package    Version  Latest   Type 
---------- -------- -------- -----
awscli     1.16.170 1.16.198 wheel
botocore   1.12.160 1.12.188 wheel
```

Because the previous command shows that there is a newer version of the AWS CLI available, you can run `pip3 install --upgrade` to get the latest version:

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

## Installing the AWS CLI in a Virtual Environment<a name="install-tool-venv"></a>

If you encounter issues when you attempt to install the AWS CLI with `pip3`, you can [install the AWS CLI in a virtual environment](install-virtualenv.md) to isolate the tool and its dependencies\. Or you can use a different version of Python than you normally do\.

## Installing the AWS CLI Using an Installer<a name="install-tool-bundled"></a>

For offline or automated installations on Linux, macOS, or Unix, try the [bundled installer](install-bundle.md)\. The bundled installer includes the AWS CLI, its dependencies, and a shell script that performs the installation for you\.

On Windows, you can also use the [MSI installer](install-windows.md#install-msi-on-windows)\. Both of these methods simplify the initial installation\. However, the tradeoff is that it's more difficult to upgrade when a new version of the AWS CLI is released\.

## Steps to Take after Installation<a name="install-post"></a>
+ [Setting the Path to Include the AWS CLI](#post-install-path)
+ [Configure the AWS CLI with Your Credentials](#post-install-configure)
+ [Upgrading to the Latest Version of the AWS CLI](#post-install-upgrade)
+ [Uninstalling the AWS CLI](#post-install-uninstall)

### Setting the Path to Include the AWS CLI<a name="post-install-path"></a>

After you install the AWS CLI, you might need to add the path to the executable file to your `PATH` variable\. For platform\-specific instructions, see the following topics:
+ **Linux** – [Add the AWS CLI Executable to Your Command Line Path](install-linux.md#install-linux-path)
+ **Windows** – [Add the AWS CLI Executable to Your Command Line Path](install-windows.md#awscli-install-windows-path)
+ **macOS** – [Add the AWS CLI Executable to Your macOS Command Line Path](install-macos.md#awscli-install-osx-path)

Verify that the AWS CLI installed correctly by running `aws --version`\.

```
$ aws --version
aws-cli/1.16.116 Python/3.6.8 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.106
```

### Configure the AWS CLI with Your Credentials<a name="post-install-configure"></a>

Before you can run a CLI command, you must configure the AWS CLI with your credentials\.

You store credential information locally by defining [profiles](cli-configure-profiles.md) in the [AWS CLI configuration files](cli-configure-files.md), which are stored by default in your user's home directory\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

**Note**  
If you are running in an Amazon EC2 instance, credentials can be automatically retrieved from the instance metadata\. For more information, see [Instance Metadata](cli-configure-metadata.md)\.

### Upgrading to the Latest Version of the AWS CLI<a name="post-install-upgrade"></a>

The AWS CLI is updated regularly to add support for new services and commands\. To update to the latest version of the AWS CLI, run the installation command again\. For details about the latest version of the AWS CLI, see the [ AWS CLI release notes](https://github.com/aws/aws-cli/blob/develop/CHANGELOG.rst)\.

```
$ pip3 install awscli --upgrade --user
```

### Uninstalling the AWS CLI<a name="post-install-uninstall"></a>

If you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ pip3 uninstall awscli
```

If you don't have Python and `pip`, use the procedure for your environment\.

## Detailed Instructions for Each Environment<a name="install-sections"></a>
+ [Install the AWS CLI on Linux](install-linux.md)
+ [Install the AWS CLI on Windows](install-windows.md)
+ [Install the AWS CLI on macOS](install-macos.md)
+ [Install the AWS CLI in a Virtual Environment](install-virtualenv.md)
+ [Install the AWS CLI Using the Bundled Installer \(Linux, macOS, or Unix\)](install-bundle.md)