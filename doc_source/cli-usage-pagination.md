# Using AWS CLI pagination options<a name="cli-usage-pagination"></a>

This topic describes the different ways to paginate output from the AWS Command Line Interface \(AWS CLI\)\.There are primarily two ways to control pagination from the AWS CLI\.
+ [Using server\-side pagination parameters\.](#cli-usage-pagination-serverside)
+ [Using your default output client\-side paging program](#cli-usage-pagination-clientside)\.

Server\-side pagination parameters process first and any output is sent to client\-side pagination\.

## Server\-side pagination<a name="cli-usage-pagination-serverside"></a>

For commands that can return a large list of items, the AWS Command Line Interface \(AWS CLI\) has multiple options to control the number of items included in the output when the AWS CLI calls a service's API to populate the list\.
+ `--no-paginate`
+ `--page-size`
+ `--max-items`
+ `--starting-token`

By default, the AWS CLI uses a page size of 1000 and retrieves all available items\. For example, if you run `aws s3api list-objects` on an Amazon S3 bucket that contains 3,500 objects, the AWS CLI automatically makes four calls to Amazon S3, handling the service\-specific pagination logic for you in the background and returning all 3,500 objects in the final output\.

### How to use the \-\-no\-paginate parameter<a name="cli-usage-pagination-nopaginate"></a>

To disable pagination and return only the first page of results, use the `--no-paginate` option\. When using a command, by default the AWS CLI automatically makes multiple calls to return all possible results to create pagination\. One call for each page\. Disabling pagination has the AWS CLI only call once for the first page of command results\. 

For example, if you run `aws s3api list-objects` on an Amazon S3 bucket that contains 3,500 objects, the AWS CLI only makes the first call to Amazon S3, returning only the first 1,000 objects in the final output\.

```
$ aws s3api list-objects \
    --bucket my-bucket \
    --no-paginate
{
    "Contents": [
...
```

### How to use the \-\-page\-size parameter<a name="cli-usage-pagination-pagesize"></a>

If you see issues when running list commands on a large number of resources, the default page size of 1000 might be too high\. This can cause calls to AWS services to exceed the maximum allowed time and generate a "timed out" error\. You can use the `--page-size` option to specify that the AWS CLI request a smaller number of items from each call to the AWS service\. The CLI still retrieves the full list, but performs a larger number of service API calls in the background and retrieves a smaller number of items with each call\. This gives the individual calls a better chance of succeeding without a timeout\. Changing the page size doesn't affect the output; it affects only the number of API calls that need to be made to generate the output\.

```
$ aws s3api list-objects \
    --bucket my-bucket \
    --page-size 100
{
    "Contents": [
...
```

### How to use the \-\-max\-items parameter<a name="cli-usage-pagination-maxitems"></a>

To include fewer items at a time in the AWS CLI output, use the `--max-items` option\. The AWS CLI still handles pagination with the service as described previously, but prints out only the number of items at a time that you specify\.

```
$ aws s3api list-objects \
    --bucket my-bucket \
    --max-items 100
{
    "NextToken": "eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==",
    "Contents": [
...
```

### How to use the \-\-starting\-token parameter<a name="cli-usage-pagination-startingtoken"></a>

If the number of items output \(`--max-items`\) is fewer than the total number of items returned by the underlying API calls, the output includes a `NextToken` that you can pass to a subsequent command to retrieve the next set of items\. The following example shows how to use the `NextToken` value returned by the previous example, and enables you to retrieve the second 100 items\.

**Note**  
The parameter `--starting-token` cannot be null or empty\. If the previous command does not return a `NextToken` value, there are no more items to return and you do not need to call the command again\.

```
$ aws s3api list-objects \
    --bucket my-bucket \
    --max-items 100 \
    --starting-token eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==
{
    "Contents": [
...
```

The specified AWS service might not return items in the same order each time you call\. If you specify different values for `--page-size` and `--max-items`, you can get unexpected results with missing or duplicated items\. To prevent this, use the same number for `--page-size` and `--max-items` to sync the AWS CLI pagination with the pagination of the underlying service\. You can also retrieve the whole list and perform any necessary paging operations locally\.

## Client\-side pager<a name="cli-usage-pagination-clientside"></a>

**This feature is available only with AWS CLI version 2\.**  
The following feature is available only if you use AWS CLI version 2\. It isn't available if you run AWS CLI version 1\. For information on how to install version 2, see [Installing the AWS CLI version 2](install-cliv2.md)\.

AWS CLI version 2 provides the use of a client\-side pager program for output\. By default, this feature returns all output through your operating system’s default pager program\. 

In order of precedence, you can specify the output pager in the following ways:
+ Using the `cli_pager` setting in the `config` file in a named profile\.
+ Using the `AWS_PAGER` environment variable\.
+ Using the `cli_pager` setting in the `config` file in `default` profile\.
+ Using the `PAGER` environment variable\.

In order of precedence, you can disable all use of an external paging program in the following ways:
+ Use the `--no-cli-pager` command line option to disable the pager for a single command use\. 
+ Set the `cli_pager` setting or `AWS_PAGER` variable to an empty string\.

### How to use the cli\_pager setting<a name="cli-usage-pagination-clipager"></a>

You can save your frequently used configuration settings and credentials in files that are maintained by the AWS CLI\. Settings in a name profile take precedence over settings in the `default` profile\. For more information on configuration settings, see [Configuration and credential file settings](cli-configure-files.md)\.

The following example sets the default output pager to the `less` program\.

```
[default]
cli_pager=less
```

The following example sets the default to disable the use of a pager\.

```
[default]
cli_pager=
```

### How to use the AWS\_PAGER environment variable<a name="cli-usage-pagination-awspager"></a>

The following example sets the default output pager to the `less` program\. For more information on environment variables, see[Environment variables to configure the AWS CLI](cli-configure-envvars.md)\.

------
#### [ Linux and macOS ]

```
$ export AWS_PAGER="less"
```

------
#### [ Windows ]

```
C:\> setx AWS_PAGER "less"
```

------

The following example disables the use of a pager\.

------
#### [ Linux and macOS ]

```
$ export AWS_PAGER=""
```

------
#### [ Windows ]

```
C:\> setx AWS_PAGER ""
```

------

### How to use the \-\-no\-cli\-pager option<a name="cli-usage-pagination-noclipager"></a>

To disable the use of a pager on a single command, use the `--no-cli-pager` option\. For more information on command line options, see [Command line options](cli-configure-options.md)\.

```
$ aws s3api list-objects \
    --bucket my-bucket \
    --no-cli-pager
{
    "Contents": [
...
```

### How to use pager flags<a name="cli-usage-pagination-flags"></a>

You can specify flags to use automatically with your paging program\. Flags are dependent on the paging program you use\. The below examples are for the typical defaults of `less` and `more`\.

------
#### [ Linux and macOS ]

If you do not specify otherwise, the pager AWS CLI version 2 uses by default is `less`\. If you don't have the `LESS` environment variable set, the AWS CLI version 2 uses the `FRX` flags\. You can combine flags by specifying them when setting the AWS CLI pager\.

The following example uses the `S` flag\. This flag then combines with the default `FRX` flags to create a final `FRXS` flag\.

```
$ export AWS_PAGER="less -S"
```

If you don't want any of the `FRX` flags, you can negate them\. The following example negates the `F` flag to create a final `RX` flag\.

```
$ export AWS_PAGER="less -+F"
```

For more information on `less` flags see [less](http://manpages.org/less/1#options) on *manpages\.org*\. 

------
#### [ Windows ]

If you do not specify otherwise, the pager AWS CLI version 2 uses by default is `more` with no additional flags\.

The following example uses the `/c` parameter\.

```
C:\> setx AWS_PAGER "more /c"
```

For more information on `more` flags see [more](http://manpages.org/less/1#options) on *Microsoft Docs*\.

------