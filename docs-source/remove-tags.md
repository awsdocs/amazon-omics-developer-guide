# Removing tags from a data store<a name="remove-tags"></a>

 You can remove one or more tags associated with a resource\. Removing a tag does not delete the tag from other AWS resources that are associated with that tag\.

 At the terminal or command line, run the untag\-resource command, specifying the Amazon Resource Name \(ARN\) of the resource where you want to remove tags and the tag key of the tag you want to remove\.

```
aws omics untag-resource --resource-arn arn:aws:omics:us-west-2:555555555555:sequenceStore/2275234794 --tag-keys key1,key2    
```

If successful, this command does not return a response\. To verify the tags associated with the resource, run the list\-tags\-for\-resource command\.