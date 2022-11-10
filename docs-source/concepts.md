# Key Concepts<a name="concepts"></a>

Definitions for key concepts and terms that are specific to the functions of Amazon Omics are provided in this section\. These definitions are provided to help you understand the terminology of Amazon Omics and this guide\.

**Topics**
+ [Storage](#sequence-store-concepts)
+ [Analytics](#variant-store-concepts)
+ [Workflows](#workflows-concepts)

## Storage<a name="sequence-store-concepts"></a>

Data storage is separated into sequence stores, for your genomics sequences and related information, and a reference store, for all of your reference genomes\. The following terms describe the implementations that are specific to Amazon Omics\.
+ *Sequence store* – A data store for the storage of genomics files\. You can have one or more sequence stores within Amazon Omics\. Access permissions and AWS KMS encryption can be set on a sequence store to control who has access to the data\. 
+ *Read set* – Read sets are an abstraction of genomics reads, which are stored in FASTQ, BAM, or CRAM formats\. Read sets can be imported into sequence stores and annotated with metadata\. You can apply permissions to read sets using attribute based access control \(ABAC\)\. 
+ *Reference* – A genome reference is used with reads to identify where in a genome a specific read, or group of reads, is mapped to\. These are in FASTA format and stored in the reference store\. 
+ *Reference store* – A data store for the storage of reference genomes\. You can have a single reference store in each account and region\. 

## Analytics<a name="variant-store-concepts"></a>

You can transform and analyze your genomics data with Omics Analytics\. Create a variant store or annotation store to include additional information for your queries\.
+ *Variant Store* – data store that stores variant data at a population scale\. Variant stores support both genomic Variant Call Format \(gVCF\) and VCF inputs\. 
+ *Annotation Store* – A data store representing an annotation database, such as one from a TSV/CSV, VCF, or General Feature Format \(GFF3\) file\. Annotation Stores are mapped to the same coordinate system as variant stores during an import\. 

## Workflows<a name="workflows-concepts"></a>

With Omics Workflows, you can process and analyze your genomics data\.
+ *Workflow* – The overall definition of an end to end process including parameters and references to tools\. Workflow definitions can be expressed as WDL or Nextflow\. Each created workflow will have a unique identifier\. 
+ *Run/Workflow Run* – A single invocation of a workflow\. An individual run uses your defined input data and produces an output\. Each created run will have a unique identifier\. 
+ *Task* – The individual processes within a run\. Omics Workflows will use these defined compute specifications to run your task\. Each task will have a unique identifier\. 
+ *Run Group* – A group of runs for which you can set the max vCPU, max duration, or max concurrent runs to help limit the compute resources used per run\. You can specify and configure priorities for your workflow runs within a run group\. For example, you can specify that a high priority run will be performed before one that is lower priority, creating a priority queue\. It is optional to use a Run Group, and each Run Group will have a unique identifier\.