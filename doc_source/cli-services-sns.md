# Using Amazon SNS with the AWS CLI<a name="cli-services-sns"></a>

You can access the features of Amazon Simple Notification Service \(Amazon SNS\) using the AWS Command Line Interface \(AWS CLI\)\. To list the AWS CLI commands for Amazon SNS, use the following command\.

```
aws sns help
```

Before you run any commands, set your default credentials\. For more information, see [Configuring the AWS CLI](cli-chap-configure.md)\.

This topic shows examples of CLI commands that perform common tasks for Amazon SNS\.

**Topics**
+ [Create a topic](#cli-create-sns-topic)
+ [Subscribe to a topic](#cli-subscribe-sns-topic)
+ [Publish to a topic](#cli-publish-sns-topic)
+ [Unsubscribe from a topic](#cli-unsubscribe-sns-topic)
+ [Delete a topic](#cli-delete-sns-topic)

## Create a topic<a name="cli-create-sns-topic"></a>

To create a topic, use the [https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html](https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html) command and specify the name to assign to the topic\.

```
$ aws sns create-topic --name my-topic
{
    "TopicArn": "arn:aws:sns:us-west-2:123456789012:my-topic"
}
```

Make a note of the response's `TopicArn`, which you use later to publish a message\.

## Subscribe to a topic<a name="cli-subscribe-sns-topic"></a>

To subscribe to a topic, use the [https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html](https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html) command\. 

The following example specifies the `email` protocol and an email address for the `notification-endpoint`\.

```
$ aws sns subscribe --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic --protocol email --notification-endpoint saanvi@example.com
{
    "SubscriptionArn": "pending confirmation"
}
```

AWS immediately sends a confirmation message by email to the address you specified in the `subscribe` command\. The email message has the following text\.

```
You have chosen to subscribe to the topic:
arn:aws:sns:us-west-2:123456789012:my-topic
To confirm this subscription, click or visit the following link (If this was in error no action is necessary):
Confirm subscription
```

After the recipient clicks the **Confirm subscription** link, the recipient's browser displays a notification message with information similar to the following\.

```
Subscription confirmed!

You have subscribed saanvi@example.com to the topic:my-topic.

Your subscription's id is:
arn:aws:sns:us-west-2:123456789012:my-topic:1328f057-de93-4c15-512e-8bb22EXAMPLE

If it was not your intention to subscribe, click here to unsubscribe.
```

## Publish to a topic<a name="cli-publish-sns-topic"></a>

To send a message to all subscribers of a topic, use the [publish](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html) command\. 

The following example sends the message "Hello World\!" to all subscribers of the specified topic\.

```
$ aws sns publish --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic --message "Hello World!"
{
    "MessageId": "4e41661d-5eec-5ddf-8dab-2c867EXAMPLE"
}
```

In this example, AWS sends an email message with the text "Hello World\!" to `saanvi@example.com`\.

## Unsubscribe from a topic<a name="cli-unsubscribe-sns-topic"></a>

To unsubscribe from a topic and stop receiving messages published to that topic, use the [unsubscribe](https://docs.aws.amazon.com/cli/latest/reference/sns/unsubscribe.html) command and specify the ARN of the topic you want to unsubscribe from\.

```
$ aws sns unsubscribe --subscription-arn arn:aws:sns:us-west-2:123456789012:my-topic:1328f057-de93-4c15-512e-8bb22EXAMPLE
```

To verify that you successfully unsubscribed, use the [list\-subscriptions](https://docs.aws.amazon.com/cli/latest/reference/sns/list-subscriptions.html) command to confirm that the ARN no longer appears in the list\.

```
$ aws sns list-subscriptions
```

## Delete a topic<a name="cli-delete-sns-topic"></a>

To delete a topic, run the [delete\-topic](https://docs.aws.amazon.com/cli/latest/reference/sns/delete-topic.html) command\.

```
$ aws sns delete-topic --topic-arn arn:aws:sns:us-west-2:123456789012:my-topic
```

To verify that AWS successfully deleted the topic, use the [list\-topics](https://docs.aws.amazon.com/cli/latest/reference/sns/list-topics.html) command to confirm that the topic no longer appears in the list\.

```
$ aws sns list-topics
```