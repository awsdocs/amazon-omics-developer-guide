# Exporting reads sets<a name="read-set-exports"></a>

You can export read sets as a batch export job to an Amazon S3 bucket\. To do so, first create an IAM policy that has write access to the bucket, similar to the following IAM policy example\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetBucketLocation"
             ],
             "Resource": [
                "arn:aws:s3:::your-bucket",
                "arn:aws:s3:::your-bucket/*"
             ]
        }
    ]
}
```

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

After the IAM policy is in place, you can then begin your read set export job\. The following example shows how to do so using the **start\-read\-set\-export\-job** API\. 

```
aws omics start-read-set-export-job --sequence-store-id (sequence-store-id) --destination (valid-s3-uri) --role-arn (role-arn) --sources readSetId=(read-set-id-1) readSetId=(read-set-id-2)            
```

You will receive the following response with information on the origin sequence store and the destination Amazon S3 bucket\.

```
{
    "id": (job-id),
    "sequenceStoreId": (sequence-store-id),
    "destination": (destination-s3-uri),
    "status": "SUBMITTED",
    "creationTime": "2022-10-22T01:33:38.079000+00:00"
}
```

Once the job is started, you check on its status using the **get\-read\-set\-export\-job** API, as shown\.

```
aws omics get-read-set-export-job --id (job-id) --sequence-store-id (sequence-store-id)        
```

You can view all export jobs initialized for a sequence store using the ** list\-read\-set\-export\-jobs** API, as shown\.

```
aws omics list-read-set-export-jobs --sequence-store-id (sequence-store-id)
```

```
{
    "exportJobs": [
        {
            "id": (job-id),
            "sequenceStoreId": (sequence-store-id),
            "destination": (destination-s3-uri),
            "status": "COMPLETED",
            "creationTime": "2022-10-22T01:33:38.079000+00:00",
            "completionTime": "2022-10-22T01:34:28.941000+00:00"
        }
    ]
}
```