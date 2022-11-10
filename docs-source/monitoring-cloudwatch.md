# Monitoring Omics with Amazon CloudWatch<a name="monitoring-cloudwatch"></a>

You can monitor Omics using CloudWatch, which collects raw data and processes it into readable, near real\-time metrics\. These statistics are kept for 15 months, so that you can access historical information and gain a better perspective on how your web application or service is performing\. You can also set alarms that watch for certain thresholds, and send notifications or take actions when those thresholds are met\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

The Omics service reports the following metrics in the `AWS/Omics` namespace\.

 API Call Count metrics are reported for the following Omics APIs\. Only the API Operation dimension is reported\. 
+ Reference and reference store APIs —CreateReferenceStore, DeleteReferenceStore, StartReferenceImportJob, StartReferenceExportJob 
+ Sequence store and Read set APIs —CreateSequenceStore, DeleteSequenceStore, StartReadSetImportJob, StartReadSetActivationJob, StartReferenceImportJob, StartReferenceExportJob
+  Variant store APIs — CreateVariantStore, DeleteVariantStore, StartVariantImportJob, CancelVariantImportJob 
+  Annotation store APIs — CreateAnnotationStore, DeleteAnotationStore, StartAnnotationImportJob, CancelAnnotationImportJob 
+ Workflow, run, and run group APIs — CreateWorkflow, DeleteWorkflow, StartRun, CancelRun, DeleteRun, CreateRunGroup, DeleteRunGroup 