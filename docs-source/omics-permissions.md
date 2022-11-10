# Amazon Omics permissions<a name="omics-permissions"></a>

You can use AWS Identity and Access Management \(IAM\) to manage access to the Amazon Omics API and resources\. For users and applications in your account that use Amazon Omics, you manage permissions in a permissions policy that you can apply to IAM users, groups, or roles\.

To manage permissions for users and applications in your accounts, [use the policies that Amazon Omics provides](permissions-user.md), or write your own\. The Amazon Omics console uses multiple services to get information about your function's configuration and triggers\. You can use the provided policies as\-is, or as a starting point for more restrictive policies\.

Amazon Omics uses IAM [service roles](permissions-service.md) to access other services on your behalf\. For example, you would create or choose a service role when you run a workflow that reads data from Amazon S3\. For some features, you also need to [configure permissions on resources in other services](permissions-resource.md)\. Review these requirements before you start working with Amazon Omics

For more information about IAM, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.

**Topics**
+ [Identity\-based IAM policies for Amazon Omics](permissions-user.md)
+ [Service roles for Amazon Omics](permissions-service.md)
+ [Resource permissions](permissions-resource.md)