# Sequence store imports<a name="import-sequence-store"></a>

Once your sequence store is created, you can create import jobs to load files into the data store\. Your Amazon S3 bucket must be in the same Region as your sequence store\. You can use a command similar to the following to move files into your Amazon S3 bucket\.

```
aws s3 cp s3://1000genomes/phase1/data/HG00100/alignment/HG00100.chrom20.ILLUMINA.bwa.GBR.low_coverage.20101123.bam s3://your-bucket
aws s3 cp s3://1000genomes/phase3/data/HG00146/sequence_read/SRR233106_1.filt.fastq.gz s3://your-bucket
aws s3 cp s3://1000genomes/phase3/data/HG00146/sequence_read/SRR233106_2.filt.fastq.gz s3://your-bucket
aws s3 cp s3://1000genomes/data/HG00096/alignment/HG00096.alt_bwamem_GRCh38DH.20150718.GBR.low_coverage.cram s3://your-bucket
```

You can reuse the IAM access policy that was used to create the Reference store\. You also must create a manifest file as follows in JSON to model the import job in import\.json\. If you are creating a sequence store in the console, you will not need to specify the `sequenceStoreId` or `roleARN`, so your manifest file will start with the `sources` input\.

------
#### [ API Manifest ]

This example code is used to import three Read Sets, one FASTQ, one BAM, and one CRAM using the API\.

```
{
    "sequenceStoreId": "3936421177",
    "roleArn": "arn:aws:iam::(your-account):role/OmicsImport",
    "sources":
    [
        {
            "sourceFiles":
            {
                "source1": "s3://your-bucket/HG00100.chrom20.ILLUMINA.bwa.GBR.low_coverage.20101123.bam"
            },
            "sourceFileType": "BAM",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:(your-account):referenceStore/3242349265/reference/8625408453",
            "name": "HG00100"
        },
        {
            "sourceFiles":
            {
                "source1": "s3://your-bucket/SRR233106_1.filt.fastq.gz",
                "source2": "s3://your-bucket/SRR233106_2.filt.fastq.gz"
            },
            "sourceFileType": "FASTQ",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:(your-account):referenceStore/3242349265/reference/8625408453",
            "name": "HG00146"
        },
        {
            "sourceFiles":
            {
                "source1": "s3://your-bucket/HG00096.alt_bwamem_GRCh38DH.20150718.GBR.low_coverage.cram"
            },
            "sourceFileType": "CRAM",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:(your-account):referenceStore/3242349265/reference/8625408453",
            "name": "HG00096"
        }
    ]
}
```

------
#### [ Console Manifest ]

This example code is used to import a single read set using the console\.

```
{
    "sourceFiles":
    {
        "source1": "s3://your-bucket/HG00100.chrom20.ILLUMINA.bwa.GBR.low_coverage.20101123.bam"
    },
    "sourceFileType": "BAM",
    "subjectId": "mySubject",
    "sampleId": "mySample",
    "referenceArn": "arn:aws:omics:us-west-2:(your-account):referenceStore/3242349265/reference/8625408453",
    "name": "HG00100"
}
```

------

The manifest file can also be uploaded in YAML format\.

To start the import job, use the following AWS CLI command\.

```
aws omics start-read-set-import-job --cli-input-json file://import.json      
```

You will receive the following response, showing that the job has been created\.

```
{
    "id": "3660451514",
    "sequenceStoreId": "3936421177",
    "roleArn": "arn:aws:iam::(account):role/OmicsImport",
    "status": "CREATED",
    "creationTime": "2022-07-13T22:14:59.309Z"
}
```

Once the import job is started, you can monitor its progress with the following command\. 

```
aws omics get-read-set-import-job --sequence-store-id 3936421177 --id 3660451514   
```

The following shows the statuses for all import jobs associated with the Sequence store id specified\.

