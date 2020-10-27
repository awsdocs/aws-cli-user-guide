# Having the AWS CLI prompt you for parameters<a name="cli-usage-parameters-prompting"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

You can have the AWS CLI version 2 prompt you for parameters when you run a command\. On your command line, include `--cli-auto-prompt`\.

```
$ aws iam add-user-to-group --cli-auto-prompt
--user-name: maria
```

In the preceding example, the first parameter is mandatory, and requires you to provide a value to continue\. The AWS CLI repeats this for each mandatory parameter required by the command that you are running, one after the other\.

After you provide values for all of the mandatory parameters, the AWS CLI displays all of the optional parameters and enables you to move between them using the up and down arrow keys\. The currently selected row is denoted with a caret \(>\) in the left margin and have light text on a dark background\. The non\-selected rows have dark text on a light background\.

```
$ aws iam create-user --cli-auto-prompt
--user-name: maria
> --path [string]: The path for the user name.
  --permissions-boundary [string]: The ARN of the policy that is used to set the permissions boundary for the user.
  --tags [list]: A list of tags that you want to attach to the newly created user.
  [DONE] Parameter input finished
```

Use the up and down arrow keys to select an optional parameter to provide a value for, then press **ENTER**\. Enter the value for the optional parameter, then press **ENTER** again\. That parameter and value move to the top of the list, above the remaining selectable rows\. Repeat this for each optional parameter that you want to use\. You can skip any that you don't need\. 

When you're done, select the last line that begins with `[DONE]` and press **ENTER** to confirm your choices\.

Finally, you're given the option to run the command or to print the final version with all of the provided parameter values\. As before, the caret \(>\) and highlighted text marks the current selection\. You can use the arrow keys to move the current selection up or down, then press **ENTER** to select that option\.

```
$ aws iam create-user --cli-auto-prompt
--user-name: maria
--path: /
> Execute CLI command
  Print CLI command.
```

If you choose to print the command, you get output similar to the following example, where only the `--user-name` and `--path` parameters were selected and provided with values\.

```
$ aws iam create-user --cli-auto-prompt
--user-name: maria                                                                                                                                                                    
--path: /
aws iam create-user --user-name maria --path /
```

If you choose instead to run the command, it does not display the command but immediately runs it\.

```
$ aws iam create-user --cli-auto-prompt
--user-name: maria
--path: /
{
    "User": {
        "Path": "/",
        "UserName": "maria",
        "UserId": "AIDA1234567890EXAMPLE",
        "Arn": "arn:aws:iam::123456789012:user/maria",
        "CreateDate": "2020-01-09T18:29:25+00:00"
    }
}
```