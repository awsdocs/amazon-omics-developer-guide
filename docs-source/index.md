# Amazon Omics Developer Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in
connection with any product or service that is not Amazon's,
in any manner that is likely to cause confusion among customers,
or in any manner that disparages or discredits Amazon. All other
trademarks not owned by Amazon are the property of their respective
owners, who may or may not be affiliated with, connected to, or
sponsored by Amazon.

-----
## Contents
+ [What is Amazon Omics?](what-is-service.md)
+ [Setting up Amazon Omics](setting-up.md)
+ [Amazon Omics permissions](omics-permissions.md)
   + [Identity-based IAM policies for Amazon Omics](permissions-user.md)
   + [Service roles for Amazon Omics](permissions-service.md)
   + [Resource permissions](permissions-resource.md)
+ [Getting started with Amazon Omics](getting-started.md)
   + [Key Concepts](concepts.md)
   + [CLI Tutorials](getting-started-tutorials.md)
      + [Sequence store tutorial](sequence-store-examples.md)
      + [Variant store tutorial](variant-store-examples.md)
      + [Workflow tutorial](workflow-examples.md)
   + [Getting Started (Console)](console-examples.md)
      + [Amazon Omics Storage](gs-console-storage.md)
      + [Omics Analytics](gs-console-analytics.md)
      + [Omics Workflows](gs-console-workflows.md)
+ [Security in Amazon Omics](security.md)
   + [Data protection in Amazon Omics](data-protection.md)
   + [Identity and access management for Amazon Omics](security-iam.md)
      + [How Amazon Omics works with IAM](security_iam_service-with-iam.md)
         + [Cross-service confused deputy prevention](cross-service-confused-deputy-prevention.md)
      + [Identity-based policy examples for Amazon Omics](security_iam_id-based-policy-examples.md)
      + [AWS managed policies for Amazon Omics](security-iam-awsmanpol.md)
      + [Troubleshooting Amazon Omics identity and access](security_iam_troubleshoot.md)
   + [Compliance validation for Amazon Omics](compliance-validation.md)
   + [Resilience in Amazon Omics](disaster-recovery-resiliency.md)
+ [Logging Amazon Omics API calls using AWS CloudTrail](logging-using-cloudtrail.md)
+ [Monitoring Amazon Omics](monitoring-overview.md)
   + [Monitoring Omics with Amazon CloudWatch](monitoring-cloudwatch.md)
+ [Tagging resources in Amazon Omics](tagging.md)
   + [Adding a tag to an Omics resource](add-a-tag.md)
   + [Listing tags for a resource](list-tags.md)
   + [Removing tags from a data store](remove-tags.md)
+ [Amazon Omics and interface VPC endpoints (AWS PrivateLink)](vpc-interface-endpoints.md)
+ [Omics Storage](sequence-stores.md)
   + [Creating reference and sequence stores](creating-datastores-cli-examples.md)
      + [Creating and managing sequence stores](manage-sequence-store.md)
      + [Sequence store imports](import-sequence-store.md)
      + [Deleting reference and sequence stores](deleting-reference-and-sequence-stores.md)
   + [Exporting reads sets](read-set-exports.md)
   + [Activating read sets](activating-read-sets.md)
+ [Omics Analytics](omics-analytics.md)
   + [Creating and managing variant stores](creating-variant-stores.md)
      + [Creating and managing annotation store examples](creating-and-managing-annotation-store-examples.md)
      + [Deleting a variant or annotation store](deleting-a-store-examples.md)
+ [Omics Workflows](workflows.md)
   + [Using workflows](creating-workflows.md)
      + [Using custom IAM permissions for runs](custom-authorization-runs.md)
      + [Creating and managing RunGroups](creating-and-managing-rungroups.md)
      + [Starting and managing runs](starting-and-managing-a-run.md)
      + [Workflow definition examples in WDL and Nextflow](workflow-definition-examples.md)
      + [Nextflow Workflow outputs](workflow-outputs.md)
      + [Deleting workflows and runs](deleting-workflows-and-runs.md)
+ [Troubleshooting](troubleshooting.md)
   + [Why can't I run my workflow?](no-run-workflow.md)
   + [Why can't I create a reference store?](reference-store-creation.md)
   + [Why can't I create a sequence store?](sequence-store-creation.md)
   + [Why can't I create a workflow?](workflow-creation.md)
   + [Why did my task fail?](task-fail.md)
   + [Why can't I import my BAM, CRAM or FASTQ files?](import-troubleshooting.md)
   + [Why can't I import my VCF or gVCF files?](variant-store-import-troubleshooting.md)
   + [Why can't I see my annotation store or variant store in Athena?](athena-troubleshooting.md)
   + [Why can't I access my data store in Athena?](athena-enginetroubleshooting.md)
+ [Quotas for Amazon Omics](load-balancer-limits.md)
+ [Document history for the Amazon Omics User Guide](doc-history.md)