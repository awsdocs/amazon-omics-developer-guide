# Creating and managing sequence stores<a name="manage-sequence-store"></a>

Omics provides storage for genomic files in FASTQ \(gzip\-only\), BAM, and CRAM formats\. These files are stored as read sets, which are an AWS resource\. This means you can add tags and control access through IAM\. To store read sets, you must first create a sequence store, as shown in the following example\.

```
aws omics create-sequence-store --name "MySequenceStore"  
```

You will receive the following response in JSON, which includes the ID number for your newly created sequence store\.

```
{
    "id": "3936421177",
    "arn": "arn:aws:omics:us-west-2:(account):sequenceStore/3936421177",
    "name": "MySequenceStore",
    "creationTime": "2022-07-13T20:09:26.038Z"
}
```



You can also view all sequence stores associated with your account by using the list\-sequence\-stores command, as shown\.

```
aws omics list-sequence-stores
```

You will receive the following response\.

```
{
    "sequenceStores": [
        {
            "arn": "arn:aws:omics:us-west-2:(account):sequenceStore/3936421177",
            "id": "3936421177",
            "name": "MySequenceStore",
            "creationTime": "2022-07-13T20:09:26.038Z"
        }
    ]
}
```