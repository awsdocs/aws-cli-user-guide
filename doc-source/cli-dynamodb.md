# Using Amazon DynamoDB with the AWS Command Line Interface<a name="cli-dynamodb"></a>

The AWS Command Line Interface \(AWS CLI\) provides support for Amazon DynamoDB\. You can use the AWS CLI for ad hoc operations, such as creating a table\. You can also use it to embed DynamoDB operations within utility scripts\.

The command line format consists of an Amazon DynamoDB API name, followed by the parameters for that API\. The AWS CLI supports a shorthand syntax for the parameter values, as well as JSON\.

For example, the following command will create a table named `MusicCollection`\. 

**Note**  
For readability, long commands in this section are broken into separate lines\. The backslash character lets you copy and paste \(or type\) multiple lines into a Linux terminal\. If you are using a shell that does not use backslash to escape characters, replace the backslash with another escape character, or remove the backslashes and put the entire command on a single line\.

```
$ aws dynamodb create-table \
    --table-name MusicCollection \
    --attribute-definitions \
        AttributeName=Artist,AttributeType=S AttributeName=SongTitle,AttributeType=S \
    --key-schema AttributeName=Artist,KeyType=HASH AttributeName=SongTitle,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1
```

The following commands will add new items to the table\. These example use a combination of shorthand syntax and JSON\.

```
$ aws dynamodb put-item \
    --table-name MusicCollection \
    --item '{
        "Artist": {"S": "No One You Know"},
        "SongTitle": {"S": "Call Me Today"} ,
        "AlbumTitle": {"S": "Somewhat Famous"} }' \
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
        "AlbumTitle": {"S": "Songs About Life"} }' \
    --return-consumed-capacity TOTAL
{
    "ConsumedCapacity": {
        "CapacityUnits": 1.0,
        "TableName": "MusicCollection"
    }
}
```

On the command line, it can be difficult to compose valid JSON; however, the AWS CLI can read JSON files\. For example, consider the following JSON snippet, which is stored in a file named `expression-attributes.json`:

**Example expression\-attributes\.json**  

```
{
  ":v1": {"S": "No One You Know"},
  ":v2": {"S": "Call Me Today"}
}
```

You can now issue a `Query` request using the AWS CLI\. In this example, the contents of the `expression-attributes.json` file are used for the `--expression-attribute-values` parameter:

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

For more documentation on using the AWS CLI with DynamoDB, go to [http://docs\.aws\.amazon\.com/cli/latest/reference/dynamodb/index\.html](http://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html)\.

In addition to DynamoDB, you can use the AWS CLI with DynamoDB Local\. DynamoDB Local is a small client\-side database and server that mimics the DynamoDB service\. DynamoDB Local enables you to write applications that use the DynamoDB API, without actually manipulating any tables or data in DynamoDB\. Instead, all of the API actions are rerouted to DynamoDB Local\. When your application creates a table or modifies data, those changes are written to a local database\. This lets you save on provisioned throughput, data storage, and data transfer fees\.

For more information about DynamoDB Local and how to use it with the AWS CLI, see the following sections of the [Amazon DynamoDB Developer Guide](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/):

+ [DynamoDB Local](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.DynamoDBLocal.html)

+ [Using the AWS CLI with DynamoDB Local](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Tools.CLI.html#UsingWithDDBLocal)