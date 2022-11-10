# Activating read sets<a name="activating-read-sets"></a>

You can activate reads sets that are archived with the **start\-read\-set\-activation\-job** API or through the AWS CLI as shown in the following example\.

```
aws omics start-read-set-activation-job --sequence-store-id (sequence-store-id) --sources readSetId=(read-set-id-1) readSetId=(read-set-id-2)        
```

In response, you will receive a response summarizing the information of the activation job, as shown\.

```
{
    "id": (job-id),
    "sequenceStoreId": (sequence-store-id),
    "status": "SUBMITTED",
    "creationTime": "2022-10-22T00:50:54.670000+00:00"
}
```

Once the activation job as started, you can monitor its progress with the **get\-read\-set\-activation\-job ** API\. The following is an example of how to use the AWS CLI to check your activation job status\.

```
aws omics get-read-set-activation-job --id (job-id) --sequence-store-id (sequence-store-id)                    
```

The response will summarize the activation job as shown\.

```
{
    "id": (job-id),
    "sequenceStoreId": (sequence-store-id),
    "status": "SUBMITTED",
    "statusUpdateReason": "The job is submitted and will start soon.",
    "creationTime": "2022-10-22T00:50:54.670000+00:00",
    "sources": [
        {
            "readSetId": (read-set-id-1),
            "status": "NOT_STARTED",
            "statusUpdateReason": "The source is queued for the job."
        },
        {
            "readSetId": (read-set-id-2),
            "status": "NOT_STARTED",
            "statusUpdateReason": "The source is queued for the job."
        }
    ]
}
```

You can check the status of an activation job with the **get\-read\-set\-metadata** API\. Possible statuses are `ACTIVE`, `ACTIVATING`, and `ARCHIVED`\.

```
aws omics get-read-set-metadata --id (read-set-id) --sequence-store-id (sequence-store-id)        
```

The following response shows that the read set is active\.

```
{
    "id": (read-set-id),
    "arn": (read-set-arn),
    "sequenceStoreId": (sequence-store-id),
    "subjectId": (subject-id),
    "sampleId": (sample-id),
    "status": "ACTIVE", 
    "name": (read-set-name),
    "fileType": (file-type),
    "creationTime": "2022-10-22T00:45:15.986000+00:00",
    "sequenceInformation": {...},
    "referenceArn": (reference-arn),
    "files": {...}
}
```

You can view all read set activation jobs using the **list\-read\-set\-activation\-jobs** as shown in the following example\.

```
aws omics list-read-set-activation-jobs --sequence-store-id (sequence-store-id)                   
```

You will receive the following response\.

```
{
    "activationJobs": [
        {
            "id": (job-id),
            "sequenceStoreId": (sequence-store-id),
            "status": "COMPLETED",
            "creationTime": "2022-10-22T01:33:38.079000+00:00",
            "completionTime": "2022-10-22T01:34:28.941000+00:00"
        }
    ]
}
```