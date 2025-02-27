# Working with Amazon SNS events and notifications in AWS Database Migration Service<a name="CHAP_Events"></a>

Beginning with the release of AWS DMS 3\.4\.6 and with later versions, we recommend that you use Amazon EventBridge to provide notifications when an AWS DMS event occurs\. For more information about using EventBridge events with AWS DMS, see [Working with Amazon EventBridge events and notifications in AWS Database Migration Service](CHAP_EventBridge.md)\.

## Moving event subscriptions to Amazon EventBridge<a name="USER_Events.Move-subscriptions"></a>

You can use the following AWS CLI command to migrate active event subscriptions from DMS to Amazon EventBridge, up to 10 at a time\.

`update-subscriptions-to-event-bridge [--force-move | --no-force-move]`

By default, AWS DMS only migrates active event subscriptions when your replication instance is current with AWS DMS 3\.4\.6 and higher\. To override this default behavior, use the `--force-move` option\. However, some types of events might not be available by using Amazon EventBridge if your replication instances aren't upgraded\.

To run the `update-subscriptions-to-event-bridge` CLI command, an AWS Identity and Access Management \(IAM\) user must have the following policy permissions\.

```
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "SNS:GetTopicAttributes",
              "SNS:SetTopicAttributes",
              "events:PutTargets",
              "events:EnableRule",
              "events:PutRule"
          ],
          "Resource": "*"
      }
  ]
}
```

