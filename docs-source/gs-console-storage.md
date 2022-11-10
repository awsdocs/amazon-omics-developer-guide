# Amazon Omics Storage<a name="gs-console-storage"></a>

With Omics Storage, you can create a sequence or reference store, import a reference genome, and import genomics files\. After you have created your stores and imported your genomic data, you can access and analyze your sequence data\. 

## Create a reference store<a name="gs-console-create-reference-store"></a>

**To create a reference store**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose **Get started with Omics** and then choose **Reference genomes** from the Genomics data storage options\.

1. You can either choose a previously imported reference genome or import a new one\. If you haven't imported a reference genome, select **Import reference genome** in the top right\.

1. On the **Create reference genome import job** page, choose either the **Quick create** or **Manual create** option to create a reference store, and then provide the following information\.
   + **Reference genome name** \- A unique name for this store\. 
   + **Description** \(optional\) \- A description of this reference store\.
   + **IAM Role** \- Select a role with access to your reference genome\. 
   + **Reference from Amazon S3** \- Select your reference sequence file in an Amazon S3 bucket\.
   + **Tags** \(optional\) \- Provide up to 50 tags for this reference store\.

## Create a sequence store<a name="gs-console-create-sequence-store"></a>

**To create a sequence store**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Reference stores**\.

1. On the **Create sequence store** page, provide the following information
   + **Sequence store name** \- A unique name for this store\. 
   + **Description** \(optional\) \- A description of this sequence store\.
   + **Data Encryption** \- Select whether you want data encryption to be owned and managed by AWS or to use a customer managed CMK\. 
   + **Tags** \(optional\) \- Provide up to 50 tags for this sequence store\.

## Import genomics files<a name="gs-console-import-genomics-files"></a>

**To import a genomics file**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Sequence stores**\.

1. On the **Sequence stores** page, choose the sequence store that you want to import your files into\.

1. On the individual sequence store page, choose **Import genomic files**\.

1. On the **Specify import details** page, provide the following information
   + **IAM role** \- The IAM role that can access the genomic files on S3\.
   + **Reference genome** \- The reference genome for this genomics data\.

1. On the **Specify import manifest** page, specify the following information **Manifest file**\. The manifest file is a JSON or YAML file that describes essential information of your genomics data\. For information about the manifest file, see [Sequence store imports](import-sequence-store.md)\.

1. Click **Create import job**\.