# Monitoring Amazon Omics<a name="monitoring-overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon Omics and your other AWS solutions\. AWS provides the following monitoring tools to watch Omics, report when something is wrong, and take automatic actions when appropriate:
+ *Amazon CloudWatch* monitors your AWS resources and and the applications you run on AWS in real time\. You can collect and track metrics, create customized dashboards, and set alarms that notify you or take actions when a specified metric reaches a threshold that you specify\. For example, you can have CloudWatch track CPU usage or other metrics of your Amazon EC2 instances and automatically launch new instances when needed\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.
+ *Amazon CloudWatch Logs* enables you to monitor, store, and access your log files from Amazon EC2 instances, CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.
+ *AWS CloudTrail* captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

*Amazon EventBridge* is a serverless event bus service that makes it easy to connect your applications with data from a variety of sources\. EventBridge delivers a stream of real\-time data from your own applications, Software\-as\-a\-Service \(SaaS\) applications, and AWS services and routes that data to targets such as Lambda\. This enables you to monitor events that happen in services, and build event\-driven architectures\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.

## Viewing *Omics* metrics<a name="viewing-cloudwatch"></a>

CloudWatch metrics for Omics are viewable in the CloudWatch console\.

**To view metrics \(CloudWatch console\)**

1. Sign in to the AWS Management Console and open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home)\.

1. Choose **Metrics**, choose **All Metrics**, and then choose **AWS/Usage**\.

1. Filter **Service** for **Omics**\.

1. Choose the dimension, choose a metric name, then choose **Add to graph**\.

1. Choose a value for the date range\. The metric count for the selected date range is displayed in the graph\.

## Creating an alarm using CloudWatch<a name="alarms-cloudwatch"></a>

A CloudWatch alarm watches a single metric over a specified time period, and performs one or more actions: sending a notification to an Amazon Simple Notification Service \(Amazon SNS\) topic or Auto Scaling policy\. The action or actions are based on the value of the metric relative to a given threshold over a number of time periods that you specify\. CloudWatch can also send you an Amazon SNS message when the alarm changes state\.

CloudWatch alarms invoke actions only when the state changes and has persisted for the period you specify\.

**To view metrics \(CloudWatch console\)**

1. Sign in to the AWS Management Console and open the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home)\.

1. Choose **Alarms**, and then choose **Create Alarm**\.

1. Choose **AWS/Usage**, and then choose an Omics metric using the Service dimension\.

1. For **Time Range**, choose a time range to monitor, and then choose **Next**\.

1. Enter a **Name** and **Description**\.

1. For Whenever, choose **>=**, and type a maximum value\.

1. If you want CloudWatch to send an email when the alarm state is reached, in the Actions section, for Whenever this alarm, choose State is **ALARM**\. For Send notification to, choose a mailing list or choose **New list** and create a new mailing list\.

1. Preview the alarm in the Alarm Preview section\. If you are satisfied with the alarm, choose **Create Alarm**\.