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

**Current AWS CLI Version**  
The AWS CLI is updated frequently with support for new services and commands\. To determine whether you have the latest version, see the [releases page on GitHub](https://github.com/aws/aws-cli/releases)\.

If you already have `pip` and a supported version of Python, you can install the AWS CLI by using the following command\. If you have Python version 3\+ installed, we recommend that you use the **pip3** command\.

```
$ pip3 install awscli --upgrade --user
```

The `--upgrade` option tells `pip3` to upgrade any requirements that are already installed\. The `--user` option tells `pip3` to install the program to a subdirectory of your user directory to avoid modifying libraries used by your operating system\.

## Installing the AWS CLI in a Virtual Environment<a name="install-tool-venv"></a>

If you encounter issues when you attempt to install the AWS CLI with `pip3`, you can [install the AWS CLI in a virtual environment](install-virtualenv.md) to isolate the tool and its dependencies\. Or you can use a different version of Python than you normally do\.

## Installing the AWS CLI Using an Installer<a name="install-tool-bundled"></a>

For offline or automated installations on Linux, macOS, or Unix, try the [bundled installer](install-bundle.md)\. The bundled installer includes the AWS CLI, its dependencies, and a shell script that performs the installation for you\.

On Windows, you can also use the [MSI installer](install-windows.md#install-msi-on-windows)\. Both of these methods simplify the initial installation\. However, the tradeoff is that it's more difficult to upgrade when a new version of the AWS CLI is released\.

## Steps to Take after Installation<a name="install-post"></a>

After you install the AWS CLI, you might need to add the path to the executable file to your `PATH` variable\. For platform\-specific instructions, see the following topics:
+ **Linux** – [Add the AWS CLI Executable to Your Command Line Path](install-linux.md#install-linux-path)
+ **Windows** – [Add the AWS CLI Executable to Your Command Line Path](install-windows.md#awscli-install-windows-path)
+ **macOS** – [Add the AWS CLI Executable to Your Command Line Path](install-macos.md#awscli-install-osx-path)

Verify that the AWS CLI installed correctly by running `aws --version`\.

```
$ aws --version
aws-cli/1.16.71 Python/3.6.5 Linux/4.14.77-81.59-amzn2.x86_64 botocore/1.12.61
```

The AWS CLI is updated regularly to add support for new services and commands\. To update to the latest version of the AWS CLI, run the installation command again\. For details about the latest version of the AWS CLI, see the [ AWS CLI release notes](https://github.com/aws/aws-cli/blob/develop/CHANGELOG.rst)\.

```
$ pip3 install awscli --upgrade --user
```

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