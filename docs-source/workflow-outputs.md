# Nextflow Workflow outputs<a name="workflow-outputs"></a>

Workflows written in Nextflow require a **publishDir** directive to export the task contents to your output S3 bucket\. Workflow written in WDL don't support output\. Use it every time any of the task outputs need to be kept on workflow run completion\. It also must point to the folder in the following example, **“/mnt/workflow/pubdir”**\. For security reasons, this is the only folder authorized to export files to your Amazon S3 bucket\.

```
 nextflow.enable.dsl=2


workflow {
    CramToBamTask(params.ref_fasta, params.ref_fasta_index, params.ref_dict, params.input_cram, params.sample_name)
    ValidateSamFile(CramToBamTask.out.outputBam)
}


process CramToBamTask {
    container "(account).dkr.ecr.us-west-2.amazonaws.com/genomes-in-the-cloud"

    publishDir "/mnt/workflow/pubdir"

    input:
        path ref_fasta
        path ref_fasta_index
        path ref_dict
        path input_cram
        val sample_name

    output:
        path "${sample_name}.bam", emit: outputBam
        path "${sample_name}.bai", emit: outputBai

    script:
    """
        set -eo pipefail

        samtools view -h -T $ref_fasta $input_cram |
        samtools view -b -o ${sample_name}.bam -
        samtools index -b ${sample_name}.bam
        mv ${sample_name}.bam.bai ${sample_name}.bai
    """
}


process ValidateSamFile {
    container "(account).dkr.ecr.us-west-2.amazonaws.com/genomes-in-the-cloud"

    publishDir "/mnt/workflow/pubdir"

    input:
        file input_bam

    output:
        path "validation_report"

    script:
    """
        java -Xmx3G -jar /usr/gitc/picard.jar \
        ValidateSamFile \
        INPUT=${input_bam} \
        OUTPUT=validation_report \
        MODE=SUMMARY \
        IS_BISULFITE_SEQUENCED=false
    """
}
```