# Install, Update, and Uninstall the AWS CLI version 1 on Windows<a name="install-windows"></a>

You can install version 1 of the AWS Command Line Interface \(AWS CLI\) on Windows by using a standalone installer \(recommended\) or `pip`, which is a package manager for Python\. If you already have `pip`\. 

Don't include the prompt symbol \(`C:\>`\) when you type a command\. These are included in program listings to differentiate commands that you type from output returned by the CLI\. The rest of this guide uses the generic prompt symbol \(`$`\), except in cases where a command is Windows\-specific\.

**Important**  
AWS CLI version 1 no longer supports Python versions 2\.6 and 3\.3\. All versions of the AWS CLI version 1 released after January 10th, 2020, starting with 1\.17, require Python 2\.7, Python 3\.4, or a later version\.  
This change does not affect the Windows MSI installer version of the AWS CLI version 1 and the AWS CLI version 2\.  
For more information, see [Using the AWS CLI version 1 with earlier versions of Python](deprecate-old-python-versions.md) in this guide, and the [deprecation announcement](https://aws.amazon.com/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/) blog post\.

**Topics**
+ [Install, update, and uninstall the AWS CLI version 1 using the MSI installer](#msi-on-windows)
+ [Install, update, and uninstall the AWS CLI version 1 using Python and pip on Windows](#awscli-install-windows-pip)
+ [Add the AWS CLI version 1 executable to your command line path](#awscli-install-windows-path)

## Install, update, and uninstall the AWS CLI version 1 using the MSI installer<a name="msi-on-windows"></a>

The AWS CLI version 1 is supported on Windows XP or later\. For Windows users, the MSI installation package offers a familiar and convenient way to install the AWS CLI version 1 without installing any other prerequisites\. 

### Install and update the AWS CLI version 1 using the MSI installer<a name="install-msi-on-windows"></a>

Check the [Releases](https://github.com/aws/aws-cli/releases) page on GitHub to see when the latest version was released\. When updates are released, you must repeat the installation process to get the latest version of the AWS CLI version 1\. 

1. Download the appropriate MSI installer:
   + AWS CLI MSI installer for Windows \(64\-bit\): [https://s3\.amazonaws\.com/aws\-cli/AWSCLI64PY3\.msi](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)
   + AWS CLI MSI installer for Windows \(32\-bit\): [https://s3\.amazonaws\.com/aws\-cli/AWSCLI32PY3\.msi](https://s3.amazonaws.com/aws-cli/AWSCLI32PY3.msi)
   + AWS CLI combined setup file for Windows: [ https://s3\.amazonaws\.com/aws\-cli/AWSCLISetup\.exe](https://s3.amazonaws.com/aws-cli/AWSCLISetup.exe) \(includes both the 32\-bit and 64\-bit MSI installers, and automatically installs the correct version\)

1. Run the downloaded MSI installer or the setup file\.

1. Follow the on\-screen instructions\. By default, the AWS CLI version 1 installs to `C:\Program Files\Amazon\AWSCLI` \(64\-bit version\) or `C:\Program Files (x86)\Amazon\AWSCLI` \(32\-bit version\)\. 

1. To confirm the installation, use the `aws --version` command at a command prompt \(open the **Start** menu and search for `cmd` to start a command prompt\)\.

   ```
   C:\> aws --version
   aws-cli/1.18.134 Python/3.7.4 Windows/10 botocore/1.13
   ```

   If Windows is unable to find the program, you might need to close and reopen the command prompt to refresh the path, or [add the installation directory to your PATH](#awscli-install-windows-path) environment variable manually\.

### Uninstall the AWS CLI version 1<a name="install-msi-uninstall"></a>

To use the following uninstall instructions, you need to have installed the AWS CLI version 1 with the MSI installer or setup file\.

1. Open **Programs and Features** by doing one of the following:
   + Open the **Control Panel**, and then choose **Programs and Features**\.
   + Open a command prompt and enter the following command\.

     ```
     C:\> appwiz.cpl
     ```

1. Select the entry named **AWS Command Line Interface**, and then choose **Uninstall** to launch the uninstaller\.

1. Confirm that you want to uninstall the AWS CLI\.

## Install, update, and uninstall the AWS CLI version 1 using Python and pip on Windows<a name="awscli-install-windows-pip"></a>

The Python Software Foundation provides installers for Windows that include `pip`\.

### Install Python<a name="awscli-install-windows-pip-python"></a>

1. Download the Python Windows installer from the [downloads page](https://www.python.org/downloads/windows/) of [Python\.org](https://www.python.org)\.

1. Run the Python installer\.

1. Choose **Add Python 3 to PATH**\.

1. Choose **Install Now**\. The installer installs Python in your user folder and adds its program folders to your user path\.

1. On the **Start** menu, choose **Command Prompt**\.

1. To verify that Python and `pip` are both installed correctly, use the following commands and confirm there is output\.

   ```
   C:\> python --version
   Python 3.7.1
   C:\> pip3 --version
   pip 19.2.3 from c:\program files\python37\lib\site-packages\pip (python 3.7)
   ```

### Install and update the AWS CLI version 1 using pip<a name="awscli-install-windows-pip-python"></a>

1. To Install the AWS CLI version 1, use the `pip3` command \(if you use Python version 3 or later\) or the `pip` command\.

   ```
   C:\> pip3 install awscli
   ```

   To upgrade to the latest version, run the installation command with the `--user` and `--upgrade` paramaters\.

   ```
   C:\> pip3 install --user --upgrade awscli
   ```

1. Verify that the AWS CLI version 1 is installed correctly\. If there is no response, see the [Add the AWS CLI version 1 executable to your command line path](#awscli-install-windows-path) section\.

   ```
   C:\> aws --version
   aws-cli/1.18.134 Python/3.7.4 Windows/10 botocore/1.13
   ```

### Uninstall the AWS CLI version 1 using pip<a name="awscli-install-windows-pip-uninstall"></a>

If you installed the AWS CLI version 1 using `pip`, you must also uninstall using `pip`\. If you use Python version 3 or later, we recommend that you use the `pip3` command\.

```
C:\> pip3 uninstall awscli
```

You might need to restart your command prompt window or your computer to remove all files\.

## Add the AWS CLI version 1 executable to your command line path<a name="awscli-install-windows-path"></a>

After installing the AWS CLI version 1 with `pip`, add the `aws` program to your operating system's `PATH` environment variable\. With an MSI installation, this should happen automatically\. But if the `aws` command doesn't run after you install it, you might need to set it manually\.

1. Use the `where` command to find the `aws` file location\. By default, the `where` command shows where a specified program is found in the system's `PATH`\. 

   ```
   C:\> where aws
   ```

   The paths that show up depend on your platform and which method you used to install the AWS CLI\. Folder names that include version numbers can vary\. These examples reflect the use of Python version 3\.7\. Replace the version with the version number you're using, as needed\. Typical paths include the following:
   + **Python 3 and `pip3`** – `C:\Program Files\Python37\Scripts\`
   + **Python 3 and `pip3` \-\-user option on earlier versions of Windows** – `%USERPROFILE%\AppData\Local\Programs\Python\Python37\Scripts`
   + **Python 3 and `pip3` \-\-user option on Windows 10** – `%USERPROFILE%\AppData\Roaming\Python\Python37\Scripts`
   + **MSI installer \(64\-bit\)** – `C:\Program Files\Amazon\AWSCLI\bin`
   + **MSI installer \(32\-bit\)** – `C:\Program Files (x86)\Amazon\AWSCLI\bin`

   Use the following steps based on whether a file path is returned\.

------
#### [ A file path is returned ]

   ```
   C:\> where aws
   C:\Program Files\Amazon\AWSCLI\bin\aws.exe
   ```

   You can find where the `aws` program is installed by running the following command\.

   ```
   C:\> where c:\ aws
   C:\Program Files\Python37\Scripts\aws
   ```

------
#### [ A file path is NOT returned ]

   If the `where` command returns the following error, it's not in the system `PATH` and you can't run it by entering its name\.

   ```
   C:\> where c:\ aws
   INFO: Could not find files for the given pattern(s).
   ```

   In that case, run the `where` command with the `/R path` parameter to tell it to search all folders, and then add the path manually\. Use the command line or File Explorer to discover where it's installed on your computer\. 

   ```
   C:\> where /R c:\ aws
   c:\Program Files\Amazon\AWSCLI\bin\aws.exe
   c:\Program Files\Amazon\AWSCLI\bincompat\aws.cmd
   c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws
   c:\Program Files\Amazon\AWSCLI\runtime\Scripts\aws.cmd
   ...
   ```

------

1. Press the Windows key and enter **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH**, and then choose **Edit**\.

1. Add the path you found into the **Variable value** field, for example, ***C:\\Program Files\\Amazon\\AWSCLI\\bin\\aws\.exe***\.

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and reopen the command prompt window\.