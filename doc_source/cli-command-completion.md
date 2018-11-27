# Command Completion<a name="cli-command-completion"></a>

On Unix\-like systems, the AWS CLI includes a command\-completion feature that enables you to use the TAB key to complete a partially typed command\. This feature is not automatically installed so you need to configure it manually\.

Configuring command completion requires two pieces of information: the name of the shell you are using and the location of the aws\_completer script\.

**Completion on Amazon Linux**  
 Command completion is configured by default on instances running Amazon Linux\.

**Topics**
+ [Identify Your Shell](#cli-command-completion-shell)
+ [Locate the AWS Completer](#cli-command-completion-completer)
+ [Enable Command Completion](#cli-command-completion-enable)
+ [Test Command Completion](#cli-command-completion-test)

## Identify Your Shell<a name="cli-command-completion-shell"></a>

If you are not sure which shell you are using, identify it with one of the following commands:

**echo $SHELL** – show the shell's installation directory\. This will usually match the in\-use shell, unless you launched a different shell after logging in\.

```
$ echo $SHELL
/bin/bash
```

**ps** – show the processes running for the current user\. The shell will be one of them\.

```
$ ps
  PID TTY          TIME CMD
 2148 pts/1    00:00:00 bash
 8756 pts/1    00:00:00 ps
```

## Locate the AWS Completer<a name="cli-command-completion-completer"></a>

 The location can vary depending on the installation method used\. 

 **Package Manager** – programs such as pip, yum, brew and apt\-get typically install the AWS completer \(or a symlink to it\) to a standard path location\. In this case, `which` will locate the completer for you\. 

```
$ which aws_completer
/usr/local/bin/aws_completer
```

 **Bundled Installer** – if you used the bundled installer per the instructions in the previous section, the AWS completer will be located in the bin subfolder of the installation directory\. 

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

## Enable Command Completion<a name="cli-command-completion-enable"></a>

Run a command to enable command completion\. The command that you use to enable completion depends on the shell that you are using\. You can add the command to your shell's RC file to run it each time you open a new shell\.
+ **bash** – use the built\-in command `complete`\.

  ```
  $ complete -C '/usr/local/bin/aws_completer' aws
  ```

  Add the command to `~/.bashrc` to run it each time you open a new shell\. Your `~/.bash_profile` should source `~/.bashrc` to ensure that the command is run in login shells as well\.
+  **tcsh** – complete for tcsh takes a word type and pattern to define the completion behavior\. 

  ```
  > complete aws 'p/*/`aws_completer`/'
  ```

  Add the command to `~/.tschrc` to run it each time you open a new shell\.
+  **zsh** – source `bin/aws_zsh_completer.sh`\. 

  ```
  % source /usr/local/bin/aws_zsh_completer.sh
  ```

  The AWS CLI uses bash compatibility auto completion \(`bashcompinit`\) for zsh support\. For further details, refer to the top of `aws_zsh_completer.sh`\.

  Add the command to `~/.zshrc` to run it each time you open a new shell\.

## Test Command Completion<a name="cli-command-completion-test"></a>

After enabling command completion, type in a partial command and press tab to see the available commands\.

```
$ aws sTAB
s3              ses             sqs             sts             swf
s3api           sns             storagegateway  support
```