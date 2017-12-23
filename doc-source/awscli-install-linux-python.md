# Installing Python on Linux<a name="awscli-install-linux-python"></a>

If your distribution did not come with Python, or came with an older version, install Python before installing pip and the AWS CLI\.

**To install Python 3 on Linux**

1. Check to see if Python is already installed:

   ```
   $ python --version
   ```
**Note**  
If your Linux distribution came with Python, you may need to install the Python developer package in order to get the headers and libraries required to compile extensions and install the AWS CLI\. Install the developer package \(typically named python\-dev or python\-devel\) using your package manager\.

1. If Python 2\.7 or later is not installed, install Python with your distribution's package manager\. The command and package name varies:

   + On Debian derivatives such as Ubuntu, use `APT`:

     ```
     $ sudo apt-get install python3
     ```

   + On Red Hat and derivatives, use `yum`:

     ```
     $ sudo yum install python
     ```

   + On SUSE and derivatives, use `zypper`:

     ```
     $ sudo zypper install python3
     ```

1. Open a command prompt or shell and run the following command to verify that Python installed correctly:

   ```
   $ python3 --version
   Python 3.6.2
   ```