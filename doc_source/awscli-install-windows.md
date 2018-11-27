# Install the AWS Command Line Interface on Microsoft Windows<a name="awscli-install-windows"></a>

You can install the AWS CLI on Windows with a standalone installer or `pip`, a package manager for Python\. If you already have `pip`, follow the instructions in the main [installation topic](installing.md)\.

**Topics**
+ [MSI Installer](#install-msi-on-windows)
+ [Install Python, pip, and the AWS CLI on Windows](#awscli-install-windows-pip)
+ [Adding the AWS CLI Executable to your Command Line Path](#awscli-install-windows-path)

## MSI Installer<a name="install-msi-on-windows"></a>

The AWS CLI is supported on Microsoft Windows XP or later\. For Windows users, the MSI installation package offers a familiar and convenient way to install the AWS CLI without installing any other prerequisites\.

When updates are released, you must repeat the installation process to get the latest version of the AWS CLI\. If you prefer to update frequently, consider [using pip](#awscli-install-windows-pip) for easier updates\.

**To install the AWS CLI using the MSI installer**

1. Download the appropriate MSI installer\.
   + [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)
   + [Download the AWS CLI MSI installer for Windows \(32\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI32PY3.msi)
   + [Download the AWS CLI setup file](https://s3.amazonaws.com/aws-cli/AWSCLISetup.exe) \(includes both the 32\-bit and 64\-bit MSI installers and will automatically install the correct version\)
**Note**  
The MSI installer for the AWS CLI does not work with Windows Server 2008 \(version 6\.0\.6002\)\. Use [pip](#awscli-install-windows-pip) to install with this version of Windows\.

1. Run the downloaded MSI installer or the setup file\.

1. Follow the instructions that appear\.

The CLI installs to `C:\Program Files\Amazon\AWSCLI` \(64\-bit\) or `C:\Program Files (x86)\Amazon\AWSCLI` \(32\-bit\) by default\. To confirm the installation, use the `aws --version` command at a command prompt \(open the START menu and search for "cmd" if you're not sure where the command prompt is installed\)\.

```
> aws --version
aws-cli/1.11.84 Python/3.6.2 Windows/7 botocore/1.5.47
```

Don't include the prompt symbol \('>' above\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the CLI\. The rest of this guide uses the generic prompt symbol '$' except in cases where a command is Windows\-specific\.

If Windows is unable to find the executable, you may need to re\-open the command prompt or [add the installation directory to your PATH](#awscli-install-windows-path) environment variable manually\.

### Updating an MSI Installation<a name="install-msi-update"></a>

The AWS CLI is updated regularly\. Check out the [Releases](https://github.com/aws/aws-cli/releases) page on GitHub to see when the latest version was released\. To update to the latest version, download and run the MSI installer again as detailed above\.

### Uninstalling<a name="install-msi-uninstall"></a>

To uninstall the AWS CLI, open the Control Panel and select *Programs and Features*\. Select the entry named *AWS Command Line Interface* and click *Uninstall* to launch the uninstaller\. Confirm that you wish to uninstall the AWS CLI when prompted\.

You can also launch the *Programs and Features* menu from the command line with the following command:

```
> appwiz.cpl
```

## Install Python, pip, and the AWS CLI on Windows<a name="awscli-install-windows-pip"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python 3\.6 and pip \(Windows\)**

1. Download the Python 3\.6 Windows x86\-64 executable installer from the [downloads page](https://www.python.org/downloads/release/python-362/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3\.6 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its executable directories to your user path\.

**To install the AWS CLI with pip \(Windows\)**

1. Open the Windows Command Processor from the Start menu\.

1. Verify that Python and pip are both installed correctly with the following commands:

   ```
   C:\Windows\System32> python --version
   Python 3.6.2
   C:\Windows\System32> pip --version
   pip 9.0.1 from c:\users\myname\appdata\local\programs\python\python36\lib\site-packages (python 3.6)
   ```

1. Install the AWS CLI using `pip`:

   ```
   C:\Windows\System32> pip install awscli
   ```

1. Verify that the AWS CLI is installed correctly:

   ```
   C:\Windows\System32> aws --version
   aws-cli/1.11.84 Python/3.6.2 Windows/7 botocore/1.5.47
   ```

To upgrade to the latest version, run the installation command again:

```
C:\Windows\System32> pip install --user --upgrade awscli
```

## Adding the AWS CLI Executable to your Command Line Path<a name="awscli-install-windows-path"></a>

After installing with `pip`, add the `aws` executable to your OS's `PATH` environment variable\. With an MSI installation, this should happen automatically, but you may need to set it manually if the `aws` command is not working\.
+ **Python 3\.6 and pip** – `%USERPROFILE%\AppData\Local\Programs\Python\Python36\Scripts`
+ **MSI installer \(64\-bit\)** – `C:\Program Files\Amazon\AWSCLI`
+ **MSI installer \(32\-bit\)** – `C:\Program Files (x86)\Amazon\AWSCLI`

**To modify your PATH variable \(Windows\)**

1. Press the Windows key and type **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH** and then choose **Edit**\.

1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\existing\path;C:\new\path`

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and re\-open\.