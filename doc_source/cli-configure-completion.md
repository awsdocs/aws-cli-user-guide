# Command Completion<a name="cli-configure-completion"></a>

On Unix\-like systems, the AWS CLI includes a command\-completion feature that enables you to use the **Tab** key to complete a partially typed command\. On most systems, this feature isn't automatically installed, so you need to configure it manually\.

To configure command completion, you must have two pieces of information: the name of the shell you're using and the location of the `aws_completer` script\.

**Amazon Linux**  
Command completion is automatically configured and enabled by default on Amazon EC2 instances that run Amazon Linux\.

**Topics**
+ [Identify Your Shell](#cli-command-completion-shell)
+ [Locate the AWS Completer](#cli-command-completion-completer)
+ [Add the Completer's Folder to Your Path](#cli-command-completion-path)
+ [Enable Command Completion](#cli-command-completion-enable)
+ [Test Command Completion](#cli-command-completion-test)

## Identify Your Shell<a name="cli-command-completion-shell"></a>

If you're not sure which shell you're using, you can use one of the following commands to identify it\.

**echo $SHELL** – Show the shell's program file name\. This usually matches the name of the in\-use shell, unless you launched a different shell after logging in\.

```
$ echo $SHELL
/bin/bash
```

**ps** – Show the processes running for the current user\. One of them is the shell\.

```
$ ps
  PID TTY          TIME CMD
 2148 pts/1    00:00:00 bash
 8756 pts/1    00:00:00 ps
```

## Locate the AWS Completer<a name="cli-command-completion-completer"></a>

**AWS CLI version 2**

For AWS CLI version 2, substitute `aws2_completer` instead of `aws_completer` in the commands below.

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

## Add the Completer's Folder to Your Path<a name="cli-command-completion-path"></a>

For the AWS completer to work successfully, you must first add it to your computer's path\.

1. Find your shell's profile script in your user folder\. If you're not sure which shell you have, run echo $SHELL\.

   ```
   $ ls -a ~
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

## Enable Command Completion<a name="cli-command-completion-enable"></a>

Run a command to enable command completion\. The command that you use depends on the shell that you're using\. You can add the command to your shell's RC file to run it each time you open a new shell\. In each command, replace the path `/usr/local/aws/bin` with the one found on your system in the previous section\.
+ **`bash`** – Use the built\-in command `complete`\.

  ```
  $ complete -C '/usr/local/aws/bin/aws_completer' aws
  ```

  Add the command to `~/.bashrc` to run it each time you open a new shell\. Your `~/.bash_profile` should source `~/.bashrc` to ensure that the command is also run in login shells\.
+  **`tcsh`** – Complete for `tcsh` takes a word type and pattern to define the completion behavior\. 

  ```
  > complete aws 'p/*/`aws_completer`/'
  ```

  Add the command to `~/.tschrc` to run it each time you open a new shell\.
+  **`zsh`** – source `bin/aws_zsh_completer.sh`\. 

  ```
  % source /usr/local/aws/bin/aws_zsh_completer.sh
  ```

  The AWS CLI uses `bash` compatibility autocompletion \(`bashcompinit`\) for `zsh` support\. For more details, see the top of `aws_zsh_completer.sh`\.

  Add the command to `~/.zshrc` to run it each time you open a new shell\.

## Test Command Completion<a name="cli-command-completion-test"></a>

After enabling command completion, enter a partial command and press **Tab** to see the available commands\.

```
$ aws sTAB
s3              ses             sqs             sts             swf
s3api           sns             storagegateway  support
```