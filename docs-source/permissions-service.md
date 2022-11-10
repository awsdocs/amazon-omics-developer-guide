# Service roles for Amazon Omics<a name="permissions-service"></a>

You can use service roles to grant Amazon Omics permission to access data and upload logs while processing a workflow or importing data to a Omics Storage or Omics Analytics data store\. A service role is an AWS Identity and Access Management \(IAM\) role that an AWS service can use to access resources from other services in your account\. You pass a service role to Amazon Omics when you start a job\.

Service roles must have the following trust policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "omics.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

The trust policy allows Amazon Omics to assume the role\.

**Topics**
+ [Sample IAM policies](#permissions-service-samplepolicies)
+ [Sample Cloudwatch templates](#permissions-service-sampletemplates)

## Sample IAM policies<a name="permissions-service-samplepolicies"></a>

The GitHub repository for this guide provides sample IAM policies that you can use as reference for developing service roles\. You can use a single role that grants permission for both importing data and sending alerts by combining the applicable policies\.

**Example Service role**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::my-bucket/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "omics:*"
            ],
            "Resource": [
                "arn:aws:omics:us-west-2:123456789012:referenceStore/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::my-bucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogStreams",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-west-2:123456789012:loggroup:/aws/omics/WorkflowLog:log-stream:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup"
            ],
            "Resource": [
                "arn:aws:logs:us-west-2:123456789012:loggroup:/aws/omics/WorkflowLog:*"
            ]
        }
    ]
}
```

## Sample Cloudwatch templates<a name="permissions-service-sampletemplates"></a>

The following sample template creates a service role that gives Amazon Omics permission to access S3 buckets that have names prefixed with `omics-`, and to upload workflow logs\.

**Example Reference store, Amazon S3 and CloudWatch Logs permissions**  

```
Parameters:
  bucketName:
    Description: Bucket name
    Type: String
    
Resources:
  serviceRole:
    Type: AWS::IAM::Role
    Properties:
      Policies:
        - PolicyName: read-reference
          PolicyDocument:
            Version: 2012-10-17
            Statement:
            - Effect: Allow
              Action:
                - omics:*
              Resource: !Sub arn:${AWS::Partition}:omics:${AWS::Region}:${AWS::AccountId}:referenceStore/*
        - PolicyName: read-s3
          PolicyDocument:
            Version: 2012-10-17
            Statement:
            - Effect: Allow
              Action: 
                - s3:ListBucket
              Resource: !Sub arn:${AWS::Partition}:s3:::${bucketName}
            - Effect: Allow
              Action:
                - s3:GetObject
                - s3:PutObject
              Resource: !Sub arn:${AWS::Partition}:s3:::${bucketName}/*
        - PolicyName: upload-logs
          PolicyDocument:
            Version: 2012-10-17
            Statement:
            - Effect: Allow
              Action: 
                - logs:DescribeLogStreams
                - logs:CreateLogStream
                - logs:PutLogEvents
              Resource: !Sub arn:${AWS::Partition}:logs:${AWS::Region}:${AWS::AccountId}:loggroup:/aws/omics/WorkflowLog:log-stream:*
            - Effect: Allow
              Action: 
                - logs:CreateLogGroup
              Resource: !Sub arn:${AWS::Partition}:logs:${AWS::Region}:${AWS::AccountId}:loggroup:/aws/omics/WorkflowLog:*
      AssumeRolePolicyDocument: |
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Action": [
                "sts:AssumeRole"
              ],
              "Effect": "Allow",
              "Principal": {
                "Service": [
                  "omics.amazonaws.com"
                ]
              }
            }
          ]
        }
```