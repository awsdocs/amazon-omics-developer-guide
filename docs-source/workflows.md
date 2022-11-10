# Omics Workflows<a name="workflows"></a>

With Omics Workflows, you can process and analyze your genomics data using workflows described in WDL or Nextflow\. A single invocation of a workflow is called a run, and a single process within a run is called a task\. Omics workflows uses your requested vCPU and memory to run each task\. A unique identifier is provided for each created workflow\. You can track your workflow version with either a tag or git hash\. Your workflow tools must be containerized and registered in your Amazon Elastic Container Registry \(Amazon ECR\) before you can create a workflow\. A workflow can be run on genomics data stored in an Amazon S3 bucket or in an Omics sequence store\.

With the Workflow APIs, you can perform the following actions\.
+ Creating, retrieving, and managing workflows
+ Starting and managing runs, including cancellation and deletion
+ Tracking the status of ongoing the runs
+ Creating and managing run groups
+ Tag AWS resources such as workflows, runs, tasks, and run groups

All parameter types for WDL and Nextflow are supported\. To learn more about workflow languages, see the specifications for [WDL](https://github.com/openwdl/wdl/blob/main/versions/1.1/SPEC.md) or [Nextflow](https://www.nextflow.io/docs/latest/script.html)\.