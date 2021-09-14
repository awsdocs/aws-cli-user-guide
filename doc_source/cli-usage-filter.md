# Filtering AWS CLI output<a name="cli-usage-filter"></a>

The AWS Command Line Interface \(AWS CLI\) has both server\-side and client\-side filtering that you can use individually or together to filter your AWS CLI output\. Server\-side filtering is processed first and returns your output for client\-side filtering\. 
+ Server\-side filtering is supported by the API, and you usually implement it with a `--filter` parameter\. The service only returns matching results which can speed up HTTP response times for large data sets\.
+ Client\-side filtering is supported by the AWS CLI client using the `--query` parameter\. This parameter has capabilities the server\-side filtering may not have\.

**Topics**
+ [Server\-side filtering](#cli-usage-filter-server-side)
+ [Client\-side filtering](#cli-usage-filter-client-side)
+ [Combining server\-side and client\-side filtering](#cli-usage-filter-combining)
+ [Additional resources](#cli-usage-filter-resources)

## Server\-side filtering<a name="cli-usage-filter-server-side"></a>

Server\-side filtering in the AWS CLI is provided by the AWS service API\. The AWS service only returns the records in the HTTP response that match your filter, which can speed up HTTP response times for large data sets\. Since server\-side filtering is defined by the service API, the parameter names and functions vary between services\. Some common parameter names used for filtering are: 
+ `--filter` such as [ses](https://docs.aws.amazon.com/cli/latest/reference/ses/create-receipt-filter.html) and [ce](https://docs.aws.amazon.com/cli/latest/reference/ce/get-cost-and-usage.html)\. 
+ `--filters` such as [ec2](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-volumes.html), [autoscaling](https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-tags.html), and [rds](https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html)\. 
+ Names starting with the word `filter`, for example `--filter-expression` for the [https://docs.aws.amazon.com/cli/latest/reference/dynamodb/scan.html](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/scan.html) command\.

For information about whether a specific command has server\-side filtering and the filtering rules, see the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

## Client\-side filtering<a name="cli-usage-filter-client-side"></a>

The AWS CLI provides built\-in JSON\-based client\-side filtering capabilities with the `--query` parameter\. The `--query` parameter is a powerful tool you can use to customize the content and style of your output\. The `--query` parameter takes the HTTP response that comes back from the server and filters the results before displaying them\. Since the entire HTTP response is sent to the client before filtering, client\-side filtering can be slower than server\-side filtering for large data\-sets\.

Querying uses [JMESPath syntax](http://jmespath.org/) to create expressions for filtering your output\. To learn JMESPath syntax, see [Tutorial](https://jmespath.org/tutorial.html) on the *JMESPath website*\.

**Important**  
The output type you specify changes how the `--query` option operates:  
If you specify `--output text`, the output is paginated *before* the `--query` filter is applied, and the AWS CLI runs the query once on *each page* of the output\. Due to this, the query includes the first matching element on each page which can result in unexpected extra output\. To work around this extra output, you can specify `--no-paginate` to apply the filter only to the complete set of results, but may result in long output\. To additionally filter the output, you can use other command line tools such as `head` or `tail`\.
If you specify `--output json`, `--output yaml`, or `--output yaml-stream` the output is completely processed as a single, native structure before the `--query` filter is applied\. The AWS CLI runs the query only once against the entire structure, producing a filtered result that is then output\.

**Topics**
+ [Before you start](#cli-usage-filter-client-side-output)
+ [Identifiers](#cli-usage-filter-client-side-identifiers)
+ [Selecting from a list](#cli-usage-filter-client-side-select-list)
+ [Filtering nested data](#cli-usage-filter-client-side-nested)
+ [Flattening results](#cli-usage-filter-client-side-specific-flattening)
+ [Filtering for specific values](#cli-usage-filter-client-side-specific-values)
+ [Piping expressions](#cli-usage-filter-client-side-pipe)
+ [Filtering for multiple identifier values](#cli-usage-filter-client-side-miltiselect-list)
+ [Adding labels to identifier values](#cli-usage-filter-client-side-multiselect-hash)
+ [Functions](#cli-usage-filter-client-side-functions)
+ [Advanced `--query` examples](#cli-usage-filter-client-side-advanced)

### Before you start<a name="cli-usage-filter-client-side-output"></a>

When using filter expressions used in these examples, be sure to use the correct quoting rules for your terminal shell\. For more information, see [Using quotation marks with strings in the AWS CLI](cli-usage-parameters-quoting-strings.md)\.

The following JSON output shows an example of what the `--query` parameter can produce\. The output describes three Amazon EBS volumes attached to separate Amazon EC2 instances\.

#### Example output<a name="cli-usage-filter-client-side-output-example"></a>

```
$ aws ec2 describe-volumes
{
  "Volumes": [
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2013-09-17T00:55:03.000Z",
          "InstanceId": "i-a071c394",
          "VolumeId": "vol-e11a5288",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-e11a5288",
      "State": "in-use",
      "SnapshotId": "snap-f23ec1c8",
      "CreateTime": "2013-09-17T00:55:03.000Z",
      "Size": 30
    },
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2013-09-18T20:26:16.000Z",
          "InstanceId": "i-4b41a37c",
          "VolumeId": "vol-2e410a47",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-2e410a47",
      "State": "in-use",
      "SnapshotId": "snap-708e8348",
      "CreateTime": "2013-09-18T20:26:15.000Z",
      "Size": 8
    },
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2020-11-20T19:54:06.000Z",
          "InstanceId": "i-1jd73kv8",
          "VolumeId": "vol-a1b3c7nd",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-a1b3c7nd",
      "State": "in-use",
      "SnapshotId": "snap-234087fb",
      "CreateTime": "2020-11-20T19:54:05.000Z",
      "Size": 15
    }
  ]
}
```

### Identifiers<a name="cli-usage-filter-client-side-identifiers"></a>

Identifier are the labels for output values\. When creating filters, you use identifiers to narrow down your query results\. In the following output example, all identifiers such as `Volumes`, `AvailabilityZone`, and `AttachTime` are highlighted\. 

```
$ aws ec2 describe-volumes
{
  "Volumes": [
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2013-09-17T00:55:03.000Z",
          "InstanceId": "i-a071c394",
          "VolumeId": "vol-e11a5288",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-e11a5288",
      "State": "in-use",
      "SnapshotId": "snap-f23ec1c8",
      "CreateTime": "2013-09-17T00:55:03.000Z",
      "Size": 30
    },
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2013-09-18T20:26:16.000Z",
          "InstanceId": "i-4b41a37c",
          "VolumeId": "vol-2e410a47",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-2e410a47",
      "State": "in-use",
      "SnapshotId": "snap-708e8348",
      "CreateTime": "2013-09-18T20:26:15.000Z",
      "Size": 8
    },
    {
      "AvailabilityZone": "us-west-2a",
      "Attachments": [
        {
          "AttachTime": "2020-11-20T19:54:06.000Z",
          "InstanceId": "i-1jd73kv8",
          "VolumeId": "vol-a1b3c7nd",
          "State": "attached",
          "DeleteOnTermination": true,
          "Device": "/dev/sda1"
        }
      ],
      "VolumeType": "standard",
      "VolumeId": "vol-a1b3c7nd",
      "State": "in-use",
      "SnapshotId": "snap-234087fb",
      "CreateTime": "2020-11-20T19:54:05.000Z",
      "Size": 15
    }
  ]
}
```

For more information, see [Identifiers](https://jmespath.org/specification.html#identifiers ) on the *JMESPath website*\.

### Selecting from a list<a name="cli-usage-filter-client-side-select-list"></a>

A list or array is an identifier that is followed by a square bracket "`[`" such as `Volumes` and `Attachments` in the [Before you start](#cli-usage-filter-client-side-output)\. 

**Syntax**

```
<listName>[ ]
```

To filter through all output from an array, you can use the wildcard notation\. [Wildcard](http://jmespath.org/specification.html#wildcard-expressions) expressions are expressions used to return elements using the `*` notation\. 

The following example queries all `Volumes` content\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*]'
[
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2013-09-17T00:55:03.000Z",
        "InstanceId": "i-a071c394",
        "VolumeId": "vol-e11a5288",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-e11a5288",
    "State": "in-use",
    "SnapshotId": "snap-f23ec1c8",
    "CreateTime": "2013-09-17T00:55:03.000Z",
    "Size": 30
  },
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2020-11-20T19:54:06.000Z",
        "InstanceId": "i-1jd73kv8",
        "VolumeId": "vol-a1b3c7nd",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-a1b3c7nd",
    "State": "in-use",
    "SnapshotId": "snap-234087fb",
    "CreateTime": "2020-11-20T19:54:05.000Z",
    "Size": 15
  }
]
```

To view a specific volume in the array by index, you call the array index\. For example, the first item in the `Volumes` array has an index of 0, resulting in the `Volumes[0]` query\. For more information about array indexes, see [index expressions](http://jmespath.org/specification.html#index-expressions) on the *JMESPath website*\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[0]'
{
  "AvailabilityZone": "us-west-2a",
  "Attachments": [
    {
      "AttachTime": "2013-09-17T00:55:03.000Z",
      "InstanceId": "i-a071c394",
      "VolumeId": "vol-e11a5288",
      "State": "attached",
      "DeleteOnTermination": true,
      "Device": "/dev/sda1"
    }
  ],
  "VolumeType": "standard",
  "VolumeId": "vol-e11a5288",
  "State": "in-use",
  "SnapshotId": "snap-f23ec1c8",
  "CreateTime": "2013-09-17T00:55:03.000Z",
  "Size": 30
}
```

To view a specific range of volumes by index, use `slice` with the following syntax, where **start** is the starting array index, **stop** is the index where the filter stops processing, and **step** is the skip interval\. 

**Syntax**

```
<arrayName>[<start>:<stop>:<step>]
```

If any of these are omitted from the slice expression, they use the following default values:
+ Start – The first index in the list, 0\.
+ Stop – The last index in the list\.
+ Step – No step skipping, where the value is 1\.

To return only the first two volumes, you use a start value of 0, a stop value of 2, and a step value of 1 as shown in the following example\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[0:2:1]'
[
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2013-09-17T00:55:03.000Z",
        "InstanceId": "i-a071c394",
        "VolumeId": "vol-e11a5288",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-e11a5288",
    "State": "in-use",
    "SnapshotId": "snap-f23ec1c8",
    "CreateTime": "2013-09-17T00:55:03.000Z",
    "Size": 30
  },
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2013-09-18T20:26:16.000Z",
        "InstanceId": "i-4b41a37c",
        "VolumeId": "vol-2e410a47",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-2e410a47",
    "State": "in-use",
    "SnapshotId": "snap-708e8348",
    "CreateTime": "2013-09-18T20:26:15.000Z",
    "Size": 8
  }
]
```

Since this example contains default values, you can shorten the slice from `Volumes[0:2:1]` to `Volumes[:2]`\.

The following example omits default values and returns every two volumes in the entire array\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[::2]'
[
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2013-09-17T00:55:03.000Z",
        "InstanceId": "i-a071c394",
        "VolumeId": "vol-e11a5288",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-e11a5288",
    "State": "in-use",
    "SnapshotId": "snap-f23ec1c8",
    "CreateTime": "2013-09-17T00:55:03.000Z",
    "Size": 30
  },
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2020-11-20T19:54:06.000Z",
        "InstanceId": "i-1jd73kv8",
        "VolumeId": "vol-a1b3c7nd",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-a1b3c7nd",
    "State": "in-use",
    "SnapshotId": "snap-234087fb",
    "CreateTime": "2020-11-20T19:54:05.000Z",
    "Size": 15
  }
]
```

Steps can also use negative numbers to filter in the reverse order of an array as shown in the following example\. 

```
$ aws ec2 describe-volumes \
    --query 'Volumes[::-2]'
[
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2020-11-20T19:54:06.000Z",
        "InstanceId": "i-1jd73kv8",
        "VolumeId": "vol-a1b3c7nd",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-a1b3c7nd",
    "State": "in-use",
    "SnapshotId": "snap-234087fb",
    "CreateTime": "2020-11-20T19:54:05.000Z",
    "Size": 15
  },
  {
    "AvailabilityZone": "us-west-2a",
    "Attachments": [
      {
        "AttachTime": "2013-09-17T00:55:03.000Z",
        "InstanceId": "i-a071c394",
        "VolumeId": "vol-e11a5288",
        "State": "attached",
        "DeleteOnTermination": true,
        "Device": "/dev/sda1"
      }
    ],
    "VolumeType": "standard",
    "VolumeId": "vol-e11a5288",
    "State": "in-use",
    "SnapshotId": "snap-f23ec1c8",
    "CreateTime": "2013-09-17T00:55:03.000Z",
    "Size": 30
  }
]
```

For more information, see [Slices](https://jmespath.org/specification.html#slices) on the *JMESPath website*\.

### Filtering nested data<a name="cli-usage-filter-client-side-nested"></a>

To narrow the filtering of the `Volumes[*]` for nested values, you use subexpressions by appending a period and your filter criteria\.

**Syntax**

```
<expression>.<expression>
```

The following example shows all `Attachments` information for all volumes\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments'
[
  [
    {
      "AttachTime": "2013-09-17T00:55:03.000Z",
      "InstanceId": "i-a071c394",
      "VolumeId": "vol-e11a5288",
      "State": "attached",
      "DeleteOnTermination": true,
      "Device": "/dev/sda1"
    }
  ],
  [
    {
      "AttachTime": "2013-09-18T20:26:16.000Z",
      "InstanceId": "i-4b41a37c",
      "VolumeId": "vol-2e410a47",
      "State": "attached",
      "DeleteOnTermination": true,
      "Device": "/dev/sda1"
    }
  ],
  [
    {
      "AttachTime": "2020-11-20T19:54:06.000Z",
      "InstanceId": "i-1jd73kv8",
      "VolumeId": "vol-a1b3c7nd",
      "State": "attached",
      "DeleteOnTermination": true,
      "Device": "/dev/sda1"
    }
  ]
]
```

To filter further into the nested values, append the expression for each nested indentifier\. The following example lists the `State` for all `Volumes`\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[*].State'
[
  [
    "attached"
  ],
  [
    "attached"
  ],
  [
    "attached"
  ]
]
```

### Flattening results<a name="cli-usage-filter-client-side-specific-flattening"></a>

For more information, see [SubExpressions](https://jmespath.org/specification.html#subexpressions) on the *JMESPath website*\.

You can flatten the results for `Volumes[*].Attachments[*].State` by removing the wildcard notation resulting in the `Volumes[*].Attachments[].State` query\. Flattening often is useful to improve the readablity of results\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[].State'
[
  "attached",
  "attached",
  "attached"
]
```

For more information, see [Flatten](https://jmespath.org/specification.html#flatten) on the *JMESPath website*\.

### Filtering for specific values<a name="cli-usage-filter-client-side-specific-values"></a>

To filter for specific values in a list, you use a filter expression as shown in the following syntax\.

**Syntax**

```
? <expression> <comparator> <expression>]
```

Expression comparators include `==`, `!=`, `<`, `<=`, `>`, and `>=` \. The following example filters for the `VolumeIds` for all `Volumes` in an `Attached``State`\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[?State=='attached'].VolumeId'
[
  [
    "vol-e11a5288"
  ],
  [
    "vol-2e410a47"
  ],
  [
    "vol-a1b3c7nd"
  ]
]
```

This can then be flattened resulting in the following example\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[?State=='attached'].VolumeId[]'
[
  "vol-e11a5288",
  "vol-2e410a47",
  "vol-a1b3c7nd"
]
```

The following example filters for the `VolumeIds` of all `Volumes` that have a size less than 20\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[?Size < `20`].VolumeId'
[
  "vol-2e410a47",
  "vol-a1b3c7nd"
]
```

For more information, see [Filter Expressions](https://jmespath.org/specification.html#filterexpressions) on the *JMESPath website*\.

### Piping expressions<a name="cli-usage-filter-client-side-pipe"></a>

You can pipe results of a filter to a new list, and then filter the result with another expression using the following syntax: 

**Syntax**

```
<expression> | <expression>] 
```

The following example takes the filter results of the `Volumes[*].Attachments[].InstanceId` expression and outputs the first result in the array\. 

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[].InstanceId | [0]'
"i-a071c394"
```

This example does this by first creating the array from the following expression\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[*].Attachments[].InstanceId'
"i-a071c394",
  "i-4b41a37c",
  "i-1jd73kv8"
```

And then returns the first element in that array\.

```
"i-a071c394"
```

For more information, see [Pipe Expressions](https://jmespath.org/specification.html#pipe-expressions) on the *JMESPath website*\.

### Filtering for multiple identifier values<a name="cli-usage-filter-client-side-miltiselect-list"></a>

To filter for multiple identifiers, you use a multiselect list by using the following syntax: 

**Syntax**

```
<listName>[].[<expression>, <expression>]
```

In the following example, `VolumeId` and `VolumeType` are filtered in the `Volumes` list resulting in the following expression\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[].[VolumeId, VolumeType]'
[
  [
    "vol-e11a5288",
    "standard"
  ],
  [
    "vol-2e410a47",
    "standard"
  ],
  [
    "vol-a1b3c7nd",
    "standard"
  ]
]
```

To add nested data to the list, you add another multiselect list\. The following example expands on the previous example by also filtering for `InstanceId` and `State` in the nested `Attachments` list\. This results in the following expression\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[].[VolumeId, VolumeType, Attachments[].[InstanceId, State]]'
[
  [
    "vol-e11a5288",
    "standard",
    [
      [
        "i-a071c394",
        "attached"
      ]
    ]
  ],
  [
    "vol-2e410a47",
    "standard",
    [
      [
        "i-4b41a37c",
        "attached"
      ]
    ]
  ],
  [
    "vol-a1b3c7nd",
    "standard",
    [
      [
        "i-1jd73kv8",
        "attached"
      ]
    ]
  ]
]
```

To be more readable, flatten out the expression as shown in the following example\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[].[VolumeId, VolumeType, Attachments[].[InstanceId, VolumeId][]][]'
[
  "vol-e11a5288",
  "standard",
  [
    "i-a071c394",
    "attached"
  ],
  "vol-2e410a47",
  "standard",
  [
    "i-4b41a37c",
    "attached"
  ],
  "vol-a1b3c7nd",
  "standard",
  [
    "i-1jd73kv8",
    "attached"
  ]
]
```

For more information, see [Multiselect list](https://jmespath.org/specification.html#multiselectlist) on the *JMESPath website*\.

### Adding labels to identifier values<a name="cli-usage-filter-client-side-multiselect-hash"></a>

To make this output easier to read, use a multiselect hash with the following syntax\.

**Syntax**

```
<listName>[].{<label>: <expression>, <label>: <expression>}
```

Your identifier label does not need to be the same as the name of the identifier\. The following example uses the label `Type` for the `VolumeType` values\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[].{VolumeType: VolumeType}'
[
  {
    "Type": "standard",
  },
  {
    "Type": "standard",
  },
  {
    "Type": "standard",
  }
]
```

For simplicity, the following example keeps the identifier names for each label and displays the `VolumeId`, `VolumeType`, `InstanceId`, and `State` for all volumes:

```
$ aws ec2 describe-volumes \
    --query 'Volumes[].{VolumeId: VolumeId, VolumeType: VolumeType, InstanceId: Attachments[0].InstanceId, State: Attachments[0].State}'
[
  {
    "VolumeId": "vol-e11a5288",
    "VolumeType": "standard",
    "InstanceId": "i-a071c394",
    "State": "attached"
  },
  {
    "VolumeId": "vol-2e410a47",
    "VolumeType": "standard",
    "InstanceId": "i-4b41a37c",
    "State": "attached"
  },
  {
    "VolumeId": "vol-a1b3c7nd",
    "VolumeType": "standard",
    "InstanceId": "i-1jd73kv8",
    "State": "attached"
  }
]
```

For more information, see [Multiselect hash](https://jmespath.org/specification.html#multiselecthash) on the *JMESPath website*\.

### Functions<a name="cli-usage-filter-client-side-functions"></a>

The JMESPath syntax contains many functions that you can use for your queries\. For information on JMESPath functions, see [Built\-in Functions](https://jmespath.org/specification.html#built-in-functions) on the *JMESPath website*\.

To demonstrate how you can incorporate a function into your queries, the following example uses the `sort_by` function\. The `sort_by` function sorts an array using an expression as the sort key using the following syntax:

**Syntax**

```
sort_by(<listName>, <sort expression>)[].<expression>
```

The following example uses the previous [multiselect hash example](#cli-usage-filter-client-side-multiselect-hash) and sorts the output by `VolumeId`\. 

```
$ aws ec2 describe-volumes \
    --query 'sort_by(Volumes, &VolumeId)[].{VolumeId: VolumeId, VolumeType: VolumeType, InstanceId: Attachments[0].InstanceId, State: Attachments[0].State}'
[
  {
    "VolumeId": "vol-2e410a47",
    "VolumeType": "standard",
    "InstanceId": "i-4b41a37c",
    "State": "attached"
  },
  {
    "VolumeId": "vol-a1b3c7nd",
    "VolumeType": "standard",
    "InstanceId": "i-1jd73kv8",
    "State": "attached"
  },
  {
    "VolumeId": "vol-e11a5288",
    "VolumeType": "standard",
    "InstanceId": "i-a071c394",
    "State": "attached"
  }
]
```

For more information, see [sort\_by](https://jmespath.org/specification.html#sort-by) on the *JMESPath website*\.

### Advanced `--query` examples<a name="cli-usage-filter-client-side-advanced"></a>

**To extract information from a specific item**

The following example uses the `--query` parameter to find a specific item in a list and then extracts information from that item\. The example lists all of the `AvailabilityZones` associated with the specified service endpoint\. It extracts the item from the `ServiceDetails` list that has the specified `ServiceName`, then outputs the `AvailabilityZones` field from that selected item\. 

```
$ aws --region us-east-1 ec2 describe-vpc-endpoint-services \
    --query 'ServiceDetails[?ServiceName==`com.amazonaws.us-east-1.ecs`].AvailabilityZones'
[
    [
        "us-east-1a",
        "us-east-1b",
        "us-east-1c",
        "us-east-1d",
        "us-east-1e",
        "us-east-1f"
    ]
]
```

**To show snapshots after the specified creation date**

The following example shows how to list all of your snapshots that were created after a specified date, including only a few of the available fields in the output\.

```
$ aws ec2 describe-snapshots --owner self \
    --output json \
    --query 'Snapshots[?StartTime>=`2018-02-07`].{Id:SnapshotId,VId:VolumeId,Size:VolumeSize}' \
[
    {
        "id": "snap-0effb42b7a1b2c3d4",
        "vid": "vol-0be9bb0bf12345678",
        "Size": 8
    }
]
```

**To show the most recent AMIs**

The following example lists the five most recent Amazon Machine Images \(AMIs\) that you created, sorted from most recent to oldest\.

```
$ aws ec2 describe-images \
    --owners self \
    --query 'reverse(sort_by(Images,&CreationDate))[:5].{id:ImageId,date:CreationDate}'
[
    {
        "id": "ami-0a1b2c3d4e5f60001",
        "date": "2018-11-28T17:16:38.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60002",
        "date": "2018-09-15T13:51:22.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60003",
        "date": "2018-08-19T10:22:45.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60004",
        "date": "2018-05-03T12:04:02.000Z"
    },
    {
        "id": "ami-0a1b2c3d4e5f60005",
        "date": "2017-12-13T17:16:38.000Z"
    }
]
```

**To show unhealthy Auto Scaling instances**

The following example shows only the `InstanceId` for any unhealthy instances in the specified Auto Scaling group\.

```
$ aws autoscaling describe-auto-scaling-groups \
    --auto-scaling-group-name My-AutoScaling-Group-Name \
    --output text \
    --query 'AutoScalingGroups[*].Instances[?HealthStatus==`Unhealthy`].InstanceId'
```

**To exclude volumes with the specified tag**

The following example describes all instances without a `test` tag\. Using a simple `?Value != `test`` expression does not work for excluding a volume as volumes can have multiple tags\. As long as there is another tag beside `test` attached to the volume, the volume is still returned in the results\.

To exclude all volumes with the `test` tag, start with the below expression to return all tags with the `test` tag in an array\. Any tags that are not the `test` tag contain a `null` value\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes.Tags[?Value == `test`]'
```

Then filter out all the positive `test` results using the `not_null` function\. 

```
$ aws ec2 describe-volumes \
    --query 'Volumes[?not_null(Tags[?Value == `test`].Value)]'
```

Pipe the results to flatten out the results resulting in the following query\.

```
$ aws ec2 describe-volumes \
    --query 'Volumes[?not_null(Tags[?Value == `test`].Value)] | []'
```

## Combining server\-side and client\-side filtering<a name="cli-usage-filter-combining"></a>

You can use server\-side and client\-side filtering together\. Server\-side filtering is completed first, which sends the data to the client that the `--query` parameter then filters\. If you're using large data sets, using server\-side filtering first can lower the amount of data sent to the client for each AWS CLI call, while still keeping the powerful customization that client\-side filtering provides\.

The following example lists Amazon EC2 volumes using both server\-side and client\-side filtering\. The service filters a list of all attached volumes in the `us-west-2a` Availability Zone\. The `--query` parameter further limits the output to only those volumes with a `Size` value that is larger than 50, and shows only the specified fields with user\-defined names\.

```
$ aws ec2 describe-volumes \
    --filters "Name=availability-zone,Values=us-west-2a" "Name=status,Values=attached" \
    --query 'Volumes[?Size > `50`].{Id:VolumeId,Size:Size,Type:VolumeType}'
[
    {
        "Id": "vol-0be9bb0bf12345678",
        "Size": 80,
        "Type": "gp2"
    }
]
```

The following example retrieves a list of images that meet several criteria\. It then uses the `--query` parameter to sort the output by `CreationDate`, selecting only the most recent\. Finally, it displays the `ImageId` of that one image\.

```
$ aws ec2 describe-images \
    --owners amazon \
    --filters "Name=name,Values=amzn*gp2" "Name=virtualization-type,Values=hvm" "Name=root-device-type,Values=ebs" \
    --query "sort_by(Images, &CreationDate)[-1].ImageId" \
    --output text
ami-00ced3122871a4921
```

The following example displays the number of available volumes that are more than 1000 IOPS by using `length` to count how many are in a list\.

```
$ aws ec2 describe-volumes \
    --filters "Name=status,Values=available" \
    --query 'length(Volumes[?Iops > `1000`])'
3
```

## Additional resources<a name="cli-usage-filter-resources"></a>

**AWS CLI autoprompt**  
When beginning to use filter expressions, you can use the auto\-prompt feature in the AWS CLI version 2\. The auto\-prompt feature provides a preview when you press the **F5** key\. For more information, see [Having the AWS CLI prompt you for commands](cli-usage-parameters-prompting.md)\.

**JMESPath Terminal**  
JMESPath Terminal is an interactive terminal command to experiment with JMESPath expressions that are used for client\-side filtering\. Using the `jpterm` command, the terminal shows immediate query results as you're typing\. You can directly pipe AWS CLI output to the terminal, enabling advanced querying experimentation\.   
The following example pipes `aws ec2 describe-volumes` output directly to JMESPath Terminal\.  

```
$ aws ec2 describe-volumes | jpterm
```
For more information on JMESPath Terminal and installation instructions, see [JMESPath Terminal](https://github.com/jmespath/jmespath.terminal) on *GitHub*\.

**jq utility**  
The `jq` utility provides you a way to transform your output on the client\-side to an output format you desire\. For more information on `jq` and installation instructions, see [jq](https://stedolan.github.io/jq/) on *GitHub*\.
