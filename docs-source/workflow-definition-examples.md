# Workflow definition examples in WDL and Nextflow<a name="workflow-definition-examples"></a>

The following examples are workflow definitions for converting from CRAM to BAM in both WDL and Nextflow\. Because the parameters are the same, both workflows can be started using the same command\. The cram\-to\-bam workflow defines two tasks and uses tools from the “genomes\-in\-the\-cloud” container, which shown in the example and is publicly available\. Note that Omics workflows require ECR containers to be in the same account and Region as the account calling the service\. ECR containers should be included as parameters in your workflow to validate access\. 

To allow Omics to access the ECR container, add the following policy to your account in the section covering ECR Repository permissions\. 

```
{
  "Version": "2008-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "omics.amazonaws.com"
      },
      "Action": [
        "ecr:BatchGetImage",
        "ecr:GetDownloadUrlForLayer"
      ]
    }
  ]
}
```

You can include the ECR container as a parameter by including it in your workflow as shown\. This is recommended so that the access permissions to your image are checked when you start the run\. The following file defines all parameters for your workflow\. 

```
{
    "input_cram": "s3://DOC-EXAMPLE-BUCKET1/inputs/NA12878.cram",
    "ref_dict": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.dict",
    "ref_fasta": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta",
    "ref_fasta_index": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta.fai",
    "sample_name": "NA12878",

     "gotc_docker":"public.ecr.aws/aws-genomics/broadinstitute/genomes-in-the-cloud:2.4.7-1603303710"
}
```

Then specify which files to use in your run\. The following example is for when your files are stored in an Amazon S3 bucket\.

```
{
    "input_cram": "s3://DOC-EXAMPLE-BUCKET1/inputs/NA12878.cram",
    "ref_dict": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.dict",
    "ref_fasta": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta",
    "ref_fasta_index": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta.fai",
    "sample_name": "NA12878"
}
```

If you want to specify files from a sequence store, indicate that as shown in the following example, using the URI for the sequence store\.

```
{
    "input_cram": "omics://429915189008.storage.us-west-2.amazonaws.com/111122223333/readSet/4500843795/source1",
    "ref_dict": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.dict",
    "ref_fasta": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta",
    "ref_fasta_index": "s3://DOC-EXAMPLE-BUCKET1/inputs/Homo_sapiens_assembly38.fasta.fai",
    "sample_name": "NA12878"
}
```

You can then define your workflow in WDL as shown in the following\.version 1\.0 workflow CramToBamFlow \{ input \{ File ref\_fasta File ref\_fasta\_index File ref\_dict File input\_cram String sample\_name String gotc\_docker = "\(account\)\.dkr\.ecr\.us\-west\-2\.amazonaws\.com/genomes\-in\-the\-cloud:latest" \} \#converts CRAM to SAM to BAM and makes BAI call CramToBamTask\{ input: ref\_fasta = ref\_fasta, ref\_fasta\_index = ref\_fasta\_index, ref\_dict = ref\_dict, input\_cram = input\_cram, sample\_name = sample\_name, docker\_image = gotc\_docker, \} \#validates Bam call ValidateSamFile\{ input: input\_bam = CramToBamTask\.outputBam, docker\_image = gotc\_docker, \} \#Outputs Bam, Bai, and validation report to the FireCloud data model output \{ File outputBam = CramToBamTask\.outputBam File outputBai = CramToBamTask\.outputBai File validation\_report = ValidateSamFile\.report \} \} \#Task Definitions task CramToBamTask \{ input \{ \# Command parameters File ref\_fasta File ref\_fasta\_index File ref\_dict File input\_cram String sample\_name \# Runtime parameters String docker\_image \} \#Calls samtools view to do the conversion command \{ set \-eo pipefail samtools view \-h \-T \~\{ref\_fasta\} \~\{input\_cram\} \| samtools view \-b \-o \~\{sample\_name\}\.bam \- samtools index \-b \~\{sample\_name\}\.bam mv \~\{sample\_name\}\.bam\.bai \~\{sample\_name\}\.bai \} \#Run time attributes: runtime \{ docker: docker\_image \} \#Outputs a BAM and BAI with the same sample name output \{ File outputBam = "\~\{sample\_name\}\.bam" File outputBai = "\~\{sample\_name\}\.bai" \} \} \#Validates BAM output to ensure it wasn't corrupted during the file conversion task ValidateSamFile \{ input \{ File input\_bam Int machine\_mem\_size = 4 String docker\_image \} String output\_name = basename\(input\_bam, "\.bam"\) \+ "\.validation\_report" Int command\_mem\_size = machine\_mem\_size \- 1 command \{ java \-Xmx\~\{command\_mem\_size\}G \-jar /usr/gitc/picard\.jar \\ ValidateSamFile \\ INPUT=\~\{input\_bam\} \\ OUTPUT=\~\{output\_name\} \\ MODE=SUMMARY \\ IS\_BISULFITE\_SEQUENCED=false \} runtime \{ docker: docker\_image \} \#A text file is generated that will list errors or warnings that apply\. output \{ File report = "\~\{output\_name\}" \} \} 