# Why can't I import my BAM, CRAM or FASTQ files?<a name="import-troubleshooting"></a>

Ensure that you have the appropriate service role\. This role must be able to read from the Amazon S3 location\(s\) where your data resides and to use the AWS KMS decrypt permissions if using a customer managed key\(CM\-CMK\)\. It must also have the appropriate trust policy for Omics to assume the role\.