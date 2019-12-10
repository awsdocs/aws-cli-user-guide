# Install the AWS CLI version 1 on Windows<a name="install-windows"></a>

You can install version 1 of the AWS Command Line Interface \(AWS CLI\) on Windows by using a standalone installer or `pip`, which is a package manager for Python\. If you already have `pip`, follow the instructions in the main [installation topic](cli-chap-install.md)\.

**Important**  
On January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI version 1 with Python 2\.6 or Python 3\.3](deprecate-python-26-33.md) in this guide, and the [deprecation announcement in this blog post](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/)\.

**Topics**
+ [Install the AWS CLI version 1 Using the MSI Installer](#install-msi-on-windows)
+ [Install the AWS CLI version 1 Using Python and `pip` on Windows](#awscli-install-windows-pip)
+ [Add the AWS CLI version 1 Executable to Your Command Line Path](#awscli-install-windows-path)

## Install the AWS CLI version 1 Using the MSI Installer<a name="install-msi-on-windows"></a>

The AWS CLI version 1 is supported on Windows XP or later\. For Windows users, the MSI installation package offers a familiar and convenient way to install the AWS CLI version 1 without installing any other prerequisites\.

When updates are released, you must repeat the installation process to get the latest version of the AWS CLI version 1\. 

**To install the AWS CLI version 1 using the MSI installer**

1. Download the appropriate MSI installer\.
   + [Download the AWS CLI MSI installer for Windows \(64\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)
   + [Download the AWS CLI MSI installer for Windows \(32\-bit\)](https://s3.amazonaws.com/aws-cli/AWSCLI32PY3.msi)
   + [Download the AWS CLI setup file](https://s3.amazonaws.com/aws-cli/AWSCLISetup.exe) \(includes both the 32\-bit and 64\-bit MSI installers and will automatically install the correct version\)
**Note**  
The MSI installer for the AWS CLI version 1 doesn't work with Windows Server 2008 \(version 6\.0\.6002\)\. Use [pip](#awscli-install-windows-pip) to install with this version of Windows Server\.

1. Run the downloaded MSI installer or the setup file\.

1. Follow the onscreen instructions\.

By default, the AWS CLI version 1 installs to `C:\Program Files\Amazon\AWSCLI` \(64\-bit version\) or `C:\Program Files (x86)\Amazon\AWSCLI` \(32\-bit version\)\. To confirm the installation, use the `aws --version` command at a command prompt \(open the **Start** menu and search for `cmd` to start a command prompt\)\.

```
C:\> aws --version
aws-cli/1.16.273 Python/3.7.3 Windows/10 botocore/1.13.0
```

Don't include the prompt symbol \(`C:\>`, shown above\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the CLI\. The rest of this guide uses the generic prompt symbol, `$` , except in cases where a command is Windows\-specific\.

If Windows is unable to find the program, you might need to close and reopen the command prompt to refresh the path, or [add the installation directory to your PATH](#awscli-install-windows-path) environment variable manually\.

### Updating an MSI Installation<a name="install-msi-update"></a>

The AWS CLI version 1 is updated regularly\. Check the [Releases](https://github.com/aws/aws-cli/releases) page on GitHub to see when the latest version was released\. To update to the latest version, download and run the MSI installer again, as described previously\.

### Uninstalling the AWS CLI version 1<a name="install-msi-uninstall"></a>

To uninstall the AWS CLI version 1, open the **Control Panel**, and then choose **Programs and Features**\. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\. Confirm that you want to uninstall the AWS CLI when you're prompted\.

You can also launch the **Programs and Features** program from the command line with the following command\.

```
C:\> appwiz.cpl
```

## Install the AWS CLI version 1 Using Python and `pip` on Windows<a name="awscli-install-windows-pip"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

**To install Python and `pip` \(Windows\)**

1. Download the Python Windows x86\-64 installer from the [downloads page](https://www.python.org/downloads/windows/) of [Python\.org](https://www.python.org)\.

1. Run the installer\.

1. Choose **Add Python 3 to PATH**\.

1. Choose **Install Now**\.

The installer installs Python in your user folder and adds its program folders to your user path\.

**To install the AWS CLI version 1 with `pip3` \(Windows\)**

If you use Python version 3\+, we recommend that you use the `pip3` command\.

1. Open the **Command Prompt** from the **Start** menu\.

1. Use the following commands to verify that Python and `pip` are both installed correctly\.

   ```
   C:\> python --version
   Python 3.7.1
   C:\> pip3 --version
   pip 19.2.3 from c:\program files\python37\lib\site-packages\pip (python 3.7)
   ```

1. Install the AWS CLI version 1 using `pip`\.

   ```
   C:\> pip3 install awscli
   ```

1. Verify that the AWS CLI version 1 is installed correctly\.

   ```
   C:\> aws --version
   aws-cli/1.16.273 Python/3.7.3 Windows/10 botocore/1.13.0
   ```

To upgrade to the latest version, run the installation command again\.

```
C:\> pip3 install --user --upgrade awscli
```

## Add the AWS CLI version 1 Executable to Your Command Line Path<a name="awscli-install-windows-path"></a>

After installing the AWS CLI version 1 with `pip`, add the `aws` program to your operating system's `PATH` environment variable\. With an MSI installation, this should happen automatically, but you might need to set it manually if the `aws` command doesn't run after you install it\.

If this command returns a response, then you should be ready to run the tool\. The `where` command, by default, shows where in the system `PATH` it found the specified program\.

```
C:\> where aws
C:\Program Files\Amazon\AWSCLI\bin\aws.exe
```

You can find where the `aws` program is installed by running the following command\.

```
C:\> where c:\ aws
C:\Program Files\Python37\Scripts\aws
```

If the `where` command returns the following error, it's not in the system `PATH` and you can't run it by simply typing its name\.

```
C:\> where c:\ aws
INFO: Could not find files for the given pattern(s).
```

In that case, run the `where` command with the `/R path` parameter to tell it to search all folders, and then add the path manually\. Use the command line or File Explorer to discover where it is installed on your computer\. 

```
C:\> where /R c:\ aws
c:\Program Files\Amazon\AWSCLI\bin\aws.exe
c:\Program Files\Amazon\AWSCLI\bincompat\aws.cmd
c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws
c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws.cmd
...
```

The paths that show up depend on your platform and which method you used to install the AWS CLI\. 

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