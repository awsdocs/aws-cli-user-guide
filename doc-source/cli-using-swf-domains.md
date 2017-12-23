# Working with Amazon SWF Domains Using the AWS Command Line Interface<a name="cli-using-swf-domains"></a>

This section describes how to perform common Amazon SWF domain tasks using the AWS CLI\.


+ [Listing Your Domains](#listing-your-domains)
+ [Getting Information About a Domain](#getting-information-about-a-domain)
+ [Registering a Domain](#registering-a-domain)
+ [Deprecating a Domain](#deprecating-a-domain)
+ [See Also](#cli-using-swf-domains-see-also)

## Listing Your Domains<a name="listing-your-domains"></a>

To list the Amazon SWF domains that you have registered for your account, you can use `swf list-domains`\. There is only one required parameter: `--registration-status`, which you can set to either `REGISTERED` or `DEPRECATED`\.

Here's a minimal example:

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
For an example of using `DEPRECATED`, see [Deprecating a Domain](#deprecating-a-domain)\. As you might guess, it returns any deprecated domains you have\.

### Setting a Page Size to Limit Results<a name="set-page-size"></a>

If you have many domains, you can set the `--maximum-page-size` parameter to limit the number of results returned\. If you get more results than the maximum number that you specified, you will receive a `nextPageToken` that you can send to the next call to `list-domains` to retrieve additional entries\.

Here's an example of using `--maximum-page-size`:

```
$ aws swf list-domains --registration-status REGISTERED --maximum-page-size 1
{
    "domainInfos": [
        {
            "status": "REGISTERED",
            "name": "ExampleDomain"
        }
    ],
    "nextPageToken": "ANeXAMPLEtOKENiSpRETTYlONG=="
}
```

**Note**  
The `nextPageToken` that is returned to you will be much longer\. This value is merely an example for illustrative purposes\.

 When you make the call again, this time supplying the value of `nextPageToken` in the `--next-page-token` argument, you'll get another page of results:

```
$ aws swf list-domains --registration-status REGISTERED --maximum-page-size 1 --next-page-token "ANeXAMPLEtOKENiSpRETTYlONG=="
{
    "domainInfos": [
        {
            "status": "REGISTERED",
            "name": "mytest"
        }
    ]
}
```

When there are no further pages of results to retrieve, `nextPageToken` will not be returned in the results\.

## Getting Information About a Domain<a name="getting-information-about-a-domain"></a>

To get detailed information about a particular domain, use `swf describe-domain`\. There is one required parameter: `--name`, which takes the name of the domain you want information about\. For example:

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

## Registering a Domain<a name="registering-a-domain"></a>

To register new domains, use `swf register-domain`\. There are two required parameters, `--name`, which takes the domain name, and `--workflow-execution-retention-period-in-days`, which takes an integer to specify the number of days to retain workflow execution data on this domain, up to a maximum period of 90 days \(for more information, see the [Amazon SWF FAQ](http://aws.amazon.com/swf/faqs/#retain_limit)\)\. If you specify zero \(0\) for this value, the retention period is automatically set at the maximum duration\. Otherwise, workflow execution data will not be retained after the specified number of days have passed\.

Here's an example of registering a new domain:

```
$ aws swf register-domain --name MyNeatNewDomain --workflow-execution-retention-period-in-days 0
```

When you register a domain, nothing is returned \(""\), but you can use `swf list-domains` or `swf describe-domain` to see the new domain\. For example:

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
            "name": "MyNeatNewDomain"
        },
        {
            "status": "REGISTERED",
            "name": "mytest"
        }
    ]
}
```

Here's an example using `swf describe-domain`:

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

## Deprecating a Domain<a name="deprecating-a-domain"></a>

To deprecate a domain \(you can still see it, but cannot create new workflow executions or register types on it\), use `swf deprecate-domain`\. It has a sole required parameter, `--name`, which takes the name of the domain to deprecate\.

```
$ aws swf deprecate-domain --name MyNeatNewDomain
```

As with `register-domain`, no output is returned\. If you use `list-domains` to view the registered domains, however, you will see that the domain no longer appears among them\.

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

You can see deprecated domains by using `--registration-status DEPRECATED` with `list-domains`\.

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

You can also use `describe-domain` to get information about a deprecated domain\.

```
$ aws swf describe-domain --name MyNeatNewDomain
{
    "domainInfo": {
        "status": "DEPRECATED",
        "name": "MyNeatNewDomain"
    },
    "configuration": {
        "workflowExecutionRetentionPeriodInDays": "0"
    }
}
```

## See Also<a name="cli-using-swf-domains-see-also"></a>

+ [deprecate\-domain](http://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-domain.html) in the *AWS Command Line Interface Reference*

+ [describe\-domain](http://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html) in the *AWS Command Line Interface Reference*

+ [list\-domains](http://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html) in the *AWS Command Line Interface Reference*

+ [register\-domain](http://docs.aws.amazon.com/cli/latest/reference/swf/register-domain.html) in the *AWS Command Line Interface Reference*