# Omics Analytics<a name="gs-console-analytics"></a>

With Omics Analytics, you can create variant stores and annotation stores\. After you have created the stores and imported your data, you can explore and analyze your stores using analytics engines, such as Amazon Athena\.

## Create a variant store<a name="gs-console-create-variant-store"></a>

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Variant stores**\.

1. On the **Create variant store** page, provide the following information
   + **Variant store name** \- A unique name for this store\. 
   + **Description** \(optional\) \- A description of this variant store\.
   + **Reference genome** \- The reference genome for this variant store\.
   + **Data Encryption** \- Choose whether you want data encryption to be owned and managed by AWS or by yourself\. 
   + **Tags** \(optional\) \- Provide up to 50 tags for this variant store\.

1. Choose **Create variant store**\.

## Create an annotation store<a name="gs-console-create-annotation-store"></a>

**To create an annotation store**

1. Open the Amazon Omics console [https://console\.aws\.amazon\.com/omics/](https://console.aws.amazon.com/omics/)\.

1. On the Amazon Omics home page, choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/omics/latest/dev/images/menuNavPane.png) in the upper left corner of the screen to open the navigation pane\. Select **Annotation stores**\.

1. On the **Annotation stores** page, choose **Create annotation store**\.

1. On the **Create annotation store** page, provide the following information
   + **Annotation store name** \- A unique name for this store\. 
   + **Description** \(optional\) \- A description of this reference genome\.
   + **Data format and schema details** \- Select data file format and upload the schema definition for this store\.
   + **Reference genome** \- The reference genome for this annotation\.
   + **Data Encryption** \- Choose whether you want data encryption to be owned and managed by AWS or by yourself\. 
   + **Tags** \(optional\) \- Provide up to 50 tags for this annotation store\.

1. Choose **Create annotation store**\.