# Creating and managing variant stores<a name="creating-variant-stores"></a>

The following examples show how you can use the APIs to create and manage variant stores\. You can also perform these operations with the AWS CLI\.

In the following example, the AWS CLI is used to create a variant store\.

```
aws omics create-variant-store --name "store_a" \
  --reference referenceArn="arn:aws:omics:us-west-2:555555555555:referenceStore/6505293348/reference/5987565360"
```

To confirm the creation of your variant store, you will receive the following response\.

```
{
    "creationTime": "2022-08-24T20:34:19.229500Z",
    "id": "3b93cdef69d2",
    "name": "store_a",
    "reference": {
        "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/6505293348/reference/5987565360"
    },
    "status": "CREATING"
}
```

To learn more about a variant store, use the **get\-variant store** API\.

```
aws omics get-variant-store --name (store_name)  
```

You will receive the following response\.

```
{
    "id": "438390982973",
    "referenceName": "(reference_name)",
    "status": "READY",
    "name": "(store_name)",
    "creationTime": "2022-07-13T23:41:14.389670Z",
    "updateTime": "2022-07-13T23:45:33.594306Z"
}
```

To view all variant stores associated with an account, use the **list\-variant\-stores** API\.

```
aws omics list-variant-stores  
```

You will receive a response that lists all variant stores, along with their IDs, statuses, and other details, as shown in the following example response\.

```
{
    "variantStores": [
        {
            "id": "45aeb91d5678",
            "reference": {
                "referenceArn": "arn:aws:omics:us-west-2:55555555555:referenceStore/5506874698"
            },
            "status": "READY",
            "storeArn": "arn:aws:omics:us-west-2:55555555555:variantStore/icebergtesting123",
            "name": "variantstore",
            "creationTime": "2022-11-03T18:19:52.296368+00:00",
            "updateTime": "2022-11-03T18:30:56.272792+00:00",
            "statusMessage": "",
            "storeSizeBytes": 141526
        }
    ]
}
```

You can also filter responses based on statuses or other criteria\.

The following example shows how to use the AWS CLI to create an import job\.

```
aws omics start-variant-import-job \
  --destination-name store_a \
  --runLeftNormalization false \
  --role-arn  arn:aws:iam::55555555555:role/roleName \
  --items source=s3://pathToS3/example.vcf.gz
```

You will receive the following response to let you know your import job has started\.

```
{
  "destinationName": "store_a",
  "roleArn": "....", 
  "runLeftNormalization": false, 
  "items": [
    {"source": "s3://pathToS3/sample.vcf.gz" } 
  ]
}
```

Use **get\-variant\-import\-job** to check the status\. 

```
aws omics get-variant-import-job --job-id 08279950-a9e3-4cc3-9a3c-a574f9c9e229      
```

You'll receive a JSON response that shows the status of your import job\.

```
{
    "id": "08279950-a9e3-4cc3-9a3c-a574f9c9e229",
    "destinationName": "TestVariant",
    "status": "CREATING",
    "creationTime": "2022-07-14T00:40:18.755559Z"
}
```

If necessary, you can cancel an import job with the following command\.

```
aws omics cancel-variant-import-job --job-id (jobID)
```

## Setting up AWS Lake Formation console<a name="create-resource-links"></a>

In the AWS Lake Formation console, you can view the permissions by choosing **Data lake permissions** in the primary navigation bar\. On the **Data permissions** page, you can view a table that shows what **Resource types**, **Databases**, and the **ARN** related to a shared resource under **RAM Resource Share**\. If you need to accept a RAM Resource Share, AWS Lake Formation will notify you in the console\.

Amazon Omics can implicitly accept the RAM Resource Shares during store creation\. To accept the RAM Resource Share, the IAM role or user that calls the CreateVariantStore or CreateAnnotationStore APIs must allow the following actions:
+ `ram:GetResourceShareInvitations` \- This action allows Amazon Omics to find the invitations\.
+ `ram:AcceptResourceShareInvitation` \- This action allows Amazon Omics to accept the invitation via FAS token\.

Without these permissions, you will see an authorization error during store creation\.

Here is a sample policy that includes these actions\.

```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "omics:*",
        "ram:AcceptResourceShareInvitation",
        "ram:GetResourceShareInvitations"
      ],
      "Resource": "*"
    }
  ]
}
```

To make a shared resource that Omics Analytics users can query, the default access controls must be disabled\. To learn more about disabling default access controls, see [Changing the default security settings for your data lake](https://docs.aws.amazon.com/lake-formation/latest/dg/change-settings.html) in the Lake Formation documentation\. You can create resource links individually or as a group, so that you can access data in Athena or other AWS services\.

**Creating resource links in the AWS Lake Formation console and sharing them with Omics Analytics users**

1. Open the AWS Lake Formation console: [https://console\.aws\.amazon\.com/lakeformation/](https://console.aws.amazon.com/lakeformation)

1. In the primary navigation bar, choose **Databases**\.

1. In the **Databases** table, choose the **Name** of Omics Analytics data store\.

1. On the Omics Analytics data store details page, choose **Actions \(▼\)**\.

1. Choose **Create resource link**\.

1. Next, you must provide a **Resource link name**\.

1. Choose **Create**\.

1. The new resource link is now listed under **Databases**\.

Now, the Lake Formation database administrator needs to grant access to this shared resource using **Grant on target**\.

1. Open the AWS Lake Formation console: [https://console\.aws\.amazon\.com/lakeformation/](https://console.aws.amazon.com/lakeformation)

1. In the primary navigation bar, choose **Databases**\.

1. On the **Databases** page, Choose the radio button next the **Name** of the resource link you previously created\.

1. Next, choose **Actions \(▼\)**\.

1. Then, choose **Grant on target**\.

1. On the **Grant data permissions** page under **Principals**, choose **IAM users or roles**\.

1. Under **IAM users or roles** use the **down arrow \(▼\)** to find the IAM user to which you want to grant access\.

1. Next, under **LF\-Tags or catalog resources** card, select the **Named data catalog resources** option\.

1. Under **Tables\-optional** use the **down arrow \(▼\)** to choose **All Tables** you previously created\.

1. In the **Table permissions** card, under **Table permissions** choose **Describe** and **Select**\.

1. Next, choose **Save**\.

To view the Lake Formation permissions that you have granted, choose **Data lake permissions** from the primary navigation pane\. The table shows all databases and resource links that you have created\.