# Omics Workflows<a name="gs-console-workflows"></a>

With Omics Workflows, you can create workflows, runs, and run groups\.

## Create a workflow<a name="gs-console-create-workflows"></a>

**To create a workflow**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Workflows**\.

1. On the **Create workflow** page, provide the following information
   + **Workflow main definion path** \- The file path that directs to the workflow definition\. 
   + **Workflow name** \- A distinctive name for this workflow\. 
   + **Description** \(optional\) \- A description of this workflow\.
   + **Run storage capacity\(optional\)** \- The default amount of storage needed for this workflow\. The default is 1\.2 TB\. This storage is deleted after the run completes\.
   + **Workflow definition** \- The Amazon S3 path to the workflow definition zip\. Choose whether it is written in Nextflow or WDL from the drop down box\.
   + **Tags** \(optional\) \- Provide up to 50 tags for this workflow\.

1. Choose **Next**\.

1. On the **Add workflow parameters** page, provide the workflow parameters\. You can either upload a JSON file that specifies the parameters or manually enter your workflow parameters\.

1. Choose **Create workflow**\.

## Start a run<a name="gs-console-start-runs"></a>

**To start a workflow run**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Runs**\.

1. On the **Create run** page, provide the following information
   + **Workflow ID** \- The workflow ID associated with this run\. 
   + **Run name** \- A distinctive name for this run\.
   + **IAM role** \- The IAM role that can access the data locations referenced in your parameter values\.It should also contain a Cloud Watch policy for the service to publish logs to your Cloud Watch account\.
   + **Run priority** \- The priority of this run\. Higher numbers specify a higher priority, and the highest priority tasks are run first\.
   + **Run storage capacity** \- The amount of temporary storage needed for the run\. By default, the run storage capacity that was set for the workflow will be selected\. You can select a different run storage capacity for your run\.
   + **Select S3 output destination** \- The S3 location where the run outputs will be saved\.

1. Choose **Next**\.

1. On the **Add parameter values** page, provide the workflow parameters\. You can either upload a JSON file that specifies the parameters or manually enter your workflow parameters\.

1. Choose **Next**\.

1. On the **Add run groups and tags** page, provide the run group details\. Optionally, you can optionally provide up to 50 tags for this run\.

1. Choose **Create run**\.

## Create a run group<a name="gs-console-create-run-groups"></a>

**To create a run group**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Run groups**\.

1. On the **Run groups** page, choose **Create run group**\.

1. On the **Create run group details** page, provide the following information
   + **Run group name** \- A unique name for this run group\. 
   + **Max vCPU for concurrent runs** \- The maximum number of vCPUs running in parallel across the run group\.
   + **Max run time \(hrs\) per run** \- The maximum amount of time that a run can be active\.
   + **Tags** \(optional\) \- Provide up to 50 tags for this sequence store\.

1. Choose **Create run group**\.