For more information about moving subscriptions to EventBridge, see [UpdateSubscriptionsToEventBridge](https://docs.aws.amazon.com/dms/latest/APIReference/API_UpdateSubscriptionsToEventBridge.html) in the *AWS Database Migration Service API Reference*\.

## Working with Amazon SNS events and notifications<a name="USER_Events.SNS"></a>

AWS DMS versions 3\.4\.5 and earlier support working with events and notifications as described following\.

 AWS Database Migration Service \(AWS DMS\) can use Amazon Simple Notification Service \(Amazon SNS\) to provide notifications when an AWS DMS event occurs, for example the creation or deletion of a replication instance\. You can work with these notifications in any form supported by Amazon SNS for an AWS Region, such as an email message, a text message, or a call to an HTTP endpoint\. 

AWS DMS groups events into categories that you can subscribe to, so you can be notified when an event in that category occurs\. For example, if you subscribe to the Creation category for a given replication instance, you are notified whenever a creation\-related event occurs that affects your replication instance\. If you subscribe to a Configuration Change category for a replication instance, you are notified when the replication instance's configuration is changed\. You also receive notification when an event notification subscription changes\. For a list of the event categories provided by AWS DMS, see [AWS DMS event categories and event messages for SNS notifications](#USER_Events.Messages), following\.

AWS DMS sends event notifications to the addresses you provide when you create an event subscription\. You might want to create several different subscriptions, such as one subscription receiving all event notifications and another subscription that includes only critical events for your production DMS resources\. You can easily turn off notification without deleting a subscription by deselecting the **Enabled** option in the AWS DMS console, or by setting the `Enabled` parameter to *false* using the AWS DMS API\. 

**Note**  
AWS DMS event notifications using SMS text messages are currently available for AWS DMS resources in all AWS Regions where Amazon SNS is supported\. For a list of AWS Regions and countries where Amazon SNS supports SMS messaging, see [Supported Regions and countries](https://docs.aws.amazon.com/sns/latest/dg/sns-supported-regions-countries.html)\.   
For more information on using text messages with SNS, see [Sending and receiving SMS notifications using Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/SMSMessages.html)\.  
AWS DMS event notifications differ from CloudTrail events in CloudWatch or EventBridge\. CloudTrail event notifications can be generated by any API invocation\. DMS sends a notification only when a DMS event occurs\.

AWS DMS uses a subscription identifier to identify each subscription\. You can have multiple AWS DMS event subscriptions published to the same Amazon SNS topic\. When you use event notification, Amazon SNS fees apply; for more information on Amazon SNS billing, see [ Amazon SNS pricing](http://aws.amazon.com/sns/#pricing)\.

To subscribe to AWS DMS events with Amazon SNS, use the following process:

1. Create an Amazon SNS topic\. In the topic, you specify what type of notification you want to receive and to what address or number the notification will go to\.

1. Create an AWS DMS event notification subscription by using the AWS Management Console, AWS CLI, or AWS DMS API\.

1. AWS DMS sends an approval email or SMS message to the addresses you submitted with your subscription\. To confirm your subscription, click the link in the approval email or SMS message\.

1. When you have confirmed the subscription, the status of your subscription is updated in the AWS DMS console's **Event subscriptions** section\.

1. You then begin to receive event notifications\.

For the list of categories and events that you can be notified of, see the following section\. For more details about subscribing to and working with AWS DMS event subscriptions, see [Subscribing to AWS DMS event notification using SNS](#CHAP_Events.Subscription)\.

**Topics**

## AWS DMS event categories and event messages for SNS notifications<a name="USER_Events.Messages"></a>

**Important**  
Beginning with the release of AWS DMS 3\.4\.6 and with later versions, we recommend that you use Amazon EventBridge to provide notifications when an AWS DMS event occurs\. For more information about using EventBridge events with AWS DMS, see [Working with Amazon EventBridge events and notifications in AWS Database Migration Service](CHAP_EventBridge.md)\. 

 AWS DMS generates a significant number of events in categories that you can subscribe to using the AWS DMS console or the AWS DMS API\. Each category applies to a source type; currently AWS DMS supports the replication instance and replication task source types\. 

The following table shows the possible categories and events for the replication instance source type\.


|  Category  |  DMS event ID  |  Description  | 
| --- | --- | --- | 
|  Configuration Change  |  DMS\-EVENT\-0012  |  The replication instance class for this replication instance is being changed\.   | 
|  Configuration Change  |  DMS\-EVENT\-0014  |  The replication instance class for this replication instance has changed\.   | 
|  Configuration Change  |  DMS\-EVENT\-0018  |  The storage for the replication instance is being increased\.   | 
|  Configuration Change  |  DMS\-EVENT\-0017  |  The storage for the replication instance has been increased\.   | 
|  Configuration Change  |  DMS\-EVENT\-0024  |  The replication instance is transitioning to a Multi\-AZ configuration\.   | 
|  Configuration Change  |  DMS\-EVENT\-0025  |  The replication instance finished transitioning to a Multi\-AZ configuration\.   | 
|  Configuration Change  |  DMS\-EVENT\-0030  |  The replication instance is transitioning to a Single\-AZ configuration\.   | 
|  Configuration Change  |  DMS\-EVENT\-0029  |  The replication instance has finished transitioning to a Single\-AZ configuration\.   | 
|  Creation  |  DMS\-EVENT\-0067  |  A replication instance is being created\.   | 
|  Creation  |  DMS\-EVENT\-0005  |  A replication instance is created\.   | 
|  Deletion  |  DMS\-EVENT\-0066  |  The replication instance is being deleted\.   | 
|  Deletion  |  DMS\-EVENT\-0003  |  The replication instance is deleted\.   | 
|  Maintenance  |  DMS\-EVENT\-0047  | Management software on the replication instance has been updated\. | 
|  Maintenance  |  DMS\-EVENT\-0026  | Offline maintenance of the replication instance is taking place\. The replication instance is currently unavailable\.  | 
|  Maintenance  |  DMS\-EVENT\-0027  | Offline maintenance of the replication instance is complete\. The replication instance is now available\.  | 
|  Maintenance  |  DMS\-EVENT\-0068  | A replication instance is in a state that can't be upgraded\.  | 
|  LowStorage  |  DMS\-EVENT\-0007  | Free storage for the replication instance is low\.  | 
|  Failover  |  DMS\-EVENT\-0013  | Failover started for a Multi\-AZ replication instance\.  | 
|  Failover  |  DMS\-EVENT\-0049  | Failover is complete for a Multi\-AZ replication instance\. | 
|  Failover  |  DMS\-EVENT\-0015  | Multi\-AZ failover to standby is complete\. | 
|  Failover  |  DMS\-EVENT\-0050  | Multi\-AZ activation has started\.  | 
|  Failover  |  DMS\-EVENT\-0051  | Multi\-AZ activation had completed\.  | 
|  Failover  |  DMS\-EVENT\-0034  | If you request failover too frequently, this event occurs instead of regular failover events\. | 
|  Failure  |  DMS\-EVENT\-0031  | The replication instance has gone into storage failure\. | 
|  Failure  |  DMS\-EVENT\-0036  | The replication instance has failed due to an incompatible network\. | 
|  Failure  |  DMS\-EVENT\-0037  | The service can't access the AWS KMS key used to encrypt the data volume\. | 

The following table shows the possible categories and events for the replication task source type\.


|  Category  |  DMS event ID  |  Description  | 
| --- | --- | --- | 
|  State Change  |  DMS\-EVENT\-0069  |  The replication task has started\.   | 
|  State Change  |  DMS\-EVENT\-0081  |  A reload of table details has been requested\.   | 
|  State Change  |  DMS\-EVENT\-0079  |  The replication task has stopped\.   | 
|  State Change  | DMS\-EVENT\-0091  | Reading paused, swap files limit reached\. | 
|  State Change  | DMS\-EVENT\-0092  | Reading paused, disk usage limit reached\. | 
|  State Change  | DMS\-EVENT\-0093  | Reading resumed\. | 
|  Failure  |  DMS\-EVENT\-0078  |  The replication task has failed\.   | 
|  Failure  |  DMS\-EVENT\-0082  |  A call to delete the task has failed to clean up task data\.   | 
|  Configuration Change  |  DMS\-EVENT\-0080  | The replication task is modified\.  | 
|  Deletion  |  DMS\-EVENT\-0073  |  The replication task is deleted\.   | 
|  Creation  |  DMS\-EVENT\-0074  | The replication task is created\. | 

The following example shows an AWS DMS event subscription with the State Change category\.

```
            Resources: 
                DMSEvent: 
                    Type: AWS::DMS::EventSubscription 
                    Properties: 
                        Enabled: true 
                        EventCategories: State Change 
                        SnsTopicArn: arn:aws:sns:us-east-1:123456789:testSNS 
                        SourceIds: [] 
                        SourceType: replication-task
```

## Subscribing to AWS DMS event notification using SNS<a name="CHAP_Events.Subscription"></a>

**Important**  
Beginning with the release of AWS DMS 3\.4\.6 and with later versions, we recommend that you use Amazon EventBridge to provide notifications when an AWS DMS event occurs\. For more information about using EventBridge events with AWS DMS, see [Working with Amazon EventBridge events and notifications in AWS Database Migration Service](CHAP_EventBridge.md)\. 

You can create an AWS DMS event notification subscription so you can be notified when an AWS DMS event occurs\. The simplest way to create a subscription is with the AWS DMS console\. In a notification subscription, you choose how and where to send notifications\. You specify the type of source you want to be notified of; currently AWS DMS supports the replication instance and replication task source types\. And, depending on the source type you select, you choose the event categories and identify the source you want to receive event notifications for\. 

### Using the AWS Management Console<a name="USER_Events.Subscribing.Console"></a>

**Important**  
Beginning with the release of AWS DMS 3\.4\.6 and with later versions, we recommend that you use Amazon EventBridge to provide notifications when an AWS DMS event occurs\. For more information about using EventBridge events with AWS DMS, see [Working with Amazon EventBridge events and notifications in AWS Database Migration Service](CHAP_EventBridge.md)\. 

**To subscribe to AWS DMS event notification with Amazon SNS by using the console**

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\. 

   If you're signed in as an IAM user, make sure that you have the appropriate permissions to access AWS DMS\.

1.  In the navigation pane, choose **Event subscriptions**\. 

1.  On the **Event subscriptions** page, choose **Create event subscription**\. 

1.  On the **Create event subscription** page, do the following:

   1. Under **Details**, for **Name**, enter a name for the event notification subscription\.

   1. Choose **Enabled** to enable the subscription\. If you want to create the subscription but not have notifications sent yet, don't choose **Enabled**\.

   1. Under **Target**, choose either **Existing topics**, **Create new email topic** or **Create new SMS topic** to send notifications\. Make sure that you either have an existing Amazon SNS topic to send notices to or create the topic\. If you create a topic, you can enter an email address where notifications will be sent\.

   1. Under **Event source**, for **Source type**, choose a source type\. The only options are **replication\-instance** and **replication\-task**\.

   1. Depending on the source type you selected, choose the event categories and sources you want to receive event notifications for\.  
![\[Console create event subscription\]](http://docs.aws.amazon.com/dms/latest/userguide/images/datarep-create-event-sub-consolev2.png)

   1. Select **Create event subscription**\.

The AWS DMS console indicates that the subscription is being created\.

**Note**  
You can also create Amazon SNS event notification subscriptions using the AWS DMS API and CLI\. For more information, see the [CreateEventSubscription](https://docs.aws.amazon.com/dms/latest/APIReference/API_CreateEventSubscription.html) in the *AWS DMS API Reference* and [create\-event\-subscription](https://docs.aws.amazon.com/cli/latest/reference/dms/create-event-subscription.html) in the *AWS DMS CLI Reference* documentation\. 

### Validating the access policy of your SNS topic<a name="USER_Events.Subscribing.Access"></a>

Your SNS access policy requires permissions that allow AWS DMS to publish events to your SNS topic\. You can validate and update your access policy as described in the following procedures\.

**To validate your access policy**

1. Open the **Amazon SNS **console\.

1. From the navigation panel, choose **Topics** and select the topic that you want to receive DMS notifications about\.

1. Select the **Access policy** tab\.

You can update your policy if your SNS access policy doesn't allow AWS DMS to publish events to your SNS topic\.

**To update your access policy**

1. From the **Details** section of your topic page, choose **Edit**\.

1. Expand the **Access policy** section, and attach the following policy into the JSON editor\.

   ```
   {
         "Sid": "dms-allow-publish",
         "Effect": "Allow",
         "Principal": {
           "Service": "dms.amazonaws.com"
         },
         "Action": "sns:Publish",
         "Resource": "your-SNS-topic-ARN"
       }
   ```

   We recommend that you further restrict the access to your SNS topic by specifying the `aws:SourceArn` condition, which is the DMS EventSubscription Arn that publishes events to the topic\.

   ```
   ...
   "Resource": "your-SNS-topic-ARN"
   "Condition": {
       "StringEquals": {
          "aws:SourceArn": "arn:partition:dms:your-AWS-region:your-AWS-account-ID:es:your-dms-es-arn or *"
    }
   ```

1. Choose **Save changes**\.