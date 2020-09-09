# Working with Amazon SWF domains using the AWS CLI<a name="cli-services-swf-domains"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to manage your Amazon Simple Workflow Service \(Amazon SWF\) domains\.

**Topics**
+ [List your domains](#listing-your-domains)
+ [Get information about a domain](#getting-information-about-a-domain)
+ [Register a domain](#registering-a-domain)
+ [Deprecate a domain](#deprecating-a-domain)

## List your domains<a name="listing-your-domains"></a>

To list the Amazon SWF domains that you have registered for your AWS account, you can use [https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html](https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html)\. You must include `--registration-status` and specify either `REGISTERED` or `DEPRECATED`\.

Here's a minimal example\.

```
$ aws swf list-domains --registration-status REGISTERED
{
    "domainInfos": [
        {
            "status": "REGISTERED",
            "name": "ExampleDomain"
        },
        {
            "status": "REGISTERED",
            "name": "mytest"
        }
    ]
}
```

**Note**  
For an example of using `DEPRECATED`, see [Deprecate a domain](#deprecating-a-domain)\.

For more information, see [list\-domains](https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html) in the *AWS CLI Command Reference*\.

## Get information about a domain<a name="getting-information-about-a-domain"></a>

To get detailed information about a particular domain, use [https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html)\. There is one required parameter, `--name`, which takes the name of the domain you want information about, as shown in the following example\.

```
$ aws swf describe-domain --name ExampleDomain
{
    "domainInfo": {
        "status": "REGISTERED",
        "name": "ExampleDomain"
    },
    "configuration": {
        "workflowExecutionRetentionPeriodInDays": "1"
    }
}
```

For more information, see [describe\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html) in the *AWS CLI Command Reference*\.

## Register a domain<a name="registering-a-domain"></a>

To register new domains, use [https://docs.aws.amazon.com/cli/latest/reference/swf/register-domain.html](https://docs.aws.amazon.com/cli/latest/reference/swf/register-domain.html)\. 

There are two required parameters: `--name` and `--workflow-execution-retention-period-in-days`\. The `--name` parmeter takes the domain name to register\. The `--workflow-execution-retention-period-in-days` parameter takes an integer to specify the number of days to retain workflow execution data on this domain, up to a maximum period of 90 days \(for more information, see the [Amazon SWF FAQ](http://aws.amazon.com/swf/faqs/#retain_limit)\)\. 

If you specify zero \(0\) for this value, the retention period is automatically set at the maximum duration\. Otherwise, workflow execution data isn't retained after the specified number of days have passed\. The following example shows how to register a new domain\.

```
$ aws swf register-domain --name MyNeatNewDomain --workflow-execution-retention-period-in-days 0
```

The command doesn't return any output, but you can use [https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html](https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html) or [https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html) to see the new domain, as shown in the following example\.

```
$ aws swf describe-domain --name MyNeatNewDomain
{
    "domainInfo": {
        "status": "REGISTERED",
        "name": "MyNeatNewDomain"
    },
    "configuration": {
        "workflowExecutionRetentionPeriodInDays": "0"
    }
}
```

For more information, see [register\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/register-domain.html) in the *AWS CLI Command Reference*\.

## Deprecate a domain<a name="deprecating-a-domain"></a>

To deprecate a domain \(you can still see it, but cannot create new workflow executions or register types on it\), use [https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-domain.html](https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-domain.html)\. It has a sole required parameter, `--name`, which takes the name of the domain to deprecate\.

```
$ aws swf deprecate-domain --name MyNeatNewDomain
```

As with `register-domain`, no output is returned\. If you use `list-domains` to view the registered domains, however, you will see that the domain no longer appears among them\. You can also use `--registration-status DEPRECATED`\.

```
$ aws swf list-domains --registration-status DEPRECATED
{
    "domainInfos": [
        {
            "status": "DEPRECATED",
            "name": "MyNeatNewDomain"
        }
    ]
}
```

For more information, see [deprecate\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-domain.html) in the *AWS CLI Command Reference*\.