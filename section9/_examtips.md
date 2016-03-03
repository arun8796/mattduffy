=== Simple Notification Service (SNS)
Fully managed Pub-Sub PUSH notification service

Can push messages out as mobile SMS text messages, to SQS queues, or to any HTTP API endpoint

All SNS messages are stored redundantly across multiple availablitity zones

SNS Topics are "access points" for allowing recpients to dynamially subscribe to for identical copies of the same notification
One topic can support mutliple endpoint types (iOS, Android, SMS, etc.)
Instantaneous push delivery, pay as you go

SNS vs SQS
push vs pull
$0.50 per 1mm SNS requests
$0.06 per 100,000 Notification deliveries over HTTP
$0.75 per 100 Notification deliveries over SMS
$2.00 per 100,000 Notification deliveries over email

SNS Data type = JSON (not XML)
subscribers have to confirm their subscription to an SNS topic before they start to receive notification deliveries
{
	type: notification,
	messageid: lskjlfjajf,
	topicArn: arn:aws:sns:us-west-1:1234567890:MyTestSNSTopic,
	subject: this is a test tickle,
	message: tickle test,
	timestamp: 39393392,
	signatureVersion: 1,
	signature: lkasjdoiu329sjdfoajsdjsaldfj,
	signingCertUrl: https://sns.us-west-1.amazonaws.com/SimpleNotificationService-lkajsldfj.pem,
	unsubscribeURL: https://sns.us-west-1.amazonaws.com/?action=unsubscribe&subscriptionArn=arn:aws:sns:us-west-1:1234567890:MyTestSNSTopic:8,
	messageAttributes: {
		x
		y
		z
	}
}


