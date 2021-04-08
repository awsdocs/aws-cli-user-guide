# Using Amazon DynamoDB with the AWS CLI<a name="cli-services-dynamodb"></a>

The AWS Command Line Interface \(AWS CLI\) provides support for all of the AWS database services, including Amazon DynamoDB\. You can use the AWS CLI for ad hoc operations, such as creating a table\. You can also use it to embed DynamoDB operations within utility scripts\. 

For more information about using the AWS CLI with DynamoDB, see [DynamoDB](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html) in the *AWS CLI Command Reference*\.

To list the AWS CLI commands for DynamoDB, use the following command\.

```
$ aws dynamodb help
```

**Topics**
+ [Prerequisites](#cli-services-dynamodb-prereqs)
+ [Creating and using DynamoDB tables](#cli-services-dynamodb-using)
+ [Using DynamoDB Local](#cli-services-dynamodb-local)
+ [Resources](#cli-services-dynamodb-resources)

## Prerequisites<a name="cli-services-dynamodb-prereqs"></a>

To run the `dynamodb` commands, you need to:
+ AWS CLI installed, see [Installing, updating, and uninstalling the AWS CLI](cli-chap-install.md) for more information\.
+ AWS CLI configured, see [Configuration basics](cli-configure-quickstart.md) for more information\. The profile that you use must have permissions that allow the AWS operations performed by the examples\.

## Creating and using DynamoDB tables<a name="cli-services-dynamodb-using"></a>

The command line format consists of an DynamoDB command name, followed by the parameters for that command\. The AWS CLI supports the CLI [shorthand syntax](cli-usage-shorthand.md) for the parameter values, and full JSON\.

FThe following example creates a table named `MusicCollection`\. 

```
$ aws dynamodb create-table \
    --table-name MusicCollection \
    --attribute-definitions AttributeName=Artist,AttributeType=S AttributeName=SongTitle,AttributeType=S \
    --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1
```

You can add new lines to the table with commands similar to those shown in the following example\. These examples use a combination of shorthand syntax and JSON\.

```
$ aws dynamodb put-item \
    --table-name MusicCollection \
    --item '{
        "Artist": {"S": "No One You Know"},
        "SongTitle": {"S": "Call Me Today"} ,
        "AlbumTitle": {"S": "Somewhat Famous"} 
      }' \
    --return-consumed-capacity TOTAL
{
    "ConsumedCapacity": {
        "CapacityUnits": 1.0,
        "TableName": "MusicCollection"
    }
}

$ aws dynamodb put-item \
    --table-name MusicCollection \
    --item '{ 
        "Artist": {"S": "Acme Band"}, 
        "SongTitle": {"S": "Happy Day"} , 
        "AlbumTitle": {"S": "Songs About Life"} 
      }' \
    --return-consumed-capacity TOTAL

{
    "ConsumedCapacity": {
        "CapacityUnits": 1.0,
        "TableName": "MusicCollection"
    }
}
```

It can be difficult to compose valid JSON in a single\-line command\. To make this easier, the AWS CLI can read JSON files\. For example, consider the following JSON snippet, which is stored in a file named `expression-attributes.json`\.

```
{
  ":v1": {"S": "No One You Know"},
  ":v2": {"S": "Call Me Today"}
}
```

You can use that file to issue a `query` request using the AWS CLI\. In the following example, the content of the `expression-attributes.json` file is used as the value for the `--expression-attribute-values` parameter\.

```
$ aws dynamodb query --table-name MusicCollection \
    --key-condition-expression "Artist = :v1 AND SongTitle = :v2" \
    --expression-attribute-values file://expression-attributes.json
{
    "Count": 1,
    "Items": [
        {
            "AlbumTitle": {
                "S": "Somewhat Famous"
            },
            "SongTitle": {
                "S": "Call Me Today"
            },
            "Artist": {
                "S": "No One You Know"
            }
        }
    ],
    "ScannedCount": 1,
    "ConsumedCapacity": null
}
```

## Using DynamoDB Local<a name="cli-services-dynamodb-local"></a>

In addition to DynamoDB, you can use the AWS CLI with DynamoDB Local\. DynamoDB Local is a small client\-side database and server that mimics the DynamoDB service\. DynamoDB Local enables you to write applications that use the DynamoDB API, without manipulating any tables or data in the DynamoDB web service\. Instead, all of the API actions are rerouted to a local database\. This lets you save on provisioned throughput, data storage, and data transfer fees\.

For more information about DynamoDB Local and how to use it with the AWS CLI, see the following sections of the [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/):
+ [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html)
+ [Using the AWS CLI with DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.CLI.html#UsingWithDDBLocal)

## Resources<a name="cli-services-dynamodb-resources"></a>

**AWS CLI reference:**
+ [https://docs.aws.amazon.com/cli/latest/reference/dynamodb/](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/)
+ [https://docs.aws.amazon.com/cli/latest/reference/dynamodb/create-table.html](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/create-table.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/dynamodb/put-item.html](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/put-item.html)
+ [https://docs.aws.amazon.com/cli/latest/reference/dynamodb/query.html](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/query.html)

**Service reference:**
+ [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html) in the Amazon DynamoDB Developer Guide
+ [Using the AWS CLI with DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.CLI.html#UsingWithDDBLocal) in the Amazon DynamoDB Developer Guide