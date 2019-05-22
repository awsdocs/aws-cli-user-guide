# Install the AWS CLI on Windows<a name="install-windows"></a>

You can install the AWS Command Line Interface \(AWS CLI\) on Windows by using a standalone installer or `pip`, which is a package manager for Python\. If you already have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\.

**Topics**
+ [Install the AWS CLI Using the MSI Installer](#install-msi-on-windows)
+ [Install the AWS CLI Using Python and `pip` on Windows](#awscli-install-windows-pip)
+ [Add the AWS CLI Executable to Your Command Line Path](#awscli-install-windows-path)

## Install the AWS CLI Using the MSI Installer<a name="install-msi-on-windows"></a>

The AWS CLI is supported on Microsoft Windows XP or later\. For Windows users, the MSI installation package offers a familiar and convenient way to install the AWS CLI without installing any other prerequisites\.

When updates are released, you must repeat the installation process to get the latest version of the AWS CLI\. To update frequently, consider [using pip](#awscli-install-windows-pip) for easier updates\.

**To install the AWS CLI using the MSI installer**

1. Download the appropriate MSI installer\.
   + [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)
   + [Download the AWS CLI MSI installer for Windows \(32\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI32PY3.msi)
   + [Download the AWS CLI setup file](https://s3.amazonaws.com/aws-cli/AWSCLISetup.exe) \(includes both the 32\-bit and 64\-bit MSI installers and will automatically install the correct version\)
**Note**  
The MSI installer for the AWS CLI doesn't work with Windows Server 2008 \(version 6\.0\.6002\)\. Use [pip](#awscli-install-windows-pip) to install with this version of Windows Server\.

1. Run the downloaded MSI installer or the setup file\.

1. Follow the onscreen instructions\.

By default, the CLI installs to `C:\Program Files\Amazon\AWSCLI` \(64\-bit version\) or `C:\Program Files (x86)\Amazon\AWSCLI` \(32\-bit version\)\. To confirm the installation, use the `aws --version` command at a command prompt \(open the **Start** menu and search for `cmd` to start a command prompt\)\.

```
C:\> aws --version
aws-cli/1.16.116 Python/3.6.8 Windows/10 botocore/1.12.106
```

Don't include the prompt symbol \(`C:\>`, shown above\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the CLI\. The rest of this guide uses the generic prompt symbol, `$` , except in cases where a command is Windows\-specific\.

If Windows is unable to find the program, you might need to close and reopen the command prompt to refresh the path, or [add the installation directory to your PATH](#awscli-install-windows-path) environment variable manually\.

### Updating an MSI Installation<a name="install-msi-update"></a>

The AWS CLI is updated regularly\. Check the [Releases](https://github.com/aws/aws-cli/releases) page on GitHub to see when the latest version was released\. To update to the latest version, download and run the MSI installer again, as described previously\.

### Uninstalling the AWS CLI<a name="install-msi-uninstall"></a>

To uninstall the AWS CLI, open the **Control Panel**, and then choose **Programs and Features**\. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\. Confirm that you want to uninstall the AWS CLI when you're prompted\.

You can also launch the **Programs and Features** program from the command line with the following command\.

```
C:\> appwiz.cpl
```

## Install the AWS CLI Using Python and `pip` on Windows<a name="awscli-install-windows-pip"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python and `pip` \(Windows\)**

1. Download the Python Windows x86\-64 installer from the [downloads page](https://www.python.org/downloads/windows/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its program folders to your user path\.

**To install the AWS CLI with `pip3` \(Windows\)**

If you use Python version 3\+, we recommend that you use the `pip3` command\.

1. Open the **Command Prompt** from the **Start** menu\.

1. Use the following commands to verify that Python and `pip` are both installed correctly\.

   ```
   C:\> python --version
   Python 3.7.1
   C:\> pip3 --version
   pip 18.1 from c:\program files\python37\lib\site-packages\pip (python 3.7)
   ```

1. Install the AWS CLI using `pip`\.

   ```
   C:\> pip3 install awscli
   ```

1. Verify that the AWS CLI is installed correctly\.

   ```
   C:\> aws --version
   aws-cli/1.16.116 Python/3.6.8 Windows/10 botocore/1.12.106
   ```

To upgrade to the latest version, run the installation command again\.

```
C:\> pip3 install --user --upgrade awscli
```

## Add the AWS CLI Executable to Your Command Line Path<a name="awscli-install-windows-path"></a>

After installing the AWS CLI with `pip`, add the `aws` program to your operating system's `PATH` environment variable\. With an MSI installation, this should happen automatically, but you might need to set it manually if the `aws` command doesn't run after you install it\.

If this command returns a response, then you should be ready to run the tool\. The where command, by default, shows where in the system PATH it found the specified program:

```
C:\> where aws
C:\Program Files\Amazon\AWSCLI\bin\aws.exe
```

You can find where the aws program is installed by running the following command\.

```
C:\> where c:\ aws
C:\Program Files\Python37\Scripts\aws
```

If instead, the `where` command returns the following error, then it is not in the system PATH and you can't run it by simply typing its name\.

```
C:\> where c:\ aws
INFO: Could not find files for the given pattern(s).
```

In that case, run the where command with the `/R path` parameter to tell it to search all folders, and look then you must add the path manually\. Use the command line or Windows Explorer to discover where it is installed on your computer\. 

```
C:\> where /R c:\ aws
c:\Program Files\Amazon\AWSCLI\bin\aws.exe
c:\Program Files\Amazon\AWSCLI\bincompat\aws.cmd
c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws
c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws.cmd
...
```

The paths that show up depend on which method you used to install the AWS CLI\. 

Typical paths include:
+ **Python 3 and `pip3`** – `C:\Program Files\Python37\Scripts\`
+ **Python 3 and `pip3` \-\-user option on earlier versions of Windows** – `%USERPROFILE%\AppData\Local\Programs\Python\Python37\Scripts`
+ **Python 3 and `pip3` \-\-user option on Windows 10** – `%USERPROFILE%\AppData\Roaming\Python\Python37\Scripts`
+ **MSI installer \(64\-bit\)** – `C:\Program Files\Amazon\AWSCLI\bin`
+ **MSI installer \(32\-bit\)** – `C:\Program Files (x86)\Amazon\AWSCLI\bin`

**Note**  
Folder names that include version numbers can vary\. The examples above reflect the use of Python version 3\.7\. Replace as needed with the version number you are using\.

**To modify your PATH variable \(Windows\)**

1. Press the Windows key and enter **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH**, and then choose **Edit**\.

1. Add the path to the **Variable value** field\. For example: ***C:\\new\\path***

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and reopen the command prompt window\.