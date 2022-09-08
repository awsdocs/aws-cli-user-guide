--------

--------

# Specifying parameter values for the AWS CLI<a name="cli-usage-parameters"></a>

Many parameters used in the AWS Command Line Interface \(AWS CLI\) are simple string or numeric values, such as the key\-pair name `my-key-pair` in the following example\. 

```
$ aws ec2 create-key-pair --key-name my-key-pair
```

You can surround strings that do not contain any space characters with quotation marks or not\. However, you must use quotation marks around strings that include one or more space characters\. For more information about using quotation marks around complex parameters, see [Using quotation marks with strings in the AWS CLI](cli-usage-parameters-quoting-strings.md)\.

**Topics**
+ [Common AWS CLI parameter types](cli-usage-parameters-types.md)
+ [Using quotation marks with strings in the AWS CLI](cli-usage-parameters-quoting-strings.md)
+ [Loading AWS CLI parameters from a file](cli-usage-parameters-file.md)
+ [AWS CLI skeletons and input files](cli-usage-skeleton.md)
+ [Using shorthand syntax with the AWS CLI](cli-usage-shorthand.md)