```
{
    "id": "3660451514",
    "sequenceStoreId": "3936421177",
    "roleArn": "arn:aws:iam::(account):role/OmicsImport",
    "status": "RUNNING",
    "creationTime": "2022-07-13T22:14:59.309Z",
    "sources": [
        {
            "sourceFiles": {
                "source1": "s3://genomic-resource-bucket-555555555555/HG00100.chrom20.ILLUMINA.bwa.GBR.low_coverage.20101123.bam",
            },
            "sourceFileType": "BAM",
            "status": "IN_PROGRESS",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "name": "HG00100"
        },
        {
            "sourceFiles": {
                "source1": "s3://genomic-resource-bucket-555555555555/SRR233106_1.filt.fastq.gz",
                "source2": "s3://genomic-resource-bucket-555555555555/SRR233106_2.filt.fastq.gz"
            },
            "sourceFileType": "FASTQ",
            "status": "IN_PROGRESS",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "name": "HG00146"
        },
        {
            "sourceFiles": {
                "source1": "s3://genomic-resource-bucket-555555555555/HG00096.alt_bwamem_GRCh38DH.20150718.GBR.low_coverage.cram",
            },
            "sourceFileType": "CRAM",
            "status": "IN_PROGRESS",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "name": "HG00096"
        }
    ]
}
```

After the job is completed, you can use the **list\-read\-sets** operation to find which sequence files were imported as shown\.

```
aws omics list-read-sets --sequence-store-id 3936421177
```

You will receive the following response\.

```
{
    "readSets": [
        {
            "id": "1865791106",
            "arn": "arn:aws:omics:us-west-2:555555555555:sequenceStore/3936421177/readSet/1865791106",
            "sequenceStoreId": "3936421177",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "status": "ACTIVE",
            "name": "HG00100",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "fileType": "BAM",
            "creationTime": "2022-07-13T23:25:20Z"
        },
        {
            "id": "4473379892",
            "arn": "arn:aws:omics:us-west-2:555555555555:sequenceStore/3936421177/readSet/4473379892",
            "sequenceStoreId": "2381480989",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "status": "ACTIVE",
            "name": "HG00146",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "fileType": "FASTQ",
            "creationTime": "2022-07-13T23:26:43Z"
        },
        {
            "id": "1456291090",
            "arn": "arn:aws:omics:us-west-2:555555555555:sequenceStore/3936421177/readSet/1456291090",
            "sequenceStoreId": "2381480989",
            "subjectId": "mySubject",
            "sampleId": "mySample",
            "status": "ACTIVE",
            "name": "HG00096",
            "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
            "fileType": "CRAM",
            "creationTime": "2022-07-13T23:30:41Z"
        }
    ]
}
```

To view more details about a given read set, use the **get\-read\-set\-metadata API**\.

```
aws omics get-read-set-metadata --sequence-store-id 3936421177 --id 1865791106     
```

You will receive the following response\.

```
{
    "id": "1865791106",
    "arn": "arn:aws:omics:us-west-2:555555555555:sequenceStore/3936421177/readSet/1865791106",
    "sequenceStoreId": "3936421177",
    "subjectId": "mySubject",
    "sampleId": "mySample",
    "status": "ACTIVE",
    "name": "HG00100",
    "fileType": "BAM",
    "creationTime": "2022-07-13T23:25:20Z",
    "sequenceInformation": {
        "totalReadCount": 1513467,
        "totalBaseCount": 163454436,
        "alignment": "ALIGNED"
    },
    "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/3242349265/reference/8625408453",
    "files": {
        "source1": {
            "totalParts": 2,
            "partSize":  10485760
            "contentLength": 17112283
        },
        "index": {
            "totalParts": 1,
            "partSize": 53216,
            "contentLength": 10485760
        }
    }
}
```

You can parallelize your download by downloading individual parts, which are similar to Amazon S3 parts, using **get\-read\-set**\. The following is an example of how you would download part 1 from a read set

```
aws omics get-read-set --sequence-store-id 3936421177 --id 8625408453 --part-number 1 outfile.bam  
```

The Omics Transfer Manager is also available to download files for an Omics reference or read set\. You can download the Omics Transfer Manager [here](https://pypi.org/project/amazon-omics-tools/)\. More information on how to use and set up the Transfer Manager can be found in this [GitHub Repository](https://github.com/awslabs/amazon-omics-tools/)\.