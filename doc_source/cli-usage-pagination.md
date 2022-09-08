--------

**This documentation is for Version 1 of the AWS CLI only\.** For documentation related to Version 2 of the AWS CLI, see the [Version 2 User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\.

--------

# Using AWS CLI pagination options<a name="cli-usage-pagination"></a>

This topic describes the different ways to paginate output from the AWS Command Line Interface \(AWS CLI\)\. 

## Server\-side pagination<a name="cli-usage-pagination-serverside"></a>

For commands that can return a large list of items, the AWS Command Line Interface \(AWS CLI\) has multiple options to control the number of items included in the output when the AWS CLI calls a service's API to populate the list\.
+ `--no-paginate`
+ `--page-size`
+ `--max-items`
+ `--starting-token`

By default, the AWS CLI uses a page size determined by the individual service and retrieves all available items\. For example, Amazon S3 has a default page size of 1000\. If you run `aws s3api list-objects` on an Amazon S3 bucket that contains 3,500 objects, the AWS CLI automatically makes four calls to Amazon S3, handling the service\-specific pagination logic for you in the background and returning all 3,500 objects in the final output\.

### How to use the \-\-no\-paginate parameter<a name="cli-usage-pagination-nopaginate"></a>

The `--no-paginate` option disables following pagination tokens on the client side\. When using a command, by default the AWS CLI automatically makes multiple calls to return all possible results to create pagination\. One call for each page\. Disabling pagination has the AWS CLI only call once for the first page of command results\. 

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

If you see issues when running list commands on a large number of resources, the default page size might be too high\. This can cause calls to AWS services to exceed the maximum allowed time and generate a "timed out" error\. You can use the `--page-size` option to specify that the AWS CLI request a smaller number of items from each call to the AWS service\. The AWS CLI still retrieves the full list, but performs a larger number of service API calls in the background and retrieves a smaller number of items with each call\. This gives the individual calls a better chance of succeeding without a timeout\. Changing the page size doesn't affect the output; it affects only the number of API calls that need to be made to generate the output\.

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