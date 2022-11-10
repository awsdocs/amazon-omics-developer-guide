# Deleting reference and sequence stores<a name="deleting-reference-and-sequence-stores"></a>

Both reference and sequence stores can be deleted\. Sequence stores can only be deleted if they don't contain read sets, and reference stores can only be deleted if they don't contain references\. Deleting a sequence or reference store will also delete any tags associated with that store\.

The following example shows how to delete a reference store using the AWS CLI\. If the action is successful, you won't receive a response\.

```
aws omics delete-reference-store --id (reference-store-id)               
```

Similarly, a sequence store can be deleted as shown\. You will not receive a response if the action is successful\.

```
aws omics delete-sequence-store --id (sequence-store-id)             
```

You can also delete a reference in a reference store as shown in the following example\. References can only be deleted if they are not being used in a read set, variant store, or annotation store\.

```
aws omics delete-reference  --id (reference-id) --reference-store-id (reference-store-id)           
```