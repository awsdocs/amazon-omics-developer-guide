# Variant store tutorial<a name="variant-store-examples"></a>

## Creating a Variant Store using the AWS Command Line Interface<a name="variant-store-examples-cli"></a>

The following example demonstrates using the `CreateVariantStore` operation with the AWS CLI\. To run the example, you must install the latest version of the AWS CLI\.

The example is formatted for Unix, Linux, and macOS\. For Windows, replace the backslash \(\\\) Unix continuation character at the end of each line with a caret \(^\)\.

To create a variant store, we will need a referenceName and name parameter\. The variant store is ready to ingest data when its status is shown as READY\. 

```
aws omics create-variant-store --name (storeName) --reference (referenceArn)
```

To confirm the creation of your variant store, you will receive the following response\.

```
{
    "id": "b533f097bade",
    "reference": {
        "referenceArn": "arn:aws:omics:us-west-2:451654099157:referenceStore/5638433913/reference/5871590330"
    },
    "status": "CREATING",
    "name": "variantstore",
    "creationTime": "2022-11-08T01:29:36.594566+00:00"
}
```