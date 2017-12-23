# Using the AWS Command Line Interface's Pagination Options<a name="pagination"></a>

For commands that can return a large list of items, the AWS CLI adds three options that you can use to modify the pagination behavior of the CLI when it calls a service's API to populate the list\.

By default, the CLI uses a page size of 1000 and retrieves all available items\. For example, if you run `aws s3api list-objects` on an Amazon S3 bucket containing 3500 objects, the CLI makes four calls to Amazon S3, handling the service specific pagination logic in the background\.

If you see issues when running list commands on a large number of resources, the default page size may be too high, causing calls to AWS services to time out\. You can use the `--page-size` option to specify a smaller page size to solve this issue\. The CLI will still retrieve the full list, but will perform a larger number of calls in the background, retrieving a smaller number of items with each call:

```
$ aws s3api list-objects --bucket my-bucket --page-size 100
{
    "Contents": [
...
```

To retrieve fewer items, use the `--max-items` option\. The CLI will handle pagination in the same way, but will only print out the number of items that you specify:

```
$ aws s3api list-objects --bucket my-bucket --max-items 100
{
    "NextToken": "eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==",
    "Contents": [
...
```

If the number of items output \(`--max-items`\) is fewer than the total number of items, the output includes a `NextToken` that you can pass in a subsequent command to retrieve the next set of items:

```
$ aws s3api list-objects --bucket my-bucket --max-items 100 --starting-token eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAxfQ==
{
    "NextToken": "eyJNYXJrZXIiOiBudWxsLCAiYm90b190cnVuY2F0ZV9hbW91bnQiOiAyfQ==",
    "Contents": [
...
```

A service may not return items in the same order each time you call\. If you specify a next token in the middle of a page, you may see different results that you expect\. To prevent this, use the same number for `--page-size` and `--max-items`, to sync the CLI's pagination with the service's\. You can also retrieve the whole list and perform any necessary parsing operations locally\.