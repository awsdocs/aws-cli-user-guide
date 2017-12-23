# Using the AWS Command Line Interface with Amazon SNS<a name="cli-sqs-queue-sns-topic"></a>

 This section describes some common tasks related to Amazon Simple Notification Service \(Amazon SNS\) and how to perform them using the AWS Command Line Interface\. 


+ [Create a Topic](#cli-create-sns-topic)
+ [Subscribe to a Topic](#cli-subscribe-sns-topic)
+ [Publish to a Topic](#cli-publish-sns-topic)
+ [Unsubscribe from a Topic](#cli-unsubscribe-sns-topic)
+ [Delete a Topic](#cli-delete-sns-topic)

## Create a Topic<a name="cli-create-sns-topic"></a>

The following command creates a topic named **my\-topic**:

```
$ aws sns create-topic --name my-topic
{
    "TopicArn": "arn:aws:sns:us-west-2:123456789012:my-topic"
}
```

Make a note of the `TopicArn`, which you will use later to publish a message\.

## Subscribe to a Topic<a name="cli-subscribe-sns-topic"></a>

The following command subscribes to a topic using the email protocol and an email address for the notification endpoint:

```
$ aws sns subscribe --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic --protocol email --notification-endpoint emailusername@example.com
{
    "SubscriptionArn": "pending confirmation"
}
```

An email message will be sent to the email address listed in the subscribe command\. The email message will have the following text:

```
You have chosen to subscribe to the topic:
arn:aws:sns:us-west-2:123456789012:my-topic
To confirm this subscription, click or visit the following link (If this was in error no action is necessary):
Confirm subscription
```

After clicking **Confirm subscription**, a "Subscription confirmed\!" notification message should appear in your browser with information similar to the following:

```
Subscription confirmed!

You have subscribed emailusername@example.com to the topic:my-topic.

Your subscription's id is:
arn:aws:sns:us-west-2:123456789012:my-topic:1328f057-de93-4c15-512e-8bb2268db8c4

If it was not your intention to subscribe, click here to unsubscribe.
```

## Publish to a Topic<a name="cli-publish-sns-topic"></a>

The following command publishes a message to a topic:

```
$ aws sns publish --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic --message "Hello World!"
{
    "MessageId": "4e41661d-5eec-5ddf-8dab-2c867a709bab"
}
```

An email message with the text "Hello World\!" will be sent to emailusername@example\.com

## Unsubscribe from a Topic<a name="cli-unsubscribe-sns-topic"></a>

The following command unsubscribes from a topic:

```
$ aws sns unsubscribe --subscription-arn arn:aws:sns:us-west-2:123456789012:my-topic:1328f057-de93-4c15-512e-8bb2268db8c4
```

To verify the unsubscription to the topic, type the following:

```
$ aws sns list-subscriptions
```

## Delete a Topic<a name="cli-delete-sns-topic"></a>

The following command deletes a topic:

```
$ aws sns delete-topic --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic
```

To verify the deletion of the topic, type the following:

```
$ aws sns list-topics
```