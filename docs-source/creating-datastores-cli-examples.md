# Creating reference and sequence stores<a name="creating-datastores-cli-examples"></a>

Reference and sequence stores are AWS resources that you can use to manage your genomic data through the APIs, AWS CLI, and console\. The first step is create a reference store to hold reference genomes used to map your read sets\. 

The following example shows how to create a reference store using the AWS CLI\. Only one reference store per &AWS; region is supported\. 

```
aws omics create-reference-store --name "MyReferenceStore"   
```

You will receive a JSON response with the Reference store ID, the name of the reference store, ARN, and the timestamp of when your Reference store was created\.

```
{
    "id": "3242349265",
    "arn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265",
    "name": "MyReferenceStore",
    "creationTime": "2022-07-01T20:58:42.878Z"
}
```

The Reference store ID can be used in additional AWS CLI commands\. You also can always check which Reference store IDs are a linked to your account using the **list\-reference\-stores** command, as shown in the following example\.

```
aws omics list-reference-stores 
```

In response, you will receive the name of the reference store you just created\.

```
{
    "referenceStores": [
        {
              "id": "3242349265",
              "arn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265",
              "name": "MyReferenceStore",
             "creationTime": "2022-07-01T20:58:42.878Z"
         }
     ]
}
```

Once you have a reference store, you can create import jobs to load genomic reference files into it\. To do so, you must use or create an IAM role to access the data\. The following is an example policy\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketLocation",
                "s3:HeadObject"
            ],
            "Resource": [
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET1",
                "arn:aws:s3:::DOC-EXAMPLE-BUCKET1/*"
            ]
         }
      ]
   }   
}
```

You will also need a trust policy similar to the following example\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                   "omics.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

You can now import a reference genome\. This example uses Genome Reference Consortium Human Build 38 \(hg38\), which is open access and available from the [Registry of Open Data on AWS](registry.opendata.aws)\. Use the following AWS CLI command to copy the genome to your Amazon S3 bucket\. 

```
aws s3 cp s3://broad-references/hg38/v0/Homo_sapiens_assembly38.fasta s3://your-bucket   
```

You can then begin your import job\.

```
aws omics start-reference-import-job --reference-store-id 3242349265 --role-arn arn:aws:iam::(aws-account-id):role/OmicsImportRole --sources sourceFile=s3://your-bucket/Homo_sapiens_assembly38.fasta,name=MyReference
```

Once the data is imported, you will receive the following response in JSON\.

```
{
        "id": "7252016478",
        "referenceStoreId": "3242349265",
        "roleArn": "arn:aws:iam::555555555555:role/OmicsReferenceImport",
        "status": "CREATED",
        "creationTime": "2022-07-01T21:15:13.727Z"
}
```

You can monitor the status of a job with the following command\.

```
aws omics get-reference-import-job --reference-store-id 3242349265 --id 7252016478  
```

In response, you will receive a response with the details for that reference store and its status\.

```
{
    "id": "7252016478",
    "referenceStoreId": "3242349265",
    "roleArn": "arn:aws:iam::55555555555:role/OmicsReferenceImport",
    "status": "RUNNING",
    "creationTime": "2022-07-01T21:15:13.727Z",
    "sources": [
        {
            "sourceFile": "s3://your-bucket/Homo_sapiens_assembly38.fasta",
            "status": "IN_PROGRESS",
            "name": "MyReference"
        }
    ]
}
```

You can also find the reference that was imported by listing your references and filtering based on the reference name\.

```
aws omics list-references --reference-store-id 3242349265 --filter name=MyReference   
```

In response, you'll receive the following information\.

```
{
    "references": [
        {
            "id": "8625408453",
            "arn": "arn:aws:omics:us-west-2:55555555555:referenceStore/3242349265/reference/8625408453",
            "referenceStoreId": "3242349265",
            "md5": "7ff134953dcca8c8997453bbb80b6b5e",
            "status": "ACTIVE",
            "name": "MyReference",
            "creationTime": "2022-07-02T00:15:19.787Z",
            "updateTime": "2022-07-02T00:15:19.787Z"
        }
    ]
}
```

To discover more detail about the metadata of the reference, use the **get\-reference\-metadata** API\.

```
aws omics get-reference-metadata --reference-store-id 3242349265 --id 8625408453   
```

You will receive the following information in response\.

```
{
    "id": "8625408453",
    "arn": "arn:aws:omics:us-west-2:(account):referenceStore/3242349265/reference/8625408453",
    "referenceStoreId": "3242349265",
    "md5": "7ff134953dcca8c8997453bbb80b6b5e",
    "status": "ACTIVE",
    "name": "MyReference",
    "creationTime": "2022-07-02T00:15:19.787Z",
    "updateTime": "2022-07-02T00:15:19.787Z",
    "files": {
        "source": {
            "totalParts": 31,
            "partSize": 104857600,
            "contentLength": 3249912778
        },
        "index": {
            "totalParts": 1,
            "partSize": 104857600,
            "contentLength": 160928
        }
    }
}
```

You can also download parts of the reference file using **get\-reference**\.

```
aws omics get-reference --reference-store-id 3242349265 --id 8625408453 --part-number 1 outfile.fa   
```