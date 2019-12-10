# Using Amazon DynamoDB with the AWS CLI<a name="cli-services-dynamodb"></a>

The AWS Command Line Interface \(AWS CLI\) provides support for all of the AWS database services, including Amazon DynamoDB\. You can use the AWS CLI for ad hoc operations, such as creating a table\. You can also use it to embed DynamoDB operations within utility scripts\.

To list the AWS CLI commands for DynamoDB, use the following command\.

```
$ aws dynamodb help
```

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

The command line format consists of an DynamoDB command name, followed by the parameters for that command\. The AWS CLI supports the CLI [shorthand syntax](cli-usage-shorthand.md) for the parameter values, and full JSON\.

For example, the following command creates a table named `MusicCollection`\. 

**Note**  
For readability, long commands in this section are broken into separate lines\. The backslash \( \\ \) character is the line continuation character for the Linux command line, and lets you copy and paste \(or enter\) multiple lines at a Linux prompt\. If you're using a shell that doesn't use the backslash for line continuation, replace the backslash with that shell's line continuation character\. Or remove the backslashes and put the entire command on a single line\.

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

For more information about using the AWS CLI with DynamoDB, see [DynamoDB](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html) in the *AWS CLI Command Reference*\.

In addition to DynamoDB, you can use the AWS CLI with DynamoDB Local\. DynamoDB Local is a small client\-side database and server that mimics the DynamoDB service\. DynamoDB Local enables you to write applications that use the DynamoDB API, without manipulating any tables or data in the DynamoDB web service\. Instead, all of the API actions are rerouted to a local database\. This lets you save on provisioned throughput, data storage, and data transfer fees\.

For more information about DynamoDB Local and how to use it with the AWS CLI, see the following sections of the [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/):
+ [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html)
+ [Using the AWS CLI with DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.CLI.html#UsingWithDDBLocal)