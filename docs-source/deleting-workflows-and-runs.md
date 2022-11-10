# Deleting workflows and runs<a name="deleting-workflows-and-runs"></a>

When you no longer need a workflow, run, or run group, you can delete it using the AWS CLI, APIs, or console\. Workflows can only be deleted when they are listed in `ACTIVE` status, and deleting a workflow does not affect any ongoing runs using that workflow\.

The following example shows how you can use the AWS CLI command to delete a workflow\. You won't receive a response\.

```
aws omics delete-workfow --id (workflow id)            
```

In addition to deleting a run, you can also cancel a run\. To cancel a run, it status must be `PENDING`, `STARTING`, `RUNNING`, or `STOPPING`\. The following AWS CLI command shows how you can cancel a run\. If successful, there will be no response\. 

```
aws omics cancel-run --id (run id)            
```

The following AWS CLI command deletes a run\. Runs can only be deleted if they are complete or have been cancelled\. There is no response if the run is successfully deleted\.

```
aws omics delete-run --id (run id)            
```

You can also delete run groups\. Run groups can only be deleted if there are no runs associated with that run group witsh the status of `PENDING`, `STARTING`, `RUNNING`, or `STOPPING`\.

The following example shows how you can use the AWS CLI to delete a run group\. You will not receive a response\.

```
aws omics delete-run-group --id (run id)            
```