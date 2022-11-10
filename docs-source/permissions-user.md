# Identity\-based IAM policies for Amazon Omics<a name="permissions-user"></a>

To grant users in your account access to Amazon Omics, you use identity\-based policies in AWS Identity and Access Management \(IAM\)\. Identity\-based policies can apply directly to IAM users, or to IAM groups and roles that are associated with a user\. You can also grant users in another account permission to assume a role in your account and access your Amazon Omics resources\.

The following IAM policy allows a user to access all Amazon Omics API actions, and to pass [service roles](permissions-service.md) to Amazon Omics\.

**Example User policy**  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "omics:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:PassRole"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "iam:PassedToService": "omics.amazonaws.com"
        }
      }
    }
  ]
}
```

When you use Amazon Omics, you also interact with other AWS services\. To access these services, use the managed policies provided by each service\. To limit access to a subset of resources, you can use the managed policies as a starting point to create your own more restrictive policies\.

****
+ [AmazonS3FullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonS3FullAccess) – Access to Amazon S3 buckets and objects used by jobs\.

  
+ [AmazonEC2ContainerRegistryFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess) – Access to Amazon ECR registries and repositories for workflow container images\.

  
+ [AWSLakeFormationDataAdmin](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSLakeFormationDataAdmin) – Access to Lake Formation databases and tables created by analytics stores\.

  
+ [ResourceGroupsandTagEditorFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/ResourceGroupsandTagEditorFullAccess) – Tag Amazon Omics resources with Amazon Omics tagging API operations\.

  

The preceding policies do not allow a user to create IAM roles\. For a user with these permissions to run a job, an administrator must create the service role that grants Amazon Omics permission to access data sources\. For more information, see [Service roles for Amazon Omics](permissions-service.md)\.