# Command completion<a name="cli-configure-completion"></a>

On Unix\-like systems, the AWS Command Line Interface \(AWS CLI\) includes a command\-completion feature that enables you to use the **Tab** key to complete a partially entered command\. On most systems, this feature isn't automatically installed, so you need to configure it manually\.

**Topics**
+ [How it works](#cli-command-completion-about)
+ [Configuring command completion](#cli-command-completion-configure)

## How it works<a name="cli-command-completion-about"></a>

When you partially enter a command, parameter, or option, the command\-completion feature either automatically completes your command or displays a suggested list of commands\. To prompt command completion, you partially enter in a command and press **Tab**\.

The following examples show different ways that you can use command completion:
+ Partially enter a command and press **Tab** to display a suggested list of commands\.

  ```
  $ aws dynamodb dTAB
  datapipeline     deploy           dlm              dynamodb       
  datasync         devicefarm       dms              dynamodbstreams
  dax              directconnect    docdb                           
  ddb              discovery        ds
  ```
+ Partially enter a parameter and press **Tab** to display a suggested list of parameters\.

  ```
  $ aws dynamodb db delete-table --TAB
  --ca-bundle              --endpoint-url           --profile              
  --cli-connect-timeout    --generate-cli-skeleton  --query                
  --cli-input-json         --no-paginate            --region               
  --cli-read-timeout       --no-sign-request        --table-name           
  --color                  --no-verify-ssl          --version              
  --debug                  --output
  ```
+ Enter a parameter and press **Tab** to display a suggested list of resource values\. This feature is available only in the AWS CLI version 2\.

  ```
  $ aws dynamodb db delete-table --table-name TAB
  Table 1                  Table 2                  Table 3
  ```

## Configuring command completion<a name="cli-command-completion-configure"></a>

To configure command completion, you must have two pieces of information: the name of the shell you're using and the location of the `aws_completer` script\.

**Amazon Linux**  
Command completion is automatically configured and enabled by default on Amazon EC2 instances that run Amazon Linux\.

**Topics**
+ [Identify your shell](#cli-command-completion-shell)
+ [Locate the AWS completer](#cli-command-completion-completer)
+ [Add the completer's folder to your path](#cli-command-completion-path)
+ [Enable command completion](#cli-command-completion-enable)
+ [Test Command Completion](#cli-command-completion-test)

### Identify your shell<a name="cli-command-completion-shell"></a>

To identify which shell you're using, you can use one of the following commands\.

**echo $SHELL** – Displays the shell's program file name\. This usually matches the name of the in\-use shell, unless you launched a different shell after logging in\.

```
$ echo $SHELL
/bin/bash
```

**ps** – Displays the processes running for the current user\. One of them is the shell\.

```
$ ps
  PID TTY          TIME CMD
 2148 pts/1    00:00:00 bash
 8756 pts/1    00:00:00 ps
```

### Locate the AWS completer<a name="cli-command-completion-completer"></a>

 The location of the AWS completer can vary depending on the installation method used\. 

 **Package Manager** – Programs such as `pip`, `yum`, `brew`, and `apt-get` typically install the AWS completer \(or a symlink to it\) to a standard path location\. In this case, the `which` command can locate the completer for you\.

If you used `pip` without the `--user` command, you might see the following path\.

```
$ which aws_completer
/usr/local/aws/bin/aws_completer
```

If you used the `--user` parameter on the `pip` install command, you can typically find the completer in the `local/bin` folder under your `$HOME` folder\.

```
$ which aws_completer
/home/username/.local/bin/aws_completer
```

 **Bundled Installer** – If you used the bundled installer per the instructions in the previous section, the AWS completer is located in the `bin` subfolder of the installation directory\. 

```
$ ls /usr/local/aws/bin
activate
activate.csh
activate.fish
activate_this.py
aws
aws.cmd
aws_completer
...
```

If all else fails, you can use `find` to search your entire file system for the AWS completer\. 

```
$ find / -name aws_completer
/usr/local/aws/bin/aws_completer
```

### Add the completer's folder to your path<a name="cli-command-completion-path"></a>

For the AWS completer to work successfully, you must first add it to your computer's path\.

1. Find your shell's profile script in your user folder\. If you're not sure which shell you have, run echo $SHELL\.

   ```
   $ ls -a ~/
   .  ..  .bash_logout  .bash_profile  .bashrc  Desktop  Documents  Downloads
   ```
   + **Bash**– `.bash_profile`, `.profile`, or `.bash_login`
   + **Zsh**– `.zshrc`
   + **Tcsh**– `.tcshrc`, `.cshrc`, or `.login`

1. Add an export command at the end of your profile script that's similar to the following example\. Replace *`/usr/local/aws/bin`* with the folder that you discovered in the previous section\.

   ```
   export PATH=/usr/local/aws/bin:$PATH
   ```

1. Reload the profile into the current session to put those changes into effect\. Replace `.bash_profile` with the name of the shell script you discovered in the first section\.

   ```
   $ source ~/.bash_profile
   ```

### Enable command completion<a name="cli-command-completion-enable"></a>

To enable command completion, run the command for the shell that you're using\. You can add the command to your shell's RC file to run it each time you open a new shell\. In each command, replace the path `/usr/local/aws/bin` with the one found on your system in the previous section\.
+ **`bash`** – Use the built\-in command `complete`\.

  ```
  $ complete -C '/usr/local/aws/bin/aws_completer' aws
  ```

  Add the command to `~/.bashrc` to run it each time you open a new shell\. Your `~/.bash_profile` should source `~/.bashrc` to ensure that the command is also run in login shells\.
+  **`zsh`** – To run command completion, you need to run `bashcompinit` by adding the following autoload line at the end of your `~/.zshrc` profile script\.

  ```
  $ autoload bashcompinit && bashcompinit
  ```

  To enable command completion, use the built\-in command `complete`\.

  ```
  $ complete -C '/usr/local/aws/bin/aws_completer' aws
  ```

  Add the command to `~/.zshrc` to run it each time you open a new shell\.
+  **`tcsh`** – Complete for `tcsh` takes a word type and pattern to define the completion behavior\. 

  ```
  > complete aws 'p/*/`aws_completer`/'
  ```

  Add the command to `~/.tschrc` to run it each time you open a new shell\.

### Test Command Completion<a name="cli-command-completion-test"></a>

After enabling command completion, enter a partial command and press **Tab** to see the available commands\.

```
$ aws sTAB
s3              ses             sqs             sts             swf
s3api           sns             storagegateway  support
```