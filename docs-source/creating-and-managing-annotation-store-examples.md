# Creating and managing annotation store examples<a name="creating-and-managing-annotation-store-examples"></a>

An annotation store is a data store representing an annotation database, such as one from a TSV, VCF, or GFF file\. If the same reference genome is specified, annotation stores are mapped to the same coordinate system as variant stores during an import\. The following examples show how to use the AWS CLI to create and manage an annotation store\. 

In the following example, the AWS CLI is used to create an annotation store\. For all AWS CLI and API operations, the format of your data must be declared\. 

```
aws omics create-annotation-store --name "ExampleStoreA" \
  --store-format VCF \
  --reference referenceArn="arn:aws:omics:us-west-2:555555555555:referenceStore/6505293348/reference/5987565360"
```

To confirm the creation of your annotation store, you will receive the following response\.

```
{
    "creationTime": "2022-08-24T20:34:19.229500Z",
    "id": "3b93cdef69d2",
    "name": "ExampleStoreA",
    "reference": {
        "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/6505293348/reference/5987565360"
    },
    "status": "CREATING"
}
```

To learn more about an annotation store, use the **get\-annotation\-store** API\.

```
aws omics get-annotation-store --name "annotation_store"    
```

You will receive the following response\.

```
{
    "id": "eeb019ac79c2",
    "reference": {
        "referenceArn": "arn:aws:omics:us-west-2:451654099157:referenceStore/5638433913/reference/5871590330"
    },
    "status": "READY",
    "storeArn": "arn:aws:omics:us-west-2:555555555555:annotationStore/gffstore",
    "name": "gffstore",
    "creationTime": "2022-11-05T00:05:19.136131+00:00",
    "updateTime": "2022-11-05T00:10:36.944839+00:00",
    "tags": {},
    "storeFormat": "GFF",
    "statusMessage": "",
    "storeSizeBytes": 0
}
```

To view all annotation stores associated with an account, use the **list\-annotation\-stores** API\.

```
aws omics list-annotation-stores 
```

You will receive a response that lists all annotation stores, along with their IDs, statuses, and other details, as shown in the following example response\.

```
{
    "annotationStores": [
        {
            "id": "41beb48d2d7a",
            "reference": {
                "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/5638433913/reference/5871590330"
            },
            "status": "READY",
            "name": "atest_store_15875024_d3ba_452a_b518_a1c23e44df68",
            "creationTime": "2022-09-15T07:42:51.907026+00:00",
            "updateTime": "2022-09-15T07:47:59.100413+00:00"
        },
        {
            "id": "4d8f3eada259",
            "reference": {
                "referenceArn": "arn:aws:omics:us-west-2:555555555555:referenceStore/5638433913/reference/5871590330"
            },
            "status": "CREATING",
            "name": "atest_store_f3b2181f_ac85_4d93_90be_3544e680a176",
            "creationTime": "2022-09-27T17:30:52.182990+00:00",
            "updateTime": "2022-09-27T17:30:53.025362+00:00"
        }
     ]
}
```

You can also filter responses based on status or other criteria\.

The following example shows how to use the AWS CLI to start an import job\.

```
aws omics start-annotation-import-job \
  --destination-name StoreA \
  --runLeftNormalization false \
  --role-arn  arn:aws:iam::555555555555:role/roleName \
  --items source=s3://pathToS3/example.gff.gz
```

For TSV and VCF formats, there are additional parameters that inform the API on how to parse your input\.

The TSV parser will also perform basic bioinformatics operations like left normalization and standardization of genomics coordinates, as listed in the table\.


| Format type | Description | 
| --- | --- | 
| Generic | Generic text file\. No genomic information | 
| CHR\_POS | Start position \- 1, Add end position, which is the same as POS | 
| CHR\_POS\_REF\_ALT | Contains contig, 1\-base position, ref and alt allele information | 
| CHR\_START\_END\_REF\_ALT\_ONE\_BASE | Contains contig, start, end, ref and alt allele information\. Coordinates are 1\-based | 
| CHR\_START\_END\_ZERO\_BASE | Contains contig, start, and end positions\. Coordinates are 0\-based | 
| CHR\_START\_END\_ONE\_BASE | Contains contig, start, and end positions\. Coordinates are 1\-based | 
| CHR\_START\_END\_REF\_ALT\_ZERO\_BASE | Contains contig, start, end, ref and alt allele information\. Coordinates are 0\-based | 

A TSV Import Annotation store request will look like the following\.

```
aws omics start-annotation-import-job \
  --destination-name TSVExample \
  --role-arn arn:aws:iam::(account):role/demoRole \
  --items source=s3://demodata/genomic_data.bed.gz
  --format-options '{ "tsvOptions": {
         "readOptions": {
            "header": false,
            "sep": "\t"
           }
       }
     }'
```

For VCF files, there are two additional inputs, ignoreQualField and ignoreFilterField that will ignore or include those parameters as shown\.

```
aws omics start-annotation-import-job --destination-name VCFAnnoExample\
  --role-arn arn:aws:iam::(account):role/demoRole \
  --items source=s3://demodata/example.garvan.vcf \
  --format-options '{ "vcfOptions": {
    "ignoreQualField": false,
    "ignoreFilterField": false         
    }
   }'
```

You can also cancel an Annotation store import, as shown\. You will not receive a response to this AWS CLI call if the action is successful, but you will receive error messages if the import job ID is not found or the import job has been completed\. 

```
aws omics cancel-annotation-import-job --job-id (jobID)
```

**Note**  
Your meta\-data import job history for **get\-annotation\-import\-job**, **get\-variant\-import\-job**, **list\-annotation\-import\-jobs**, and **list\-variant\-import\-jobs** is auto\-deleted after two years\. The variant and annotation data imported is not auto\-deleted and will remain in your data stores\.