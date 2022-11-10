# Creating and managing RunGroups<a name="creating-and-managing-rungroups"></a>

Using Run Groups limits the compute resources used per run, and is optional\. You can set the max vCPU, max duration, or max concurrent runs to help limit the compute resources used\.

To create a RunGroup, use the **create\-run\-group** API to create a run group named 'TestRunGroup', with a max of 20 CPUs and a max duration of 600 minutes\. 

```
aws omics create-run-group --name TestRunGroup --max-cpus 20 --max-duration 600    
```

The response from this API will be the newly created RunGroup ID\. You can then use this ID to get additional details using the get\-run\-group API as shown\.

```
aws omics get-run-group --id (run_group_id)   
```

You can also use the **list\-run\-group** API to view all created run groups\.

```
aws omics list-run-groups     
```