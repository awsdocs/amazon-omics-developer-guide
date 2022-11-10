# Resource permissions<a name="permissions-resource"></a>

Amazon Omics creates and accesses resources in other services on your behalf when you run a job or create a store\. In some cases, you need to configure permissions in other services to access resources or to allow Amazon Omics to access them\.

**Topics**
+ [Lake Formation](#permissions-resource-lakeformation)
+ [Amazon ECR](#permissions-resource-ecr)

## Lake Formation<a name="permissions-resource-lakeformation"></a>

Before you use analytics features in Amazon Omics, configure default database settings in Lake Formation\.

**To configure resource permissions in Lake Formation**

1. Open the [Data catalog settings](https://console.aws.amazon.com/lakeformation/home#default-permission-settings) page in the Lake Formation console\.

1. Uncheck the IAM access control requirements for databases and tables under **Default permissions for newly created databases and tables**\.

1. Choose **Save**\.

Omics Analytics will auto accept data as long as your service policy has the correct RAM permissions, such as the following example\. 

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
        "ram:AcceptResourceShareInvitation",
        "ram:GetResourceShareInvitations"
      ],
      "Resource": "*"
    }
  ]
}
```

## Amazon ECR<a name="permissions-resource-ecr"></a>

When you run a workflow, you provide access to one or more containers for Amazon Omics by using Amazon Elastic Container Registry \(Amazon ECR\)\. To access container images on your behalf, Amazon Omics needs access to your private registry\. This policy needs to be added to each container registry that will be used with Omics workflows\. Public, private, and cross\-account countainers are supported as long as they are in the same region\. 

**To grant Amazon Omics permission to access Amazon ECR**

1. Open the [private registry permissions](https://console.aws.amazon.com/ecr/private-registry/permissions) page in the Amazon ECR console\.

1. Choose **Edit JSON**\.

1. Add the following policy statement\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "omics workflow",
               "Effect": "Allow",
               "Principal": {
                   "Service": "omics.amazonaws.com"
               },
               "Action": [
                   "ecr:BatchGetImage",
                   "ecr:CompleteLayerUpload"
               ]
           }
       ]
   }
   ```

The resource\-based policy on the registry grants Omics permission to acquire container images\.

To use cross\-account containers in the same region, you would add a permission like the following example\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "OmicsAccessPrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "omics.amazonaws.com"
            },
            "Action": [
                "ecr:BatchGetImage",
                "ecr:CompleteLayerUpload"
            ]
        },
        {
            "Sid":"OmicsAccessCrossAccount",
            "Effect":"Allow",
            "Principal":{
                "AWS":"arn:aws:iam::{{AWS-account-ID}}:root"
        },
            "Action":[
            "ecr:GetDownloadUrlForLayer",
            "ecr:BatchGetImage",
            "ecr:BatchCheckLayerAvailability"
           ]
        }
   ]
}        
    ]
}
```