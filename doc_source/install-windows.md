# Install the AWS Command Line Interface on Microsoft Windows<a name="install-windows"></a>

You can install the AWS CLI on Windows with a standalone installer or `pip`, a package manager for Python\. If you already have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\.

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

1. Follow the on\-screen instructions\.

The CLI installs to `C:\Program Files\Amazon\AWSCLI` \(64\-bit version\) or `C:\Program Files (x86)\Amazon\AWSCLI` \(32\-bit version\) by default\. To confirm the installation, use the `aws --version` command at a command prompt \(you can open the **Start** menu and search for `cmd` to start a command prompt\)\.

```
C:\> aws --version
aws-cli/1.16.67 Python/3.7.1 Windows/10 botocore/1.12.57
```

Don't include the prompt symbol \('C:\\>' above\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the CLI\. The rest of this guide uses the generic prompt symbol '$ ' except in cases where a command is Windows\-specific\.

If Windows is unable to find the program, you might need to close and re\-open the command prompt to refresh the path or [add the installation directory to your PATH](#awscli-install-windows-path) environment variable manually\.

### Updating an MSI Installation<a name="install-msi-update"></a>

The AWS CLI is updated regularly\. Check the [Releases](https://github.com/aws/aws-cli/releases) page on GitHub to see when the latest version was released\. To update to the latest version, download and run the MSI installer again as detailed above\.

### Uninstalling<a name="install-msi-uninstall"></a>

To uninstall the AWS CLI, open the **Control Panel** and select **Programs and Features**\. Select the entry named **AWS Command Line Interface** and click **Uninstall** to launch the uninstaller\. Confirm that you wish to uninstall the AWS CLI when prompted\.

You can also launch the **Programs and Features** program from the command line with the following command:

```
C:\> appwiz.cpl
```

## Install Python, pip, and the AWS CLI on Windows<a name="awscli-install-windows-pip"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python 3 and pip \(Windows\)**

1. Download the Python 3 Windows x86\-64 installer from the [downloads page](https://www.python.org/downloads/windows/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its program folders to your user path\.

**To install the AWS CLI with pip \(Windows\)**

1. Open the **Windows Command Prompt** from the **Start** menu\.

1. Verify that Python and pip are both installed correctly with the following commands:

   ```
   C:\Windows\System32> python --version
   Python 3.7.1
   C:\Windows\System32> pip --version
   pip 18.1 from c:\program files\python37\lib\site-packages\pip (python 3.7)
   ```

1. Install the AWS CLI using `pip`:

   ```
   C:\Windows\System32> pip install awscli
   ```

1. Verify that the AWS CLI is installed correctly:

   ```
   C:\Windows\System32> aws --version
   aws-cli/1.16.67 Python/3.7.1 Windows/10 botocore/1.12.57
   ```

To upgrade to the latest version, run the installation command again:

```
C:\Windows\System32> pip install --user --upgrade awscli
```

## Adding the AWS CLI Executable to your Command Line Path<a name="awscli-install-windows-path"></a>

After installing with `pip`, add the `aws` program to your OS's `PATH` environment variable\. With an MSI installation, this should happen automatically, but you might need to set it manually if the `aws` command isn't working\.
+ **Python 3 and pip** – `C:\Program Files\Python37\Scripts\`
+ **Python 3 and pip \-\-user option** – `%USERPROFILE%\AppData\Local\Programs\Python\Python37\Scripts`
+ **MSI installer \(64\-bit\)** – `C:\Program Files\Amazon\AWSCLI`
+ **MSI installer \(32\-bit\)** – `C:\Program Files (x86)\Amazon\AWSCLI`

**To modify your PATH variable \(Windows\)**

1. Press the Windows key and type **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH** and then choose **Edit**\.

1. Add paths to the **Variable value** field, separated by semicolons\. For example: `C:\existing\path;C:\new\path`

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and re\-open\.