# Quotas for Amazon Omics<a name="load-balancer-limits"></a>

Your AWS account has default quotas, formerly referred to as limits, for each AWS service\. Unless otherwise noted, each quota is Region\-specific\. You can request increases for some quotas, and other quotas cannot be increased\.

To view the quotas for Amazon Omics, open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\. In the navigation pane, choose **AWS services** and select **Amazon Omics**\.

To request a quota increase, see [Requesting a Quota Increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota is not yet available in Service Quotas, use the [limit increase form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\.

Your AWS account has the following quotas related to Amazon Omics\. With the exception of the quota for number of reference stores, all can be adjusted by request\.


| Maximum Resource per account or region | Default | 
| --- | --- | 
| Sequence stores | 20 | 
| Reference stores | 1 | 
| Read sets in a sequence store | 1,000,000 | 
| References in a reference store | 50 | 
| Concurrent import jobs | 20 | 
| Concurrent export jobs | 20 | 
| Read sets per activate job | 1000 | 
| Concurrent activate jobs | 20 | 
| Read set sources per import job | 1000 | 
| Read sets per export job | 1000 | 
| Workflows | 100 | 
| Workflow runs\(active or inactive\) | 200 | 
| Active workflow runs | 3 | 
| Concurrent tasks per run | 50 | 
| Concurrent active CPUs | 512 | 
| Workflow run duration | 168 hours | 
| Variant stores | 10 | 
| Annotation stores | 10 | 
| Concurrent import jobs \(variant or annotation stores\) | 5 | 
| Files per import job \(variant or annotation stores\) | 1 | 
| Variant store import file size \(GB\) | 20 | 
| Annotation store import file size \(GB\) | 20 | 

The throttling quotas for Omics are as follows\. All are in transactions\-per\-second\(TPS\)\.


| API Operation | Transactions per second \(TPS\) | 
| --- | --- | 
| CreateSequenceStore, CreateReferenceStore, DeleteSequenceStore, DeleteReferenceStore | 1 | 
| GetSequenceStore, ListSequenceStores | 5 | 
|  GetReadSet | 10 | 
|  GetReadSetMetadata, ListReadSets | 5 | 
|  BatchDeleteReadSet | 1 | 
| StartReadSetImportJob, GetReadSetImportJob, ListReadSetImportJobs | 5 | 
|  StartReadSetExportJob, GetReadSetExportJob, ListReadSetImportJobs | 5 | 
| GetReferenceStore, ListReferenceStores | 5 | 
| StartReferencetImportJob, GetReferenceImportJob, ListReferenceImportJobs | 5 | 
|  ListReferences, GetReferenceMetadata | 5 | 
|  DeleteReference | 1 | 
|  StartReadsetActivationJob | 1 | 
|  ListReadsetActivationJobs, GetReadSetActivationJob | 5 | 
|  GetReference | 10 | 
| TagResource, UntagResource, ListTagsForResource | 5 | 