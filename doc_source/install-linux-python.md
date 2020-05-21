# Installing Python on Linux<a name="install-linux-python"></a>

If your distribution didn't come with Python, or came with an earlier version, install Python 2 version 2\.7 or later, or Python 3 version 3\.4 or later before installing `pip` and the AWS CLI\.

**To install Python 3 on Linux**

1. See if Python is already installed\.

   ```
   $ python --version
   ```

   Or you can use the following\.

   ```
   $ python3 --version
   ```
**Note**  
If your Linux distribution came with Python, you might need to install the Python developer package to get the headers and libraries required to compile extensions, and install the AWS CLI\. Use your package manager to install the developer package \(typically named `python-dev` or `python-devel`\)\.

1. If Python 2 version 2\.7 or later, or Python 3 version 3\.4 or later isn't installed, install Python with your distribution's package manager\. The command and package name vary:
   + On Debian derivatives such as Ubuntu, use `apt`\. Check the `apt` repository for the versions of Python available to you\. Then, run a command similar to the following, substituting the correct package name\.

     ```
     $ sudo apt-get install python3
     ```
   + On Red Hat and derivatives, use `yum`\. Check the `yum` repository for the versions of Python available to you\. Then, run a command similar to the following, substituting the correct package name\.

     ```
     $ sudo yum install python3
     ```
   + On SUSE and derivatives, use `zypper`\. Check the `zypper` repository for the versions of Python available to you\. Then, run a command similar to the following, substituting the correct package name\.

     ```
     $ sudo zypper install python3
     ```

   See the documentation for your system's package manager and for [Python](https://www.python.org/doc/) for more information about where it is installed and how to use it\.

1. Open a command prompt window or shell and run the following command to verify that Python installed correctly\.

   ```
   $ python3 --version
   Python 3.7.4
   ```