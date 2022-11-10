# AWS managed policies for Amazon Omics<a name="security-iam-awsmanpol"></a>





To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ViewOnlyAccess` AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AmazonOmicsReadOnlyAccess<a name="security-iam-awsmanpol-AmazonOmicsReadOnlyAccess"></a>





You can attach the `AWSOmicsReadOnlyAccess` policy to your IAM identities when you wish to limit the permissions for that identity to read\-only access\.



```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"omics:Get*"
				"omics:List*"
			],
			"Resource": "*"
		}		
	]
}
```





## Amazon Omics updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon Omics since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon Omics Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  Amazon Omics started tracking changes  |  Amazon Omics started tracking changes for its AWS managed policies\.  | November 29, 2022 | 
|  AmazonOmicsReadOnlyAccess \- New policy added  |  Amazon Omics added a new policy that limits access to read only\. To learn more, [AmazonOmicsReadOnlyAccess](#security-iam-awsmanpol-AmazonOmicsReadOnlyAccess)\.  | November 29, 2022 | 