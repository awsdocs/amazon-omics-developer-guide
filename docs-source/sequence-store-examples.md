# Sequence store tutorial<a name="sequence-store-examples"></a>

## Creating a Sequence Store using the AWS Command Line Interface<a name="sequence-store-examples-cli"></a>

The following example demonstrates using the `CreateSequenceStore` operation with the AWS CLI\. To run the example, you must install the AWS CLI\.

The example is formatted for Unix, Linux, and macOS\. For Windows, replace the backslash \(\\\) Unix continuation character at the end of each line with a caret \(^\)\.

Omics Storage provides storage for genomic files in FASTQ, BAM, and CRAM formats\. These files are stored in Read Sets, which are an AWS resource\. To store Read Sets, you need to first create a Sequence store, as shown in the following example\.

```
aws omics create-sequence-store --name "MySequenceStore"
```

You will receive the following response in JSON, which include the ID number for your newly created Sequence store\.

```
{
    "id": "3936421177",
    "arn": "arn:aws:omics:us-west-2:(account):sequenceStore/3936421177",
    "name": "MySequenceStore",
    "creationTime": "2022-07-13T20:09:26.038Z"
}
```