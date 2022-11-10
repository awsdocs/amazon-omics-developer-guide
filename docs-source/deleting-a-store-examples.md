# Deleting a variant or annotation store<a name="deleting-a-store-examples"></a>

Both variant and annotation stores can be deleted\. Deleting a variant or annotation store will also delete all imported data in that store, and any associated tags\.

The following example shows how to delete a variant store using the AWS CLI\. If the action is successful, the variant store will have a status of DELETING\.

```
aws omics delete-variant-store --id (variant-store-id)               
```

Similarly, an annotation store can be deleted as shown\. If the action is successful, the annotation store will have a status of DELETING\.

```
aws omics delete-annotation-store --id (annotation-store-id)             
```