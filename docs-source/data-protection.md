# Data protection in Amazon Omics<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in Amazon Omics\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with Amazon Omics or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.



## Encryption at rest<a name="encryption-rest"></a>

Amazon Omics provides encryption by default to protect sensitive customer data at rest by using a service owned AWS Key Management Service \(AWS KMS\) key\. Customer\-managed KMS keys are also supported\. To learn more about Customer\-managed KMS Key, see [Amazon Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)\.

All Omics data stores \(Storage and Analytics\) support the use of Customer\-managed KMS keys\. The encryption configuration cannot be changed after a data store has been created\. If a data store is using an AWS owned KMS Key, it will be denoted as AWS\_OWNED\_KMS\_KEY and you will not see the specific key used for encryption at rest\.

For Omics Workflows, customer\-managed keys are not supported by the temporary file system; however, all data is encrypted at rest automatically using XTS\-AES\-256 block cipher encryption algorithm to encrypt the file system\. The IAM user and role used to start a workflow run must also have access to the AWS KMS keys used for workflow input and output buckets\. Workflows does not use grants, and AWS KMS encryption is limited to input and output Amazon S3 buckets\. The IAM role used both for workflow APIs must also have access to the AWS KMS keys used as well as the input and output Amazon S3 buckets\. You can use either IAM roles and permissions to control access or AWS KMS policies\. To learn more, see [Authentication and access control for AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html)\. 

 Additionally, when using AWS Lake Formation with Omics Analytics, any decrypt permissions associated with the Lake Formation are also given to the input and output Amazon S3 buckets\. More information about how AWS Lake Formation manages permissions can be found in the [AWS Lake Formation documentation](https://docs.aws.amazon.com/lake-formation/latest/dg/register-encrypted.html)\. 

Omics Analytics grants Lake Formation kms:Decrypt permissions to read the encrypted data in an S3 bucket\. As long as you have permissions to query the data through Lake Formation, you will be able to read the encrypted data\. Access to the data is controlled through data access control in Lake Formation, not through a KMS key policy\. To learn more, see the [AWS Integrated AWS service requests](https://docs.aws.amazon.com/lake-formation/latest/dg/access-control-underlying-data.html) in the Lake Formation documentation\. 



### AWS owned KMS key<a name="AWS-owned-cmk"></a>

Amazon Omics uses these keys by default to automatically encrypt potentially sensitive information such as personally identifiable or Protected Health Information \(PHI\) data at rest\. AWS owned KMS keys aren't stored in your account\. They're part of a collection of KMS keys that AWS owns and manages for use in multiple AWS accounts\.

AWS services can use AWS owned KMS keys to protect your data\. You can't view, manage, use AWS owned KMS keys, or audit their use\. However, you don't need to do any work or change any programs to protect the keys that encrypt your data\.

You're not charged a monthly fee or a usage fee if you use AWS owned KMS keys, and they don't count against AWS KMS quotas for your account\. For more information, see [AWS owned keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk)\.

### Customer managed KMS keys<a name="customer-owned-cmk"></a>

Amazon Omics supports the use of a symmetric customer managed KMS key that you create, own, and manage to add a second layer of encryption over the existing AWS owned encryption\. Because you have full control of this layer of encryption, you can perform such tasks as:
+ Establishing and maintaining key policies, IAM policies, and grants
+ Rotating key cryptographic material
+ Enabling and disabling key policies
+ Adding tags
+ Creating key aliases
+ Scheduling keys for deletion

 You can also use CloudTrail to track the requests that Amazon Omics sends to AWS KMS on your behalf\. Additional AWS KMS charges apply\. For more information, see [customer owned keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk)\.

### Create a customer managed key<a name="creating-co-cmk"></a>

You can create a symmetric customer managed key by using the AWS Management Console, or the AWS KMS APIs\.

Follow the steps for [Creating symmetric customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-symmetric-cmk) in the AWS Key Management Service Developer Guide\.

Key policies control access to your customer managed key\. Every customer managed key must have exactly one key policy, which contains statements that determine who can use the key and how they can use it\. When you create your customer managed key, you can specify a key policy\. For more information, see [Managing access to customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the AWS Key Management Service Developer Guide\.

To use your customer managed key with your Amazon Omics resources, [kms:CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) operations must be permitted in the key policy\. This adds a grant to a customer managed key that controls access to a specified KMS key\. This key gives a user access to the [kms:grant operations](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html#terms-grant-operations ) that Amazon Omics requires\. See [Using grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) for more information\.

To use your customer managed KMS key with your Amazon Omics resources, the following API operations must be permitted in the key policy:
+ kms:CreateGrant adds grants to a specific customer managed KMS key, which allows access to grant operations in Omics Analytics and Omics Storage\. Omics Workflows does not use grants\.
+ kms:DescribeKey provides the customer managed key details needed to validate the key\. This is required for all operations\.
+ kms:GenerateDataKey provides access to encrypt resources at rest for all write operations\.
+ kms:Decrypt provides access to read or search operations for encrypted resources\.

The following is a policy statement example that allows a role to create and interact with a data store in Amazon Omics which is encrypted by that key:

```
{    
    "Statement": [
        {
            "Sid": "Allow access XXX in Amazon Omics",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:omicsFullAccessRole"
            },
            "Action": [
                "kms:DescribeKey",
                "kms:CreateGrant",
                "kms:GenerateDataKey",
                "kms:Decrypt"
            ],
           "Resource": "*",
           "Condition": {
                "StringEquals": {
                    "kms:ViaService": "omics.amazonaws.com",
                    "kms:CallerAccount": "111122223333"
               }
            }
        }
    ]
}
```

The following policy would create permissions for a data store to decrypt data from an Amazon S3 bucket\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "omics:GetReference", 
                "omics:GetReferenceMetadata"
            ],
            "Resource": [
                "arn:aws:omics:{{region}}:{{accountId}}:referenceStore/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::[[s3path]]/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": [
                "arn:aws:kms:{{region}}:{{account_id}}:key/{{key_id}}"
            ]
             "Condition": {
                "StringEquals": {
                    "kms:ViaService": [
                      "s3.{{region}}.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

### Required IAM permissions for using a customer managed KMS key<a name="required-iam-cmk"></a>

 When creating a resource such as a data store with AWS KMS encryption using a customer managed KMS key, there are required permissions for both the key policy and the IAM policy for the user or role\.

You can use the [kms:ViaService condition key](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-via-service) to limit use of the KMS key to only requests that originate from Amazon Omics\.

 For more information about key policies, see [Enabling IAM policies](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-root-enable-iam) in the AWS Key Management Service Developer Guide\. 

The IAM user, IAM role, or AWS account creating your repositories must have the kms:CreateGrant, kms:GenerateDataKey, and kms:DescribeKey permissions plus the necessary Amazon Omics permissions\.



#### How Amazon Omics uses grants in AWS KMS<a name="grants-kms"></a>

Omics Analytics requires a [grant](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) to use your customer managed KMS key\. Grants are not required or used for either Omics Workflows or Omics Storage\. When you create a data store encrypted with a customer managed KMS key, Amazon Omics creates a grant on your behalf by sending a [CreateGrant](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateGrant.html) request to AWS KMS\. Grants in AWS KMS are used to give Amazon Omics access to a KMS key in a customer account\.

It is not recommended to revoke or retire the grants that Amazon Omics creates on your behalf\. If you revoke or retire the grant that gives Amazon Omics permission to use the AWS KMS keys in your account, Amazon Omics cannot access this data, encrypt new resources pushed to the data store, or decrypt them when they are pulled\. When you revoke or retire a grant for Amazon Omics, the change occurs immediately\. To revoke access rights, you should delete the data store rather than revoking the grant\. When a data store is deleted, Amazon Omics retires the grants on your behalf\.

#### Monitoring your encryption keys for Amazon Omics<a name="monitoring-kms"></a>

You can use CloudTrail to track the requests that Amazon Omics sends to AWS KMS on your behalf when using a customer managed KMS key\. The log entries in the CloudTrail log show Amazon Omics\.amazonaws\.com in the userAgent field to clearly distinguish requests made by Amazon Omics\.

The following examples are CloudTrail events for CreateGrant, GenerateDataKey, Decrypt, and DescribeKey to monitor AWS KMS operations called by Amazon Omics to access data encrypted by your customer managed key\.

The following also shows how to use CreateGrant to allow Amazon Omics to access a customer provided KMS key, enabling Amazon Omics to use that KMS key to encrypt all customer data at rest\.

You are not required to create your own grants\. Amazon Omics creates a grant on your behalf by sending a CreateGrant request to AWS KMS\. Grants in AWS KMS are used to give Amazon Omics access to a AWS KMS key in a customer account\.

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "xx:test",
        "arn": "arn:aws:sts::555555555555:assumed-role/user-admin/test",
        "accountId": "xx",
        "accessKeyId": "xxx",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "xxxx",
                "arn": "arn:aws:iam::555555555555:role/user-admin",
                "accountId": "555555555555",
                "userName": "user-admin"
            },
            "webIdFederationData": {},
            "attributes": {
                "creationDate": "2022-11-11T01:36:17Z",
                "mfaAuthenticated": "false"
            }
        },
        "invokedBy": "apigateway.amazonaws.com"
    },
    "eventTime": "2022-11-11T02:34:41Z",
    "eventSource": "kms.amazonaws.com",
    "eventName": "CreateGrant",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "apigateway.amazonaws.com",
    "userAgent": "apigateway.amazonaws.com",
    "requestParameters": {
        "granteePrincipal": "AWS Internal",
        "keyId": "arn:aws:kms:us-west-2:555555555555:key/a6e87d77-cc3e-4a98-a354-e4c275d775ef",
        "operations": [
            "CreateGrant",
            "RetireGrant",
            "Decrypt",
            "GenerateDataKey"
        ]
    },
    "responseElements": {
        "grantId": "4869b81e0e1db234342842af9f5531d692a76edaff03e94f4645d493f4620ed7",
        "keyId": "arn:aws:kms:us-west-2:245126421963:key/xx-cc3e-4a98-a354-e4c275d775ef"
    },
    "requestID": "d31d23d6-b6ce-41b3-bbca-6e0757f7c59a",
    "eventID": "3a746636-20ef-426b-861f-e77efc56e23c",
    "readOnly": false,
    "resources": [
        {
            "accountId": "245126421963",
            "type": "AWS::KMS::Key",
            "ARN": "arn:aws:kms:us-west-2:245126421963:key/xx-cc3e-4a98-a354-e4c275d775ef"
        }
    ],
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "245126421963",
    "eventCategory": "Management"
}
```

The following example shows how to use GenerateDataKey to ensure the user has the necessary permissions to encrypt data before storing it\.

```
             {
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "EXAMPLEUSER",
        "arn": "arn:aws:sts::111122223333:assumed-role/Sampleuser01",
        "accountId": "111122223333",
        "accessKeyId": "EXAMPLEKEYID",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "EXAMPLEROLE",
                "arn": "arn:aws:iam::111122223333:role/Sampleuser01",
                "accountId": "111122223333",
                "userName": "Sampleuser01"
            },
            "webIdFederationData": {},
            "attributes": {
                "creationDate": "2021-06-30T21:17:06Z",
                "mfaAuthenticated": "false"
            }
        },
        "invokedBy": "omics.amazonaws.com"
    },
    "eventTime": "2021-06-30T21:17:37Z",
    "eventSource": "kms.amazonaws.com",
    "eventName": "GenerateDataKey",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "omics.amazonaws.com",
    "userAgent": "omics.amazonaws.com",
    "requestParameters": {
        "keySpec": "AES_256",
        "keyId": "arn:aws:kms:us-east-1:111122223333:key/EXAMPLE_KEY_ARN"
    },
    "responseElements": null,
    "requestID": "EXAMPLE_ID_01",
    "eventID": "EXAMPLE_ID_02",
    "readOnly": true,
    "resources": [
        {
            "accountId": "111122223333",
            "type": "AWS::KMS::Key",
            "ARN": "arn:aws:kms:us-east-1:111122223333:key/EXAMPLE_KEY_ARN"
        }
    ],
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "111122223333",
    "eventCategory": "Management"
}
```

#### Learn more<a name="more-info-kms"></a>

The following resources provide more information about data at rest encryption\.

For more information about [AWS Key Management Service basic concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html), see the AWS KMS documentation\.

For more information about [Security best practices](https://docs.aws.amazon.com/kms/latest/developerguide/best-practices.html) in the AWS KMS documentation\.

### Encryption in transit<a name="encryption-transit"></a>

Amazon Omics uses TLS 1\.2\+ to encrypt data in transit through the public endpoints and through backend services\.