# Listing tags for a resource<a name="list-tags"></a>

Follow these steps to use the AWS CLI to view a list of the AWS tags for an Omics resource\. If no tags have been added, the returned list is empty\.

 At the terminal or command line, run the list\-tags\-for\-resource command as shown in the following example\.

```
aws omics list-tags-for-resource --resource-arn arn:aws:omics:us-west-2:555555555555:sequenceStore/2275234794       
```

You will receive a list of tags in response, in JSON format\.

```
 {
    "tags": {
        "key1": "value1",
        "key2": "value2"
    }
}
```