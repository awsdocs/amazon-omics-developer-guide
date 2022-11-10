# Starting and managing runs<a name="starting-and-managing-a-run"></a>

To run a workflow, you first need an input data file in an Amazon S3 bucket or an Omics sequence store\. For this example, create a text file called "hello\_world\.txt" and upload it to an Amazon S3 bucket\. Create a file “params\_sample\_content\.json” with the definition of the input file to use as shown\.

```
{
"input_txt_file": "s3://(your_bucket)/hello_world.txt"
}
```

You can then use the **start\-run** API using the IAM role you've created, as well as the Amazon S3 bucket created in the setup\.

```
aws omics start-run --workflow-id 12345 --role-arn arn:aws:iam::(account):role/demoRole --output-uri (personal_bucket) --parameters file://params_sample_content.json   
```

In response, you will get the following output\.

```
{
    "arn": "arn:aws:omics:us-west-2:....:run/6789", 
    "id": "6789",
    "status": "PENDING"
}
```

You can then use the ID in the response with the **get\-run** API to check the status of a run, as shown\. 

```
aws omics get-run --id 6789
```

The response from this API will tell you the status of the workflow run\. Possible statuses are PENDING, STARTING, RUNNING, and COMPLETED\. Once a run is in the COMPLETED state, there will be an output file called "outfile\.txt" in your output Amazon S3 bucket, in a folder named after the run ID\. 

You can see the status of all runs with the **list\-runs** API, as shown\.

```
aws omics list-runs 
```

To see all the tasks completed for a specific run, use the **list\-run\-tasks** API\.

```
aws omics list-run-tasks --id (run_)id      
```

To get the details of any specific task, use the get\-run\-task API\.

```
aws omics get-run-task --id (run_id) --task-id (task_id)     
```