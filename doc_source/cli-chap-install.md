# Installing the AWS Command Line Interface<a name="cli-chap-install"></a>

**Ways to install the AWS CLI**
+ [`pip`](#install-tool-pip)
+ [Using a virtual environment](#install-tool-venv)
+ [Using a bundled installer](#install-tool-bundled)

**Requirements**
+ Python 2 version 2\.6\.5\+ or Python 3 version 3\.3\+
+ Windows, Linux, macOS, or Unix

**Note**  
Older versions of Python may not work with all AWS services\. If you see `InsecurePlatformWarning` or deprecation notices when you install or use the &CLI;, update to a recent version\.

## Installing the AWS CLI Using *`pip`*<a name="install-tool-pip"></a>

The primary distribution method for the AWS CLI on Linux, Windows, and macOS is `pip`, a package manager for Python that provides an easy way to install, upgrade, and remove Python packages and their dependencies\.

**Current AWS CLI Version**  
The AWS CLI is updated frequently with support for new services and commands\. To see if you have the latest version, see the [releases page on GitHub](https://github.com/aws/aws-cli/releases)\.

If you already have `pip` and a supported version of Python, you can install the AWS CLI with the following command:

```
$ pip install awscli --upgrade --user
```

The `--upgrade` option tells `pip` to upgrade any requirements that are already installed\. The `--user` option tells `pip` to install the program to a subdirectory of your user directory to avoid modifying libraries used by your operating system\.

## Installing the AWS CLI in a Virtual Environment<a name="install-tool-venv"></a>

If you encounter issues when you attempt to install the AWS CLI with `pip`, you can [install the AWS CLI in a virtual environment](install-virtualenv.md) to isolate the tool and its dependencies, or use a different version of Python than you normally do\.

## Installing the AWS CLI Using an Installer<a name="install-tool-bundled"></a>

For offline or automated installations on Linux, macOS, or Unix, try the [bundled installer](install-bundle.md)\. The bundled installer includes the AWS CLI, its dependencies, and a shell script that performs the installation for you\.

On Windows, you can also use the [MSI installer](install-windows.md#install-msi-on-windows)\. Both of these methods simplify the initial installation, with the tradeoff of being more difficult to upgrade when a new version of the AWS CLI is released\.

## Steps to Take After Installation<a name="install-post"></a>

After you install the AWS CLI, you may need to add the path to the executable file to your PATH variable\. For platform specific instructions, see the following topics:
+ **Linux** – [Adding the AWS CLI Executable to your Command Line Path](install-linux.md#install-linux-path)
+ **Windows** – [Adding the AWS CLI Executable to your Command Line Path](install-windows.md#awscli-install-windows-path)
+ **macOS** – [Adding the AWS CLI Executable to your Command Line Path](install-macos.md#awscli-install-osx-path)

Verify that the AWS CLI installed correctly by running `aws --version`\.

```
$ aws --version
aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.61
```

The AWS CLI is updated regularly to add support for new services and commands\. To update to the latest version of the AWS CLI, run the installation command again\.

```
$ pip install awscli --upgrade --user
```

If you need to uninstall the AWS CLI, use `pip uninstall`\.

```
$ pip uninstall awscli
```

If you don't have Python and `pip`, use the procedure for your operating system:

## Detailed Instructions for Each Environment<a name="install-sections"></a>
+ [Install the AWS Command Line Interface on Linux](install-linux.md)
+ [Install the AWS Command Line Interface on Microsoft Windows](install-windows.md)
+ [Install the AWS Command Line Interface on macOS](install-macos.md)
+ [Install the AWS Command Line Interface in a Virtual Environment](install-virtualenv.md)
+ [Install the AWS CLI Using the Bundled Installer \(Linux, macOS, or Unix\)](install-bundle.md)