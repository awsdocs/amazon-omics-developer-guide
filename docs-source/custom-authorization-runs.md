# Using custom IAM permissions for runs<a name="custom-authorization-runs"></a>

You can include any workflow, run, or run group referenced by the StartRun request in the authorization request\. By doing so, you can build IAM policies that will allow or deny start run requests based on the combination of resources in the request\. This includes any combination of workflows and runs or run groups to be explicitly listed in the IAM policy\. For example, you could limit the use of a workflow to a specific run or run group\. You could also specify that a workflow can only be used with a run group\. 

The following is an example IAM policy that allows a single workflow with a single run group\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "omics:StartRun"
            ],
            "Resource": [
                "arn:aws:omics:us-west-2:123456789012:workflow/1234567",
                "arn:aws:omics:us-west-2:123456789012:runGroup/2345678"
            ]
        },
        {
            # Optionally, allow user to rerun a failed run
            "Effect": "Allow",
            "Action": [
                "omics:StartRun"
            ],
            "Resource": [
                "arn:aws:omics:us-west-2:123456789012:run/*",
                "arn:aws:omics:us-west-2:123456789012:runGroup/2345678"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "omics:GetRun",
                "omics:ListRunTasks",
                "omics:GetRunTask",
                "omics:CancelRun",
                "omics:DeleteRun"
            ],
            "Resource": [
                "arn:aws:omics:us-west-2:123456789012:run/*"
            ]
        },
        
    ]
}
```